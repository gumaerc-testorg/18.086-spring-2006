---
content_type: page
title: Study Materials
uid: af075064-5d4b-3d59-a9b9-10404ec68fc9
---

Papers
------

Chan, Tony, and Henk A. Van der Vorst. "[Approximate and Incomplete Factorizations](https://doi.org/10.1007/978-94-011-5412-3_6)."

Benzi, M., G. H. Golub, and J. Liesen. "Numerical Solution of Saddle Point Problems." _Acta Numerica_ 14 (2005): 1-137. (To appear.) (![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[PDF - 1.0 MB](http://www.mathcs.emory.edu/~benzi/Web_papers/bgl05.pdf))

MATLAB® Documentation
---------------------

[MATLAB® Technical Documentation](http://www.mathworks.com/access/helpdesk/help/techdoc/matlab.shtml)

[Documentation for MathWorks Products, R2006a](https://www.mathworks.com/help/doc-archives.html)

[MATLAB® Mathematics Documentation](https://www.mathworks.com/help/matlab/)

Other Thoughts
--------------

Is there a simple way to count the fill-in in elimination?

Run chol (or lu) and use "nnz" (number of non-zeros)

% Generate discrete Laplacian operator  
A=delsq(numgrid('S',100));  
% Cholesky factorize  
R=chol(A);  
nnz(R)  
ans = 941289  
  
% As above, but permute rows & columns by approximate minimum degree  
p=symamd(A);  
R=chol(A(p,p));  
nnz(R)  
ans = 184917

Does min degree just take nodes a row at a time (if it breaks ties correctly)? If that is N^3 - and would sparse backslash do it?

Yes it does, it is a "greedy algorithm." The approximate one might be a little different, but in principle it does the same thing. Sparse backslash does this automatically. If symmetric, then it uses "symmmd" (symmetric minimum degree), then "chol." If non-symmetric, "colamd" (column approximate minimum degree), then umfpack.

This is about as fast as possible except for Fast Poisson Solvers?

Well, it is probably as fast as possible for Gaussian elimination. But iterative solvers do better, I think CG with Incomplete Cholesky should be N^(2.5). Then of course fast solvers, FFT-based and multigrid.

Develop a code for **Incomplete** LU and try it on the 2D matrix for increasing N. You can make it a general code or specialize it to these particular matrices (and decide which entries to keep in the approximate factors L and U). Try on A2D u = random right side. You could use eig to find the spectral radius of the preconditioned matrix B. That matrix is I - inv(P)A2D where P ≈ LU. Of course if P = A2D then B = 0 and convergence in 1 step.