---
layout: post
title: "論筆記－演算法"
catogories: [演算法]
tags: [algorithm, 論筆記]
image_description: false
description: 演算法的心得。
comments: true
published: true
---
### <font color="green">implement algorithm</font>

#### <font color="orange"> 1. Sequential Search</font>



**Problem:** Is the key *x* in the array S of *n* keys.
**Inputs**(parameters): positive integer *n*, array of keys S indexed from 1 to *n*, and a key *x*.
**Outputs:** *location*, the location of *x* in *S* (0 if *x* is not in *S*).



```clike=
void seqsearch(int n,
                const keytype S[ ],
                keytype x,
                index& location)
{
     location = 1;
     while (location <= n && S[location] != x)
         location++;
     if(location > n)
        location = 0;
}
```
```clike=
#include <stdio.h>

void seqsearch(int, const int*, int);

int main(){
    int n1, S1[10],x1, location1;
    int i;
    printf("how many elements in S? ");
    scanf("%d", &n1);
    printf("ok, fill in the elements' value. \n");
    for(i=0;i<n1;i++){
        scanf("%d", &S1[i]);
    }
    printf("You want to search:");
    scanf("%d", &x1);
    seqsearch(n1,&S1[0],x1);
    return 0;
}

void seqsearch(int n2, const int* S2, int x2){
    int location = 0;
    while(location < n2 && S2[location] != x2)
        location++;
    if(location == n2)
        location = 0;
    if(location == 0){
        printf("no\n, this number is not in S");
    }
    else {
        printf("yes, %d is in S and is at %d", x2, location);
    }
}
```
<br>
<br>
<br>

#### <font color="orange">2. Add Array Members</font>


**Problem:** Add all the numbers in the array *S* of *n* numbers.
**Inputs:** positive integer *n*, array of numbers *S* indexed from 1 to *n*.
**Outputs:** *sum*, the sum of the numbers in *S*.

```clike=
number sum (int n, const number S[ ])
{
    index i;
    number result;
    
    result = 0;
    for( i=1 ; i<= n ; i++)
        result = result + S[i];
    return result;
}
```
```clike=
#include <stdio.h>

int sum(int, const int *);

int main(){
    
    int n1,S1[11];
    int i,result2;
    printf("Today we want to add all the numbers in the array S.\n===\n");
    printf("how many elements you want to fill in S? Don't exceed 10\n");
    scanf("%d", &n1);
    printf("ok, what number you want to fill in?\n");
    for(i=1 ; i<=n1 ; i++)
        scanf("%d", &S1[i]);
    printf("start to add all the numbers in S~\n");
    result2 = sum(n1,&S1[0]);
    printf("sum S is %d\n", result2);
    return 0;
}

int sum(int n2, const int* S2){
    int i;
    int result=0;
    for(i=1;i<=n2;i++)
        result = result + S2[i];
    return result;
}
```
<br>
<br>
<br>


#### <font color="orange">3. Exchange Sort</font>

**Problem:** Sort *n* keys in nondecreasing order.
**Inputs:** positive integer *n*, array of keys *S* indexed from 1 to *n*.
**Outputs:** the array *S* containing the keys in nondecreasing order.

small to big.

```clike=
void exchangesort(int n, keytype S[])
{
    index i, j;
    for( i=1 ; i<=n ; i++ )
        for( j=i+1 ; j<=n ; j++ )
            if (S[j] < S [i])
                exchange S[i] and S[j];
}
```
```clike=
#include <stdio.h>

void exchangesort(int, int*);

int main(){

    int n1, S1[21];
    int i;
    printf("Today we want to sort an array from small to big.\n");
    printf("Let's go\n");
    printf("how many elements you want to sort? Please Don't exceed 20.\n");
    scanf("%d", &n1);
    printf("ok, here I say again..enter the elements' value you want to be sorted\n");
    for(i=1;i<=n1;i++)
        scanf("%d", &S1[i]);
    printf("exchange for you 啦\n");
    exchangesort(n1,&S1[0]);
    return 0;
}

void exchangesort(int n2, int* S2){
    int i,j;
    int temp;
    for( i=1 ; i<=n2 ; i++)
        for( j=i+1 ; j<=n2 ; j++)
            if(S2[j] < S2[i]){
                temp = S2[i];
                S2[i] = S2[j];
                S2[j] = temp;
            }
    for(i=1;i<=n2;i++)
        printf("%d ", S2[i]);
    printf("\n");
}
```
<br>
<br>
<br>

#### <font color="orange">4. Matrix Multiplication</font>

**Problem:** Determine the product of two *n* × *n* matrices.
**Inputs:** a positive integer n, two-dimensional arrays of numbers *A* and *B*, each of which has both its rows and columns indexed from 1 to *n*.
**Outputs:** a two-dimensional array of numbers *C*, which has both its rows and columns indexed from 1 to *n*, containing the product of *A* and *B*.
```clike=
void matrixmult (int n,
                 const number A[][],
                 const number B[][],
                       number C[][])
{
    index i,j,k;
    
    for( i=1 ; i<=n ; i++)
        for (j=1;j<=n;j++){
            C[i][j] = 0;
            for(k=1;k<=n;k++)
                C[i][j] = C[i][j]+A[i][k]*B[k][j];
        }
}
```
```
I can't implementation it because when I try to send 2-dimentional array into
subfunction, it's always wrong. I have tried to send the pointer but I don't know
the algorithm of this way...
```
<br>
<br>
<br>

