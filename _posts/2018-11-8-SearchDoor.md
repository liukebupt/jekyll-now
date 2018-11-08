---
layout: default
comments: true
title: Search Door Problem
---
## Search Door Problem
Imagine that you are facing a high wall that stretches infinitely in both directions. There is a door in the wall, but you don't know how far away is the door or in which direction. It is pitch dark, but you have a very dim lighted candle that will enable you to see the door when you are right next to it.   

1. Show an algorithm that enables you to find the door by walking at most $$O(n)$$ steps in the worst case, where $$n$$ is the number of steps that you would have taken if you knew where the door is and walked directly to it (note that your algorithm does **not** know the value of $$n$$ in advance)

2. What is the constant multiple in the worst-case analysis for your algorithm?

3. Observe that if you knew $$n$$, you could trivially solve the problem in $$3n$$ steps in the worst case (since you do not know the direction of the door); an interesting question (beyond the scope of this problem) is: what the minimum constant $$c>3$$ that can be achieved by a linear-time algorithm (in the worst case)?

### Q1  
We have two approaches for this problem.  

1. 
```
initialize a to some number m
while not find door:
     walk left a steps
     a*=2
     walk right a steps
     a*=2
```
* Install BLAS
     > * Download latest version from http://www.netlib.org/blas/.  
     > * Untar   
     > * `$cd BLAS-*/`  
     > * `$make`  
     > * get lib `blas*.a`  
* Download latest version from http://www.netlib.org/lapack/.
* Untar
* `$cd lapack-*/`
* `$cp make.inc.example make.inc` and change line 81 to point to `blas*.a`
* `$make lib`
* `$make lapackelib`
* `$make cblaslib`
* Move files in `LAPACKE/include/` and `CBLAS/include/` to some `include/` you are familiar with. 
* Move 'lapacke', 'lapack', 'blas', 'cblas' librarys to some `lib/` you are familiar with, and rename them as `liblapacke.a`, `liblapack.a`, `libblas.a`, `libcblas.a`.

### Test
* Get test program from http://www.netlib.org/lapack/lapacke.html.
* Compile with `$gcc test_lapack.c -L lib/ -llapacke -llapack -lcblas -lblas -lgfortran -lm -I include/ -o test_lapack` 
      
### Documentation
* http://www.netlib.org/lapack/explore-html/index.html.
* This documentation have specific defination for all lapack functions. But, if you want to use some function 'f()' in lapack, you should use 'LAPACKE_f()' or 'cblas_f()', you can see parameters of 'LAPACKE_f()' or 'cblas_f()' in this documentation as well, but there are no details. It might be hard to find how to use 'LAPACKE_f()' or 'cblas_f()' properly, one possible way is to test different combinations of parameters especially for parameters 'N', 'M', 'LDA', 'LDB'.

### Functions
`#include <lapacke.h>`for lapacke functions, `#include <cblas.h>`for cblas functions  
- cblas_dtrsm  
`void cblas_dtrsm (const CBLAS_LAYOUT layout, const CBLAS_SIDE Side, const CBLAS_UPLO Uplo, const CBLAS_TRANSPOSE TransA, const CBLAS_DIAG Diag, const int M, const int N, const double alpha, const double \*A, const int lda, double \*B, const int ldb)`   
     > - `layout`: `CblasRowMajor` or `CblasRowMajor`   
     > - `Side`: `CblasLeft` or `CblasRight`, same meaning as `SIDE` in `dtrsm()`  
     > - `Uplo`: `CblasLower` or `CblasUpper`, same meaning as `UPLO` in `dtrsm()`  
     > - `TransA`: `CblasNoTrans` or `CblasTrans`, same meaning as `TRANSA` in `dtrsm()`  
     > - `Diag`: `CblasNonUnit` or `CblasUnit`, same meaning as `DIAG` in `dtrsm()`  
     > - `alpha`: same meaning as `ALPHA` in `dtrsm()`  
     > - `A`: same meaning as `A` in `dtrsm()`  
     > - `B`: same meaning as `B` in `dtrsm()`  
If you are using `CblasRowMajor`, and `A` is a square matrix:  
     > - `M`: length of each edge of `A`  
     > - `N`: number of columns in `B`  
     > - `LDA`: the same as `M`  
     > - `LDB`: the same as `N`  

### FAQ
* What is LDA? (http://icl.cs.utk.edu/lapack-forum/viewtopic.php?f=2&t=1897)
> *On LDA, the doc says:
>
> * LDA (input) INTEGER
> * The leading dimension of the array A. LDA >= max(1,N).
>
> Now N is the number of columns in the input matrix. But the input matrix is symmetric, so we'd expect it to be N x N, right? In that case LDA = N. The ">=" part in the comment just allows for the possibility that the input matrix A is in fact part of a larger array, with a greater number of rows than are actually used in this function. But unless you're doing something tricksy, "LDA" is just the rows of A, and equals N.  
  
  Understunding of mine: While storing a matrix `A` in memory, the jth element of ith row `A[i][j]` can also be `A[i*n+j]`, which means the (i\*n+j)th element in `A`, where `n` is the number of elements in each row. However, while you are using part of a matrix to compute, for example part of `A`, `A'`, `A'[i][j]` is the (i\*n+j)th element of `A'`, but `n` is no longer the number of elements in each row of `A'`. As a result, you need to tell the computer `A'` is part of a larger matrix `A` using `LDA=n`.   
    
  In conclution, `LDA` is the number of elements in each row of the matrix you stored in memory.  


{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://blog-of-ke-liu.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
{% endif %}
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                            
