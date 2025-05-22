# OpenMP在并行构造中的运用 #
## 1.常用指令详解 ##
**for:**
-  `#pragma omp parallel for`后跟 for：parallel 指令后通常跟 for 循环。
- `单入口单出口`：并行区域应保持单入口单出口，`break 和 goto` 语句也应在循环内。
- 不存在数据相关，也不存在数据竞争：在并行执行时，确保数据独立性，避免数据竞争。
**数据竞争:**
- 数据竞争是在 parallel 区域由两个以上的线程对相同的共享变量同时进行写操作而引起的。
- 避免数据竞争的方法包括使用 `private`、`firstprivate`、`lastprivate` 等子句。
**collapse 指令**
- `collapse` 指令可以将一个多重循环合并后，展开成为一个更大的循环，从而增加线程组上划分调度的循环总数。
- 示例代码：
  ```c
  #pragma omp parallel for collapse(2) shared(a,b,c,M,N,P) private()
  for(int i=0;i<M;i++)        
     for(int k=0;k<P;k++) -------->M*P
       for(int j=0;j<N;j++>)
         c[i,k]=a[i]+b[i,j*b[i,k]];
  ```
**分段并行 (Sections)**  
用于非循环的代码并行。若不同子任务之间不存在相互依赖的关系，可以将他们分配给不同的线程去执行。
```c
#pragma omp sections [private(firstprivate lastprivate reduction nowait)]
{
    #pragma omp section
    结构块
    #pragma omp section
    结构块
}
```
**Single**
- 在 single 后面会有一个隐含的屏障，因此在 single 部分执行期间，其他线程处于空闲状态。
- single 只能由一个线程来执行，但是并不要求主线程。
- 不执行的其余线程会在 single 结束后同步，如果指令存在 nowait，则不执行 single 的其余线程可以直接越过 single 结构向下执行。
```c
#pragma omp single [private firstprivate copyprivate nowait]
{
    结构块
}
```
## 2.私有变量 ##
**private 子句：**
private 子句可以将变量列表里的变量声明为线程组中子线程的私有变量。并行区域内每个线程只能访问自己的私有副本，无法访问其他线程的私有副本
``` c
int i = 100;
float a = 111.11;
omp_set_num_threads(4);
#pragma omp parallel private(i,a)
{
    i=i+1;                                      
    print(thread_number, i , a);--------------->i=1,a=0.00000
}
print(i);                ---------------------->i=100
```
**firstprivate 子句:**  
让私有变量继承原始变量的值，可以通过 firstprivate 子句来实现。  
**lastprivate 子句**  
将一个或多个变量声明为线程的私有变量,在并行处理结束后将这些变量的值传递给并行区域外的原始变量。
## 规约操作 - reduction ##
在并行区域中对变量进行累加、求差、求积等运算