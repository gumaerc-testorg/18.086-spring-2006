---
content_type: page
description: The study material section discusses about papers, matlab documentation
  and other thoughts.
draft: false
learning_resource_types: []
ocw_type: CourseSection
title: Study Materials
uid: af075064-5d4b-3d59-a9b9-10404ec68fc9
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---
## Papers

Chan, Tony, and Henk A. Van der Vorst. "{{% resource_link "88367af1-22ec-412e-9e79-e329d89e3477" "Approximate and Incomplete Factorizations" %}}."

Benzi, M., G. H. Golub, and J. Liesen. "{{% resource_link "53bde6d0-2fd6-4254-99e4-f767120060f9" "Numerical Solution of Saddle Point Problems" %}}." *Acta Numerica* 14 (2005): 1-137. 

## MATLAB® Documentation

{{% resource_link "aa13fe9c-a209-42a1-ae7b-1e841bc81dd7" "MATLAB® Technical Documentation" %}}

{{% resource_link "d959624b-ad48-419d-9641-02684dea91c9" "Documentation for MathWorks Products, R2006a" %}}

{{% resource_link "ae51850f-941c-4501-8ca5-91964351d5bf" "MATLAB® Mathematics Documentation" %}}

## Other Thoughts

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