#### <font color="orange">5. Binary Search</font>

**Problem:** Determine whether *x* is in the sorted array *S* of *n* keys.
**Inputs:** positive integer *n*, sorted (nondecreasing order) array of keys *S* indexed from 1 to *n*, a key *x*.
**Outputs:** *location*, the location of *x* in *S* (0 if *x* is not in *S*).
```clike=
void binsearch (int n,
                const keytype S[],
                keytype x,
                index& location)
{
    index low, high, mid;
    
    low = 1; high = n;
    location = 0;
    while(low<=high && location==0){
        mid= ⌊ (low+high)/2 ⌋;
        if(x==S[mid])
            location = mid;
        else if (x<S[mid])
            high=mid-1;
        else 
            low=mid-1;
    }
}
```
```
I can't implementation it. :.( 
```
<br>
<br>
<br>

#### <font color="orange">6. *n*th Fibonacci Term (Recursive)</font>

**Problem:** Determine the *n*th term in the Fibonacci sequence.
**Inputs:** a nonnegative integer *n*.
**Outputs:** *fib*, the *n*th term of the Fibonacci sequence.

```clike=
int fib (int n){
    if(n<=1)
        return n;
    else
        return fib(n-1) + fib(n-2);
}
```
```clike=
#include <stdio.h>

int fib(int);

int main(){
    int choice, fib_nu;
    printf("\n************\nChoose a number~\n");
    printf("You choose: ");
    scanf("%d", &choice);
    fib_nu = fib(choice);
    printf("See! the %dth Fibonacci number is %d\n", choice, fib_nu);
    printf("\nSee ya~~~\n\n");
    return 0;
}

int fib(int n){
    if( n<= 1) return n;
    else return fib( n-1 ) + fib( n-2 );
}
```
<br>
<br>
<br>

#### <font color="orange">7. *n*th Fibonacci Term (Iterative)</font>

**Problem:** Determine the *n*th term in the Fibonacci sequence.
**Inputs:** a nonnegative integer *n*.
**Outputs:** *fib*2, the *n*th term in the Fibonacci sequence.
```clike=
int fib2(int n){
    index i;
    int f[0..n];

    f[0]=0;
    if(n>0){
        f[1]=1;
        for(i=2;i<=n;i++)
            f[i] =  f[i-1] + f[i-2];
    }
    return f[n];
}
```
```clike=
#include <stdio.h>

int fib2 (int);

int main(){
    int choice, fib_nu;
    printf("\n******\nChoose a number~\n");
    printf("You choose: ");
    scanf("%d", &choice);
    fib_nu = fib2(choice);
    printf("%dth Fibonacci number is %d\n", choice, fib_nu);
    return 0;
}

int fib2 (int n){
    int i;
    int f[n];
    
    f[0] = 0;
    if( n>0 ){
        f[1] = 1;
        for( i=2 ; i<=n ; i++ )
            f[ i ] = f[ i-1 ] + f[ i-2 ];
    }
    return f[n];
}
```
<br>
<br>
<br>

### <font color="green">Exercises</font>

##### 8. Under what circumstances, when a searching operation is needed, would Sequential Search (Algorithm 1.1) not be appropriate?
```
在要搜尋的數量很龐大，而且所要尋找的值恰好在很後面。
...是說我沒找我要怎麼知道它在很後面?= =
```
##### 9. Give a practical example in which you would not use Exchange Sort (Algorithm 1.3) to do a sorting task.
```
實際的例子...當我想要從小排到大時，有一個從1000000000000開始，公差為1的數列來到我的面前時。(-ω-)
```
##### 10. Define basic operations for your algorithms in Exercises 1-7, and study the performance of these algorithms. If a given algorithm has an every-case time  complexity, determine it. Otherwise, determine the worst-case time complexity.
```

```

>#### 時間複雜度分析
>因為這樣那樣很多原因，簡化並統一了分析演算法的效率方式，在這裡先只包含
>1. input size 輸入值大小，比如一個整數陣列宣告了10，那它的大小就是10
>2. basic operation 基本運算，算出這個演算法完成要幾次基本運算
    包含
    + 比較指令 comparison instruction
    + 賦值指令 assignment instruction
>>> 所以基本運算是指這個演算法最核心的那個指令嗎= =?
>
>##### Every-Case Time Complexity 所有情況下的時間複雜度
>T(n) 被定義為
>已知1.input size大小為n
>則T(n)會被賦值"演算法的基本運算次數"
>也就是T(n) = "演算法的'基本運算'次數"
>所以T(n)被叫做"演算法的所有情況下的時間複雜度"
>然後我們要做的就是決定T(n)的值，這個決定的名稱則是叫做"所有情況下的時間複雜度分析"
>*好饒舌= =*
>>>不能理解我剛剛寫了什麼...
>##### Worst-Case Time Complexity 糟糕情況下的時間複雜度
>W(n) 被定義為
>已知1.input size大小為n
>則W(n)會被賦值"這演算法的基本運算最多要做的次數"
>...以下省略
>##### Average-Case Time Complexity 一般狀況下的時間複雜度
>A(n) 被定義為
>已知1.input size大小為n
>則A(n)會被賦值"我們預期這個演算法一般會算出來的基本運算次數"
>...以下省略
>**As is the case for W(n)**，如果T(n)存在，那麼A(n) = T(n)。
>要決定A(n)的值必須要牽扯到機率，每一種可能輸入的n值的機率













