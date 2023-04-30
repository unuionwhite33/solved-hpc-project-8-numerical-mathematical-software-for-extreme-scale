Download Link: https://assignmentchef.com/product/solved-hpc-project-8-numerical-mathematical-software-for-extreme-scale
<br>



Science and Interactive Supercomputing with JupyterLab

In this project, you will learn about popular scientific mathematical software libraries and tools widely used in computational science and engineering (CSE) practice. The project is also based on programming in Python, using innovative software in scientific computing for numerical optimization, and you will get familiar with the increasingly popular Jupyter notebook, which is an incredibly powerful tool for interactively developing and presenting CSE projects. Jupyter notebook integrates code and its output into a single document that combines visualizations, narrative text, mathematical equations, and other rich media. This intuitive workflow promotes iterative and rapid development, making notebooks an increasingly popular choice at the heart of contemporary CSE science, analysis, and increasingly science at large.

In this project, we will first solve the Poisson equation using state-of-the-art scientific mathematical software libraries and, second, set up Jupyter Notebooks on your local machine and we will use it to solve a large-scale PDEconstrained optimization problem. These HPC software concepts will be practiced on an application of boundary control PDE problem with Dirichlet boundary conditions. You will implement a second-order finite-difference discretization scheme by which the elliptic control problem will be transcribed into a nonlinear programming problem. The full-space approach will be used, treating both control and state variables as optimization variables, resulting in a high-dimensional nonlinear programming problem.

<h1>1.      An HPC Computational Software Stack for Extreme-Scale CSE</h1>

<h2>1.1.     High-Level Languages for Numerical Computations</h2>

Using numerical methods and reference implementations from popular textbooks is often not sufficient for the development of serious software for large-scale applications in computational science. In general, state-of-the-art algorithms are not covered by these books and neither are the corresponding implementations suited to achieve a reasonable performance on multicores. Software packages like Mathematica [1], Maple [2], Matlab [3] provide high-level programming languages that can support the initial development of new numerical methods and the rapid prototype implementation. However, these standard packages are often not sufficient as HPC kernels, and neither are they intended for the realization of scientific software that has to deal with large-scale parallel applications on an HPC platform. A more modern example is Julia [4], which is a high-level, high-performance, dynamic programming language. While it is a general purpose language and can be used to write any application, some of its features are well suited for high-performance numerical analysis and computational science as well. It was designed with parallelism in mind, thus on top of classical multi-threading paradigm it supports asynchronous tasks and distributed computing such as MPI.

Due to the dedicated programming language of these examples, algorithms can be implemented based on a syntax which is close to mathematical notation. The expressive syntax not only allows rapid prototyping but also makes it easier to ensure the correctness of the resulting implementation. All of these high-level languages use external software libraries (e.g., BLAS [5], LAPACK [6], and other implementations, as well as libraries for numerical optimizations and PDE frameworks such as IPOPT [7] and PETSc [8]) to carry out numerical computations. Hence, they can be considered as high-level interfaces to these HPC mathematical libraries. On the one hand, Matlab and Mathematica greatly benefit from the performance provided by external libraries. On the other hand, in general, not all features of these libraries can be exploited. For instance, these packages support only two matrix types, namely dense and sparse matrices. Other matrix types, e.g., for matrices with band or block structure, that are supported by LAPACK [6], are not available in Matlab.

In conclusion, high-level languages for numerical computations are very useful, easy to learn, and popular in computational science — they can represent a layer to HPC software libraries without exposing the application developer to the multicore complexity.

<h2>1.2.     Scientific Computing Software Toolkits and Libraries</h2>

In the following text, various well-known high-performance computing mathematical software libraries that have influenced computational science and engineering are described. All parallel software toolkits can be used as an underlying layer in computational science to build successful parallel applications on multi- and many-core architectures.<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> Enabling parallel technologies in computational science must also ensure that a successful high-performance software library is fully and freely available for use throughout the scientific computing community.

<h3>Parallel Dense Matrix Software Libraries</h3>

The Basic Linear Algebra Subprogramm (BLAS) [5] specifies a set of computational routines for linear algebra operations such as vector and matrix operations. BLAS is a well-known example of a software layer which represents a de facto standard in the field of high-performance computing. All hardware vendors, such as Intel on multicores, or NVIDIA on manycores provide these BLAS routines as building blocks to ensure efficient scientific software portability.

The Linear Algebra Package, LAPACK, [6] is a software library for matrix operations such as solving systems of linear equations, least-squares problems, and eigenvalue or singular value problems. Almost all LAPACK routines make use of BLAS to perform efficient computations — LAPACK acts as an additional parallel software layer to the underlying BLAS. The motivation for the development of BLAS and LAPACK was to provide efficient and portable implementations for solving dense systems of linear equations and least-squares problems and it was initiated in the 1970s. Influenced by the changes in the hardware architectures (e.g., vector computers, cache-based processors to parallel multiprocessing architectures), it changed from the original specification to, finally, BLAS Level-3 [9] for matrix-matrix operations.

ScaLAPACK (Scalable LAPACK) is an extension of LAPACK that is targeted to multiprocessing computers with a distributed-memory hierarchy. Fundamental building blocks of ScaLAPACK are the parallel BLAS (PBLAS) and the Basic Linear Algebra Communication Subprograms (BLACS). PBLAS is a distributed-memory version of BLAS, and BLACS is a library for handling interprocessor communication. The design policy of ScaLAPACK was to have the ScaLAPACK routines similar to their LAPACK equivalents so that both libraries can be used as a computational software layer for application developers.

<h3>Parallel Sparse Matrix Software Toolkits</h3>

A number of applications give rise to large-scale sparse linear systems that are becoming exceedingly difficult to handle by standard numerical linear algebra techniques such as 3D wave propagation problems or PDE-constrained optimization problems. A number of highly efficient parallel software tools have been developed within the last fifteen years that can be used to tackle large-scale systems of linear equations or eigenvalue problems in parallel. These tools usually make heavy use of the BLAS and LAPACK routines, and all these parallel software tools now represent a de facto standard in the field of high-performance computing. The following is a list of the most widely use of the tools.

<ul>

 <li>The Multifrontal Massively Parallel Solver (MUMPS) [10] is being developed at CERFACS, ENSEEIHT-IRIT, and INRIA. It is a package for solving systems of linear equations of the form <em>Ax </em>= <em>b</em>, where <em>A </em>is a square sparse matrix that can be either unsymmetric, symmetric positive definite, or general symmetric. MUMPS is a direct method based on a multifrontal direct factorization. It exploits both parallelism arising from sparsity in the matrix <em>A </em>and from dense factorizations kernels. MUMPS offers several built-in ordering algorithms to some external parallel ordering packages such as METIS [11]. The parallel version of MUMPS requires MPI for message passing and makes use of the BLAS, BLACS, and ScaLAPACK libraries.</li>

 <li>SuperLU [12] is a general purpose library for the direct solution of large, sparse, nonsymmetric systems of linear equations on high-performance machines. It has been developed at the Computer Science Department of the University of California, Berkeley and at the National Energy Research Scientific Computing Center (NERSC). The library is written in C and is callable from either C or C++. The library routines will perform an LU decomposition with partial pivoting and triangular system solves through forward and back substitution. The matrix columns may be preordered (before factorization) either through library or user-supplied routines. SuperLU comes in three different flavors: SuperLU for sequential machines, SuperLU MT for shared memory parallel machines, and SuperLU DIST for highly parallel distributed-memory architectures. The parallel version of SuperLU also requires MPI for the message-passing layer and makes use of the BLAS, BLACS, and ScaLAPACK libraries.</li>

 <li>PARDISO [13] is a thread-safe, high-performance, robust, memory efficient, easy-to-use software for solving large sparse symmetric and nonsymmetric linear systems of equations on multicore processing architectures. The solver is also part of the Intel Math Kernel Library with interfaces to Matlab, C, C++, and Python. It is available from <a href="https://www.pardiso-project.org/">https://www.pardiso-project.org/.</a></li>

 <li>The FEAST library package [14] represents a unified framework for solving various families of eigenvalue problems and achieving accuracy, robustness, high-performance and scalability on parallel architectures. Its originality lies with a new transformative numerical approach to the traditional eigenvalue algorithm design the FEAST algorithm. The FEAST algorithm is a general purpose eigenvalue solver which takes its inspiration from the density-matrix representation and contour integration technique in quantum mechanics. FEAST can be used for solving both standard and generalized forms of the Hermitian or non-Hermitian problems (linear or nonlinear), and it belongs to the family of contour integration eigensolvers. FEAST’s main computational task consists of a numerical quadrature computation that involves solving independent linear systems along a complex contour, each with multiple right-hand sides. FEAST is both a comprehensive library package, and an easy to use software. It includes flexible reverse communication interfaces and ready to use driver interfaces for dense, banded, and sparse systems.</li>

</ul>

<h3>Software Toolkits for Parallel Large-Scale Nonlinear Optimization</h3>

IPOPT [7] is a C++ open-source software package for large-scale nonlinear optimization based on a Newton-based interior point algorithm with filter line-search method. It has been designed for equality constrained problems and treats upper and lower bounds for the variables with an interior penalty formulation as a sequence of barrier problems for a decreasing sequence of barrier parameters converging to zero. It can be shown that under mild regularity assumptions the solutions of the barrier problems will converge to the solution of the original problem. The IPOPT algorithm has been shown to be globally and superlinearly convergent under weaker assumptions than other barrier methods. IPOPT has been applied to thousands of test problems and applications and has been adopted by a widespread user community. A key advantage is its ability to use second derivative information efficiently. IPOPT makes use of PARDISO or MUMPS, so it can also require MPI for message passing and makes use of the BLAS, BLACS, and ScaLAPACK libraries. Carl Laird and Andreas Wchter are the developers of IPOPT. W¨ achter and Laird were awarded the 2011 J.¨ H. Wilkinson Prize for Numerical Software for this development.

<h3>Software Toolkits for Parallel Partitioning, Load Balancing, and Data-Management Services</h3>

Parallel partitioning, load balancing, and data-management services are important enabling technologies for parallel computing. Their goal is to distribute work evenly to processors while creating decompositions that have low communication costs for applications. This distribution must be done statically as a first step in most parallel computations. For adaptive computations, where processor workloads change as computations proceed, dynamic load balancing may also be needed. Two such toolkits are, e.g., the following:

<ul>

 <li>The Zoltan toolkit [15] is a C++ open-source software collection of such data management services for parallel unstructured, adaptive, and dynamic applications, available as open-source software from the Sandia National Laboratories. The design goal is to simplify the load balancing, data movement, and unstructured communication that arise in dynamic applications such as adaptive finite element methods, particle methods, and multiphysics simulations.</li>

 <li>The METIS software [16] is another alternative for parallel partitioning, load balancing, and data-management services. It can provide efficient parallel partitioning of graphs with sizes up to a billion vertices, distributed over a thousand processors. It is an MPI-based parallel library that implements a variety of algorithms for partitioning unstructured graphs, meshes, and for computing fill-reducing orderings of sparse matrices. METIS includes routines that are especially suited for parallel AMR computations and large-scale numerical simulations. The algorithms implemented in METIS are based on the parallel multilevel k-way graph-partitioning, adaptive repartitioning, and parallel multi-constrained partitioning schemes.</li>

</ul>

<h3>Extreme-Scale Software Framework for Multiphysics Engineering and Scientific Problems</h3>

<ul>

 <li>The Trilinos Project [17, 18] is a C++ open-source software package that represents an effort to develop algorithms and enabling technologies within an object-oriented software framework for the solution of large-scale, complex multiphysics engineering and scientific problems. A unique design feature of Trilinos is its heavy focus on other similar packages within a computational software layer stack. Trilinos has been under development at Sandia National Laboratories since 2002, and it provides uniform access to accurate, robust, and efficient solvers and tools. It also facilitates more rapid development of new libraries by providing important core functionality and software engineering processes for developers. Trilinos unifies a diverse set of libraries that have been developed at Sandia, as well as tools developed by other researchers. Trilinos has also been the development basis for other fundamental numerical algorithms, e.g., time integration methods, eigenvalue solvers, and multilevel preconditioners.</li>

 <li>The Portable, Extensible Toolkit for Scientific Computation (PETSc, pronounced PET-see; the S is silent) [8], is a suite of data structures and routines developed by Argonne National Laboratory for the scalable (parallel) solution of scientific applications modeled by partial differential equations. It employs the Message Passing Interface (MPI) standard for all message-passing communication. PETSc is the world’s most widely used parallel numerical software library for PDEs and sparse matrix computations. The PETSc Core Development Group won the SIAM/ACM Prize in Computational Science and Engineering for 2015. PETSc is intended for use in large-scale application projects; many ongoing computational science projects are built around the PETSc libraries. Its careful design allows advanced users to have detailed control over the solution process. PETSc includes a large suite of parallel linear and nonlinear equation solvers that are easily used in application codes written in C, C++, Fortran, and now Python. PETSc provides many of the mechanisms needed within parallel application code, such as simple parallel matrix and vector assembly routines that allow the overlap of communication and computation. In addition, PETSc includes support for parallel distributed arrays useful for finite-difference methods.</li>

</ul>

<h2>Virtualization and Scientific Software Containerization</h2>

Scientific software usually builds on top of existing code base and thus has multiple dependencies on other software packages and libraries. It might be cumbersome and time consuming to deploy the software library of interest on a specific machine with given version of the system and other software stack components. Consider, for example, compiling a particular scientific library and its dependencies on your laptop. After you resolve all the dependencies of the library, you proceed with development of the code using the library. Next, you would like to deploy the application on a HPC cluster but you realize that you need to do the whole set-up process from a scratch, usually encountering many additional problems during the cluster environment preparation. Alternatively, you might need your colleague/collaborator to try running the code on his computer, but she is missing several software dependencies and needs to spend considerable time just on environment setup satisfying these dependencies.

Docker is a set of platform as a service products that use OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries and configuration files. You configure the environment once, setting up all levels of the software stack required for the development process, and then reuse the container on different machines. Containers can be run on every Docker-equipped machine in plug-and-play fashion.

For this project, we provide a Docker image that contains all the libraries you will need for solving the required tasks. These include, e.g., Petsc, Ipopt, Pardiso, MKL, custom built Python packages, Jupyter notebook, etc. Instead of installing all these software packages and their dependencies, or building the libraries from source yourself, the libraries are built and the environment is ready to be used for solving the actual scientific problems. All you need to do is install Docker app on your laptop (see instructions at the following URL: <a href="https://docs.docker.com/get-started/">https://docs.docker. </a><a href="https://docs.docker.com/get-started/">com/get-started/</a><a href="https://docs.docker.com/get-started/">)</a> and obtain the image of the hpc2020 container. The Docker image will be automatically downloaded when running it for the first time. Use the command

docker run -it –publish 8888:8888 -v &lt;host_path&gt;:&lt;container_path&gt; goghino/hpc2020:1.0 bash

This will start the container with an interactive bash session. In your terminal, you will find a new Linux session with user <em>jovyan </em>and a hostname representing hash of the running Docker image. Here, you can find a directory Libraries with the installation of the scientific libraries for this assignment. Note also that you are in a conda environment <em>(base)</em>, which has all the required packages already installed.

Important note: When you close the Docker session, all the changes you have made <u>will be lost</u>! Every time you

run the Docker image, it will be in a state as when you first run it. Remember to periodically save your work! In order to keep your changes safe, it is recommended to mount a host volume to the Docker image. This allows you to access files from the host inside the container, thus the changes and edits you have made in these files will be kept even after you exit the Docker session. You can define the volume (shared filesystem) using -v or –volume in the docker run command:

-v &lt;host_path&gt;:&lt;container_absolute_path&gt;

For example, in order to mount your HPC2020 repository to HPC2020 folder in the Docker image, use:

-v HPC2020_git_repo_path_your_computer:/home/jovyan/HPC2020

<h1>2.      Scientific Mathematical HPC Software Frameworks — The PoissonEquation [40 points]</h1>

Many books on programming languages start with a ”Hello, World!” program. Readers are curious to know how fundamental tasks are expressed in the language, and printing a text to the screen can be such a task. In the world of finite-difference methods for PDEs and HPC, one of the most fundamental tasks is to solve the Poisson equation. Our counterpart to the classical ”Hello, World!” program therefore is to solve the Poisson equation

<table width="680">

 <tbody>

  <tr>

   <td width="663">                                                                         −∆<em>y</em>(<em>x</em>)      =         <em>f</em>(<em>x</em>) on Ω = [0<em>,</em>1] × [0<em>,</em>1]</td>

   <td width="17">(1)</td>

  </tr>

  <tr>

   <td width="663">                                                                               <em>y</em>(<em>x</em>)     =     0          on <em>∂</em>Ωfor <em>x </em>∈ Ω = [0<em>,</em>1] × [0<em>,</em>1]. The Laplace operator ∆ for a two-dimensional problem is defined as</td>

   <td width="17">(2)</td>

  </tr>

 </tbody>

</table>

<em>.                                                                                          </em>(3)

Here, <em>y </em>= <em>y</em>(<em>x</em><sub>1</sub><em>,x</em><sub>2</sub>) is the unknown function, <em>f </em>= <em>f</em>(<em>x</em>) is a prescribed function, ∆ is the Laplace operator, Ω is the spatial domain, and <em>∂</em>Ω is the boundary of Ω. The Poisson problem, including both the PDE −∆<em>y</em>(<em>x</em>) = <em>f</em>(<em>x</em>) and the boundary condition <em>y</em>(<em>x</em>) = 0 on <em>∂</em>Ω, is an example of a boundary-value problem, which must be precisely stated before it makes sense to start solving it with various parallel numerical software tools for CSE.

In two space dimensions with coordinates <em>x</em><sub>1 </sub>and <em>x</em><sub>2</sub>, we can write out the Poisson equation as

<em>.                                                                   </em>(4)

The unknown <em>y </em>is now a function of two variables, <em>y </em>= <em>y</em>(<em>x</em><sub>1</sub><em>,x</em><sub>2</sub>), defined over a two-dimensional domain Ω ∈ R<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>. The Poisson equation arises in numerous physical contexts, including heat conduction, electrostatics, diffusion of substances, twisting of elastic rods, inviscid fluid flow, and water waves. Solving a boundary-value problem such as the Poisson equation using a second-order finite difference discretization, and deploying scalable mathematical algorithms and software tools for reliable parallel simulation consists of the following steps:

<ul>

 <li>Identify the computational domain (Ω), the PDE modeling the system under study, its boundary conditions, and source terms <em>f</em>(<em>x</em><sub>1</sub><em>,x</em><sub>2</sub>).</li>

 <li>Reformulate the PDE using a second-order finite-difference discretization scheme.</li>

 <li>Select an appropriate mathematical software for extreme-scale computing, such as, PETSc <sup>2 </sup>or Trilinos <a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a>, used to solve the discretized problem.</li>

 <li>The program should define the computational domain, the boundary conditions, and source terms, using the corresponding abstractions of the particular problem.</li>

</ul>

<h2>2.1.     Task to Solve [40 points]</h2>

<ul>

 <li>Please read the paper [19] entiled ”Challenges and Opportunities in CSE Research,” in particular, section 2 on ”Challenges and Opportunities in CSE Research,” ”CSE and High-Performance Computing,” and ”CSE Software”.</li>

 <li>Please view the two videos on the iCorsi course webpage that are discussing the challenges posed by architecture and software environments of today’s most powerful supercomputers, and even the greater complexity on the horizon from next-generation and exascale systems.</li>

</ul>

Figure 1: Solution of the Poisson equation (referred to as the forward problem) from Eqn. 5 and 6, using different RHS value in the boundary condition.

<ul>

 <li>We would like to ask you to build a code using one of the aforementioned HPC software frameworks to solve the following Poisson equation:</li>

</ul>

<table width="445">

 <tbody>

  <tr>

   <td width="64">−∆<em>y</em>(<em>x</em>)</td>

   <td width="25">=</td>

   <td width="340">20 on Ω = [0<em>,</em>1] × [0<em>,</em>1]<em>,</em></td>

   <td width="17">(5)</td>

  </tr>

  <tr>

   <td width="64"><em>y</em>(<em>x</em>)</td>

   <td width="25">=</td>

   <td width="340">0       on <em>∂</em>Ω<em>,</em></td>

   <td width="17">(6)</td>

  </tr>

 </tbody>

</table>

using second-order finite differences in parallel using multiple cores. You may select one of the PDE mathematical software frameworks that we mentioned above. You might consider looking at Petsc example in the repository, based on one of the example codes distributed with Petsc library. You can compile and run the example as following:

<table width="639">

 <tbody>

  <tr>

   <td width="639">(base) <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2a40455c534b446a1219484c1b484e19481e4b1d">[email protected]</a>:$ make petsc_ex(base) <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3c56534a455d527c040f5e5a0d5e580f5e085d0b">[email protected]</a>:$ OMP_NUM_THREADS=1 mpirun -n 1 ./petsc_ex -pc_type lu –<em>,</em>→ pc_factor_mat_solver_type mkl_pardiso -ksp_view -ksp_converged_reason</td>

  </tr>

 </tbody>

</table>

As an alternative, you might try to implement your own discretization and Python solver using the Jupyter notebook (please refer to the second part of the project on interactive supercomputing). You should also use a parallel direct factorization solver to solve the resulting discretized PDE.

<ul>

 <li>Summarize in two paragraphs your particular HPC implementation for this PDE, the motivation why you selected this framework, and report the parallel timing for various meshes below as listed in Table 1.</li>

 <li>Visualize the solution <em>y</em>(<em>x</em><sub>1</sub><em>,x</em><sub>2</sub>) of the Poisson problem for various sizes of discretized mesh <em>N</em>, similar to Fig. 1.</li>

</ul>

Table 1: Wall-clock time (in seconds) and speedup (in brackets) using multiple cores for solving the Poisson PDE problem (adjust cores count according to machined you used to solve the problem).

<table width="361">

 <tbody>

  <tr>

   <td width="65">Problem</td>

   <td width="53"><em>N</em></td>

   <td width="62"> </td>

   <td colspan="3" width="180">Number of cores</td>

  </tr>

  <tr>

   <td width="65"> </td>

   <td width="53"> </td>

   <td width="62">1</td>

   <td width="60">2</td>

   <td width="60">4</td>

   <td width="60">8</td>

  </tr>

  <tr>

   <td width="65">Poisson</td>

   <td width="53">100<sup>2</sup></td>

   <td width="62"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

  </tr>

  <tr>

   <td width="65">Poisson</td>

   <td width="53">500<sup>2</sup></td>

   <td width="62"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

  </tr>

  <tr>

   <td width="65">Poisson</td>

   <td width="53">1000<sup>2</sup></td>

   <td width="62"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

  </tr>

 </tbody>

</table>

<h1>3.      Interactive Supercomputing using Jupyter Notebook [10 points]</h1>

The Jupyter notebook is an incredibly powerful tool for interactively developing and presenting CSE projects. The Jupyter notebook integrates code and its output into a single document that combines visualizations, narrative text, mathematical equations, and other rich media. This intuitive workflow promotes iterative and rapid development, making notebooks an increasingly popular choice at the heart of contemporary CSE science, analysis, and increasingly science at large. In this project, we will set up Jupyter Notebooks on your local machine and we will start using it to perform a computational science and engineering project related to large-scale PDE-constrained optimization.

Many supercomputing centers, including CSCS, support the use of Jupyter notebook for interactive supercomputing. It is the next-generation web-based user interface for Project Jupyter. It is an open-source web application that allows creation and sharing of documents containing live code, equations, visualizations and narrative text. This short introduction will help you to familiarize with the HPC libraries used in the scientific computing and set up the environment. Additionally, we will establish a remote connection to the Jupyter notebook from your web browser, giving you an interactive access to the code development.

Nowadays, the console-based approach of developing and running the code is being replaced by a qualitatively new paradigm based on interactive computing represented by the Jupyter notebook.<a href="#_ftn4" name="_ftnref4"><sup>[4]</sup></a> It is providing a web-based application suitable for capturing the whole computation process: developing, documenting, and executing code, as well as communicating the results. The Jupyter notebook combines two components:

<ul>

 <li>A web application: a browser-based tool for interactive authoring of documents which combine explanatory text, mathematics, computations, and their rich media output.</li>

 <li>Notebook documents: a representation of all content visible in the web application, including inputs and outputs of the computations, explanatory text, mathematics, images, and rich media representations of objects.</li>

</ul>

<h2>3.1.     Setting-up Jupyter Notebook</h2>

In order to start the Jupyter notebook, simply start the Docker image and run

(base) <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="97fdf8e1eef6f9d7f4f1a6a7a6a7a7f1f2a7a2a5">[email protected]</a>:˜$ jupyter notebook

This will start the notebook kernel and print a message such as

<table width="675">

 <tbody>

  <tr>

   <td width="675">To access the notebook, open this file in a browser:file:///home/jovyan/.local/share/jupyter/runtime/nbserver-167-open.html Or copy and paste one of these URLs:http://cf10100fe052:8888/?token=4886725084e987042bb8f8270df86496a6bf25ff66280b27 or http://127.0.0.1:8888/?token=4886725084e987042bb8f8270df86496a6bf25ff66280b27</td>

  </tr>

 </tbody>

</table>

Figure 2: New Jupyter notebook.

Use the last URL, starting with http://127.0.0.1:8888, and open it using your local browser. In order to start the notebook with access to multiple cores, set the OMPNUMTHREADS environment variable, e.g.

(base) <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b3d9dcc5cad2ddf3d0d58283828383d5d6838681">[email protected]</a>:˜$ OMP_NUM_THREADS=4 jupyter notebook

<h2>3.2.     Jupyter Notebook — A Toy Example</h2>

In order to start working with the Jupyter notebook you need to create a new notebook. A new notebook may be created at any time, either from the dashboard (see Figure 2), or using the <em>File – New </em>menu option from within an active notebook. The new notebook is created within the same directory and will open in a new browser tab. It will also be reflected as a new entry in the notebook list on the dashboard. Be sure to create a Python 3 notebook, as shown in Figure 2. An open notebook has exactly one interactive session connected to a kernel, which will execute code sent by the user and communicate back results. This kernel remains active if the web browser window is closed, and reopening the same notebook from the dashboard will reconnect the web application to the same kernel. The notebook consists of a sequence of cells. A cell is a multiline text input field, and its contents can be executed by using Shift-Enter, or by clicking the ”<em>Play</em>” button on the toolbar. The execution behavior of a cell is determined by the cell’s type. There are three types of cells: code cells, markdown cells, and raw cells. Every cell starts off being a code cell, but its type can be changed by using a drop-down on the toolbar (which will be ”<em>Code</em>”, initially), or via keyboard shortcuts. The Jupyter notebook aims to support the latest versions of popular browsers; please use one of the supported browsers: Chrome, Safari, Firefox.

<h2>3.3.     Task to solve [10 points]</h2>

Your first task is to explore Python’s numerical libraries (NumPy, SciPy) and verify that you are able to run the computational kernels using multiple threads. These libraries use optimized implementation of BLAS, taking advantage of cache memory and assembler implementation. But many architectures now have a BLAS that also takes advantage of a multicore machine. If your numpy/scipy is compiled using one of these, then dot() will be computed in parallel (if this is faster) without you doing anything. Similarly for other matrix operations, like inversion, singular value decomposition, determinant, and so on.<a href="#_ftn5" name="_ftnref5"><sup>[5]</sup></a>

<ul>

 <li>Pick some computational kernel that can benefit from multi-threading provided by Python’s numerical libraries</li>

 <li>Verify that the library scales with the number of CPU threads. Note that depending on the underlying BLAS library, you will have to set different variables controlling multi-threading (OMPNUMTHREADS: OpenMP,</li>

</ul>

OPENBLASNUMTHREADS: OpenBLAS, MKLNUMTHREADS: MKL, VECLIBMAXIMUMTHREADS: Accelerate, NUMEXPRNUMTHREADS: Numexpr).

<ul>

 <li>Describe your Toy example (code, mathematical expression, etc.) and report the scaling of your Python example using a Jupyter notebook.</li>

</ul>

<h1>4.      Jupyter Notebook — Parallel PDE-Constrained Optimization [50 points]</h1>

<h2>4.1.     Large-Scale Parallel Nonlinear Optimization with IPOPT</h2>

In this project we will use Jupyter notebook and IPOPT [7] to solve a PDE-constrained optimization problem. IPOPT is an open-source software package for large-scale non-linear optimization. It is designed to find (local) solutions of mathematical optimization problems of the form

<table width="421">

 <tbody>

  <tr>

   <td width="397">minimize <em>f</em>(<em>x</em>) <em>x</em>∈R<em><sup>n</sup></em></td>

   <td width="24">(7a)</td>

  </tr>

  <tr>

   <td width="397">subject to <em>g<sub>L </sub></em>≤ <em>g</em>(<em>x</em>) ≤ <em>g<sub>U</sub>,</em></td>

   <td width="24">(7b)</td>

  </tr>

  <tr>

   <td width="397"><em>x<sub>L </sub></em>≤ <em>x </em>≤ <em>x<sub>U</sub>,</em></td>

   <td width="24">(7c)</td>

  </tr>

 </tbody>

</table>

where the objective function <em>f </em>is a mapping <em>f </em>: R<em><sup>n </sup></em>→ R, the equality and inequality constraints are defined as <em>g </em>: R<em><sup>n </sup></em>→ R<em><sup>m</sup></em>. Note that equality constraints can be formulated in the above formulation by setting the corresponding components of <em>g<sub>L </sub></em>and <em>g<sub>U </sub></em>to the same value.

Such optimization problems arise in a number of important engineering, financial, scientific, and medical applications. IPOPT implements an interior-point line-search filter method [20], which is particularly suitable for large problems with up to millions of variables and constraints, assuming that the Jacobian matrix of constraint function is sparse. It is important to keep in mind that the algorithm is only trying to find a local minimizer of the problem; if the problem is nonconvex, many stationary points with different objective function values might exist, and it depends on the starting point and algorithmic choices which particular one the method converges to. In general, the computational effort during the optimization with IPOPT is typically concentrated in the solution of linear systems, or in the computation of the problem functions and their derivatives, depending on the particular application. Thus an efficient linear solver, optimized linear algebra kernels and code implementing the problem functions and their gradient and Hessian is of paramount importance. Consequently, in order to compile IPOPT, certain third party code is required (such as the linear algebra routines: e.g., optimized Intel MKL<a href="#_ftn6" name="_ftnref6"><sup>[6]</sup></a> BLAS/LAPACK and a high-performance linear solver: e.g. PARDISO). Those codes are usually available under different conditions/licenses. The MKL library is already available on Euler cluster (after loading the appropriate module) and PARDISO licence is required and can be obtained for free for academic purposes at <a href="https://www.pardiso-project.org/">https://www.pardiso-project.org</a><a href="https://www.pardiso-project.org/">.</a>

An interface between your application and IPOPT is usually established by a set of the callback routines you need to implement. These routines provide information about your problem, e.g. (7), such as the number of variables, functions for evaluating the objective and constraints, their first- and second-order derivatives, etc. This means you have to work out derivatives, including the sparsity structure of the problem, and provide gradient/Hessian of the objective function <em>f </em>and Jacobian/Hessians of the constraints <em>g</em>.

We will be using a Python interface<a href="#_ftn7" name="_ftnref7"><sup>[7]</sup></a> in this project, meaning you will have to write a Python script following certain rules established by the interface, e.g., structure of the object and signature of the methods implementing your problem. Please refer to the official documentation<a href="#_ftn8" name="_ftnref8"><sup>[8]</sup></a> for details.

Figure 3: A Python class implementing the example problem (8).

<h2>4.2.     JupyterLab — Example of using IPOPT for a Small Optimization Problem</h2>

An example of how to formulate and solve a small problem using IPOPT is provided to you in a Jupyter notebook named Project8Template. The problem on hand is to find <em>x </em>= [<em>x</em><sub>0</sub><em>,x</em><sub>1</sub><em>,x</em><sub>2</sub><em>,x</em><sub>3</sub>] such that

minimize                                                          (8a)

<table width="467">

 <tbody>

  <tr>

   <td width="175">subject to <em>x<sup>T </sup>x </em>= 40<em>,</em></td>

   <td width="292">(8b)</td>

  </tr>

 </tbody>

</table>

<em>,                                                                                                       </em>(8c)

(8d)

In order to solve the problem, we need to create a Python class which implements routines that evaluate the following:

<ul>

 <li>objective function <em>f</em>(<em>x</em>),</li>

 <li>objective function gradient ∇<em><sub>x</sub>f</em>(<em>x</em>),</li>

 <li>constraints <em>g</em>(<em>x</em>) (8b) and (8c),</li>

 <li>Jacobian of constraints ∇<em><sub>x</sub>g</em>(<em>x</em>),</li>

 <li>Hessian of Lagrangian.</li>

</ul>

You can find an implementation of such a class in one of the notebook cells, as shown in Figure 3, illustrating definition of the class, implementation of the objective function, and its gradient. Note that name of the class methods, their signature, and type of the return value is given by the IPOPT’s interface and need to be strictly respected by the user. Please see the documentation for more information.<a href="#_ftn9" name="_ftnref9"><sup>[9]</sup></a> The call of IPOPT library is shown in Figure 4. Apart from the class implementing the problem, you need to provide the initial point <em>x</em><sub>0 </sub>and the bounds for the variables <em>lb,ub </em>and constraints <em>cl,cu</em>. We also need to provide the size of the vector of optimization variables <em>n </em>and number of constraints <em>m</em>. The solution, i.e., the local minimizer given the problem and the starting point, is returned, together with additional information (e.g. the values of the Lagrange multipliers at the optimum). Next, you can continue in your notebook to analyze the solution, create the visualizations or do a performance analysis. It’s up to you to explore and familiarize with the library so that you can solve yourself more interesting problem in the next part of this project.

Figure 4: IPOPT library call.

Important note! When running Python applications using MKL library (including Jupyter notebook) you need to start the application with explicitly telling the system to load the MKL libraries. In case of Jupyter notebook, it needs to be started as following:

<table width="675">

 <tbody>

  <tr>

   <td width="675">LD_PRELOAD=$MKLROOT/lib/intel64_lin/libmkl_core.so:$MKLROOT/lib/intel64_lin/libmkl_sequential<em>,</em>→ .so jupyter notebook</td>

  </tr>

 </tbody>

</table>

<h2>4.3.     JupyterLab — Large-Scale Parallel PDE-Constrained Optimization with IPOPT</h2>

<h3>4.3.1.     PDE-Constrained Optimization</h3>

One of the outstanding challenges of computational sciences and engineering is large-scale nonlinear parameter estimation governed by partial differential equations. These inverse problems are known as PDE-constrained optimization problems and are significantly more difficult to solve than PDE forward problems. PDE-constrained optimization refers to the optimization of systems governed by partial differential equations. These are known as <em>inverse problems </em>and typically characterize very large-scale optimizations, where the <em>forward problem </em>or <em>simulation problem </em>appears as one part within the optimizations. The forward problem usually characterizes applications in which the <em>control variables </em>of the PDE — initial conditions, domain sources, material coefficients, or boundary conditions (as we will use it in this project) — are known, and the <em>state variables </em>are determined by solving the PDE. The <em>inverse </em>or optimization problem reverses the process: here one tries to determine some control variables given performance goals in the form of an objective function and possibly inequality and equality constraints on the behavior of the system. Since the behavior of the system is modeled by a PDE, they appear as (usually equality) constraints in the optimization problem and it will be referred to as the <em>state equations</em>.

Let <strong>y </strong>represent the state variables, <strong>u </strong>the control variables, J the objective function, <strong>c </strong>the PDE state equations, and <strong>h </strong>the inequality constraints. The PDE-constrained optimization problem can be stated as

min              J(<strong>y</strong><em>,</em><strong>u</strong>)<em>,   </em>(9a) <strong>y</strong><em>,</em><strong>u</strong>

s.t.              <strong>c</strong>(<strong>y</strong><em>,</em><strong>u</strong>) = 0<em>,            </em>(9b) <strong>h</strong>(<strong>y</strong><em>,</em><strong>u</strong>) ≥ 0<em>. </em>(9c)

Many engineering and science problems — in such diverse areas as aerodynamics, atmospheric sciences, image registration, medicine, structural-fluid interactions, and chemical process industry — can be expressed in the form of a PDE-constrained problem. The common difficulty is that the PDE solution is just a subproblem associated with the optimization problem. Moreover, the inverse problem is often ill-posed despite the well-posedness of the forward problem, and the inverse problem can have numerous local solutions. For these reasons the optimization problem is often significantly more difficult to solve than the simulation problem.

The size, complexity, and infinite-dimensional nature of PDE-constrained optimization problems present significant challenges for general-purpose optimization algorithms, and parallel HPC implementations are typically necessary to cope with the numerical challenges. When the infinite-dimensional conditions in (9) are discretized, one can represent the discretized PDE-constrained optimization problem by

<table width="408">

 <tbody>

  <tr>

   <td width="65">min <em>y,u</em></td>

   <td width="294">J(<em>y,u</em>)<em>,</em></td>

   <td width="48">(10a)</td>

  </tr>

  <tr>

   <td width="65">s.t.</td>

   <td width="294"><em>c</em>(<em>y,u</em>) = 0<em>,</em></td>

   <td width="48">(10b)</td>

  </tr>

 </tbody>

</table>

where <em>y </em>∈ R<em><sup>n</sup></em>, <em>u </em>∈ R<em><sup>m </sup></em>are the discrete state and control variables, J ∈ R is the objective function, and <em>c </em>∈ R<em><sup>m </sup></em>are the discretized state equations.<a href="#_ftn10" name="_ftnref10"><sup>[10]</sup></a> Using adjoint variables <em>λ </em>∈ R<em><sup>n</sup></em>, one can define the Lagrangian function by L := J(<em>y,u</em>)+<em>λ<sup>T </sup>c</em>(<em>y,u</em>). The first-order optimality conditions require that the gradient of the Lagrangian vanishes:

 <em>∂<sub>y</sub></em>L   <em>g<sub>y </sub></em>+ <em><sup>J</sup></em><em><sub>y</sub></em><em>T </em><em>λ </em><sub></sub>

<em>∂<sub>u</sub></em>L = <em>g<sub>u </sub></em>+ <em>J<sub>u</sub><sup>T </sup>λ </em>= 0<em>. </em>(11)  <em>∂<sub>λ</sub></em>L   <em>c </em>

Here, <em>g<sub>y </sub></em>∈ R<em><sup>n </sup></em>and <em>g<sub>u </sub></em>∈ R<em><sup>m </sup></em>are the gradients of J with respect to the states and control variables respectively; <em>J<sub>y </sub></em>∈ R<em><sup>n</sup></em><sup>×<em>n </em></sup>is the Jacobian of the state equations with respect to the state variables; and <em>J<sub>u </sub></em>∈ R<em><sup>n</sup></em><sup>×<em>m </em></sup>is the Jacobian of the state equations with respect to the control variables. A Newton step on the optimality conditions gives the following linear system:

<em> .                                                           </em>(12)

Here, <em>W </em>∈ R<sup>(<em>n</em>+<em>m</em>)×(<em>n</em>+<em>m</em>) </sup>is the Hessian of the Lagrangian L, it involves second derivatives of both J and <em>c</em>, and it is block-partioned according to state and control variables. <em>p<sub>y </sub></em>∈ R<em><sup>n </sup></em>is the search direction in the <em>y </em>variables, <em>p<sub>u </sub></em>∈ R<em><sup>m </sup></em>is the search direction in the <em>u </em>variables, and <em>λ </em>∈ R<em><sup>n </sup></em>is the update in the Lagrangian multipliers.

In this ”HPC Lab for CSE” project, we are aiming to solve a boundary control problem. The goal is to determine a boundary control <em>u</em>(<em>x</em><sub>1</sub><em>,x</em><sub>2</sub>) on <em>∂</em>Ω = [0<em>,</em>1] × [0<em>,</em>1], that minimizes the functional with a given weight <em>α </em>≥ 0  (13)

<table width="680">

 <tbody>

  <tr>

   <td width="397">subject to the state equations</td>

   <td width="259"> </td>

   <td width="24"> </td>

  </tr>

  <tr>

   <td width="397">−∆<em>y</em>(<em>x</em>) + <em>d</em>(<em>x,y</em>(<em>x</em>)) = 0<em>,</em></td>

   <td width="259">for <em>x </em>∈ Ω</td>

   <td width="24">(14)</td>

  </tr>

  <tr>

   <td width="397"><em>y</em>(<em>x</em>) = <em>b</em>(<em>x,u</em>(<em>x</em>))<em>,</em>and the inequality constraints on state and control</td>

   <td width="259">for <em>x </em>∈ <em>∂</em>Ω<em>,</em></td>

   <td width="24">(15)</td>

  </tr>

  <tr>

   <td width="397"><em>S</em>(<em>x,y</em>(<em>x</em>)) ≤ 0<em>,</em></td>

   <td width="259">for <em>x </em>∈ Ω<em>,</em></td>

   <td width="24">(16)</td>

  </tr>

  <tr>

   <td width="397"><em>C</em>(<em>x,u</em>(<em>x</em>)) ≤ 0<em>,</em></td>

   <td width="259">for <em>x </em>∈ <em>∂</em>Ω<em>.</em></td>

   <td width="24">(17)</td>

  </tr>

 </tbody>

</table>

<h3>4.3.2.     Discretization</h3>

As in the forward problem (in our case the Poisson example from Eq. 1 and 2), the discretization is restricted to the Laplacian operator ∆ on a unit square <sup>Ω = (0¯ </sup><em>,</em>1)×(0<em>,</em>1) considering Dirichlet boundary conditions. The domain can be discretized using a standard mesh with <em>N </em>∈ N+ points and the step size <em>h </em>= 1<em>/</em>(<em>N </em>+ 1), obtaining the mesh points

<em>x<sub>i,j </sub></em>= (<em>ih,jh</em>)<em>,             </em>0 ≤ <em>i,j </em>≤ <em>N </em>+ 1<em>.                                </em>(18)

The approximation of the state and control values, <em>y</em>(<em>x<sub>i,j</sub></em>) and <em>u</em>(<em>x<sub>i,j</sub></em>) are denoted as <em>y<sub>i,j </sub></em>and <em>u<sub>i,j</sub></em>, respectively. The discretized cost function, considering the functional defined in (13) becomes

<em>.             </em>(19)

<sup>2     </sup>Ω                                                     <sup>2    </sup><em>∂</em>Ω                                                                                    Figure 5: Discretization of the domain.

Similarly, the discretized state equation (14) (in our case the Poisson equation from ??) becomes a standard 5-point stencil operator

<em>G</em><em>hi,j </em>= 4<em>y</em><em>i,j </em>− <em>y</em><em>i</em>+1<em>,j </em>− <em>y</em><em>i</em>−1<em>,j </em>− <em>y</em><em>i,j</em>+1 − <em>y</em><em>i,j</em>−1 + <em>h</em>2<em>d</em><em>i,j</em><em>.                                                             </em>(20)

Assuming the Dirichlet boundary conditions, the equation (15) reduces to identity <em>y<sub>i,j </sub></em>= <em>u<sub>i,j </sub></em>on the boundary <em>∂</em>Ω. Finally, the state and control inequality take the form

<em>S<sup>h</sup></em>(<em>x<sub>i,j</sub>,y<sub>i,j</sub></em>) ≤ 0<em>,                                                            </em>for (<em>i,j</em>) ∈ Ω<em>,                                                  </em>(21)

<em>C<sup>h</sup></em>(<em>x<sub>i,j</sub>,u<sub>i,j</sub></em>) ≤ 0<em>,                                                            </em>for (<em>i,j</em>) ∈ <em>∂</em>Ω<em>.                                               </em>(22)

<h3>4.3.3.     Optimization Problem</h3>

Having defined the objective function, constraints, and the variable bounds we can formulate the optimization problem in form (7) to be solved. This reads

<table width="426">

 <tbody>

  <tr>

   <td width="395">minimize <em>F<sup>h</sup></em>(<em>y,u</em>) <em>y,u</em></td>

   <td width="32">(23a)</td>

  </tr>

  <tr>

   <td width="395">subject to <em>g<sub>L </sub></em>≤ <em>G<sup>h</sup></em>(<em>y</em>) ≤ <em>g<sub>U</sub>,</em></td>

   <td width="32">(23b)</td>

  </tr>

  <tr>

   <td width="395"><em>u<sub>L </sub></em>≤ <em>u </em>≤ <em>u<sub>U</sub>,</em></td>

   <td width="32">(23c)</td>

  </tr>

  <tr>

   <td width="395"><em>y<sub>L </sub></em>≤ <em>y </em>≤ <em>y<sub>U</sub>.</em></td>

   <td width="32">(23d)</td>

  </tr>

 </tbody>

</table>

<h3>4.3.4.     Task to Solve [50 points]</h3>

<ol>

 <li>Use the Jupyter notebook to implement and solve the PDE-constrained optimization problem (24) using IPOPT. To get you started you can find a demo notebook Project7Template.ipynb provided with this assignment, which demonstrates a basic usage of the IPOPT interface and some useful features of the Jupyter notebook. Consider the following parameters for the boundary control problem on Ω = [0<em>,</em>1] × [0<em>,</em>1]:</li>

</ol>

<table width="485">

 <tbody>

  <tr>

   <td width="64">−∆<em>y</em>(<em>x</em>)</td>

   <td width="57">=</td>

   <td width="266">20 on Ω</td>

   <td width="98">(24)</td>

  </tr>

  <tr>

   <td width="64"><em>y</em>(<em>x</em>)</td>

   <td width="57">≤</td>

   <td width="266">3<em>.</em>5 on Ω</td>

   <td width="98">(25)</td>

  </tr>

  <tr>

   <td width="64"><em>y</em><em>d</em></td>

   <td width="57">≡</td>

   <td width="266">3 + 5<em>x</em><sub>1</sub>(<em>x</em><sub>1 </sub>− 1)<em>x</em><sub>2</sub>(<em>x</em><sub>2 </sub>− 1) on Ω</td>

   <td width="98">(26)</td>

  </tr>

  <tr>

   <td width="64"><em>y</em>(<em>x</em>)</td>

   <td width="57">=</td>

   <td width="266"><em>u</em>(<em>x</em>)        on <em>∂</em>Ω</td>

   <td width="98">(27)</td>

  </tr>

  <tr>

   <td width="64">0</td>

   <td width="57">≤ <em>u</em>(<em>x</em>)</td>

   <td width="266">≤ 10        on <em>∂</em>Ω</td>

   <td width="98">(28)</td>

  </tr>

  <tr>

   <td width="64"><em>u</em><em>d</em></td>

   <td width="57">≡</td>

   <td width="266">0 on <em>∂</em>Ω</td>

   <td width="98">(29)</td>

  </tr>

  <tr>

   <td width="64"><em>α</em></td>

   <td width="57">=</td>

   <td width="266">0<em>.</em>01 on <em>∂</em>Ω</td>

   <td width="98">(30)</td>

  </tr>

 </tbody>

</table>

(31)

using second-order finite differences. We would like to ask you to build a Jupyter notebook code and to solve the PDE-constrained optimization problem in parallel using multiple cores on Euler. You should use IPOPT to solve this PDE-constrained optimization problem in parallel. [25 points]

<ol start="2">

 <li>Can you think of an appropriate initial point for the optimizer? [2.5 points]</li>

 <li>Visualize the result <em>y</em>(<em>x</em>) on Ω and on <em>∂</em>Ω using Jupyter notebook. [2.5 points]</li>

 <li>IPOPT is a package for solving general NLP problems, and as such evaluates the Hessian and Jacobian of constraints in every iteration. Is this necessary for problem (23)? Consult the IPOPT documentation to see if you can solve the problem more efficiently<a href="#_ftn11" name="_ftnref11"><sup>[11]</sup></a>. [5 points]</li>

 <li>Analyze the performance of the optimization procedure for different discretization sizes of <em>N</em>, and observe scaling with different number of threads. Please report these numbers using Table 2. What are your findings? Add this analysis to your Jupyter notebook (be sure to start the Jupyter notebook with sufficient resources). [15 points]</li>

 <li>The optimizer needs to control only the <em>u </em>variables, but we ask you to also optimize the state variables <em>y </em>by including them in the optimization vector. What would be the consequences of letting the optimizer control only the <em>u </em>variables and handling <em>y </em>explicitly given the controls <em>u</em>? What is the consequence for the Hessian and Jacobian of the problem when solving the reduced space problem (in terms of dimensionality and sparsity)? [20 bonus points]</li>

</ol>

Table 2: Wall-clock time (in seconds) and speedup (in brackets) using multiple cores for solving the PDE-constrained optimization problem (adjust cores count according to machined you used to solve the problem).

<table width="406">

 <tbody>

  <tr>

   <td width="111">Problem</td>

   <td width="53"><em>N</em></td>

   <td width="62"> </td>

   <td colspan="3" width="180">Number of cores</td>

  </tr>

  <tr>

   <td width="111"> </td>

   <td width="53"> </td>

   <td width="62">1</td>

   <td width="60">2</td>

   <td width="60">4</td>

   <td width="60">8</td>

  </tr>

  <tr>

   <td width="111">Inverse Poisson</td>

   <td width="53">100<sup>2</sup></td>

   <td width="62"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

  </tr>

  <tr>

   <td width="111">Inverse Poisson</td>

   <td width="53">500<sup>2</sup></td>

   <td width="62"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

  </tr>

  <tr>

   <td width="111">Inverse Poisson</td>

   <td width="53">1000<sup>2</sup></td>

   <td width="62"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

   <td width="60"> </td>

  </tr>

 </tbody>

</table>

Figure 6: Solution of the PDE-constrained optimization equation (referred to as the inverse problem) from Eqn 24 to 31

<h1>Additional Reading</h1>

<ul>

 <li>Andreas Wachter. Short Tutorial: Getting Started With Ipopt in 90 Minutes.¨ <a href="https://projects.coin-or.org/CoinBinary/export/837/CoinAll/trunk/Installer/files/doc/Short%20tutorial%20Ipopt.pdf">[online]</a></li>

 <li>Helmut Mauer and Hans D. Mittelmann. Optimization Techniques for Solving Elliptic Control Problems with</li>

</ul>

Control and State Constraints: Part 1. Boundary Control. <a href="https://pdfs.semanticscholar.org/44fd/197c5bfa2a76f2ccacf8f9a7a62ab290335f.pdf">[online]</a>

<ul>

 <li>The Jupyter Notebook. <a href="https://jupyter-notebook.readthedocs.io/en/stable/notebook.html">[online]</a></li>

</ul>

<h1>Additional Notes and Submission Details</h1>

Collect all your source code, results and figures in a Jupyter notebook. Be sure it also contains your name and summary of your answers, results, and observations for all exercises. When you are satisfied with your notebook, upload it together with the PDF version (export the notebook as a PDF file) to <a href="https://www.icorsi.ch/course/view.php?id=10049">iCorsi .</a> Submit the source code files (together with your used Makefile) in an archive file (tar, zip, etc.), and summarize your results and observations for all exercises by writing an extended Latex report. Use the Latex template provided on the webpage and upload the Latex summary as a PDF to <a href="https://www.icorsi.ch/course/view.php?id=10049">iCorsi</a> <a href="https://www.icorsi.ch/course/view.php?id=10049">.</a>

<ul>

 <li>Your submission should be a gzipped tar archive, formatted like project number lastname firstname.zip or project number lastname firstname.tgz. It should contain

  <ul>

   <li>all the source codes of your solutions;</li>

   <li>your write-up with your name project number lastname firstname.pdf.</li>

  </ul></li>

 <li>Submit your .zip/.tgz through <a href="https://www.icorsi.ch/course/view.php?id=10049">iCorsi .</a>.</li>

</ul>

<h1>References</h1>

<ul>

 <li>Wolfram. <em>The Mathematica Book 5</em>. Cambridge University Press, 2008.</li>

 <li>B. Monagan, K.M. Heal and Labahn K.O. Geddes, S.M. Vorkoetter, J. McCarron, and P. DeMarco. <em>Maple 12 Advanced Programming Guide</em>. Maplesoft, Waterloo, Ontario, Canada, 2008.</li>

 <li><em>Version Release 2019b</em>. The MathWorks Inc., Natick, Massachusetts, 2019.</li>

 <li>Jeff Bezanson, Alan Edelman, Stefan Karpinski, and Viral B. Shah. Julia: A fresh approach to numerical computing. <em>SIAM Review</em>, 59(1):65–98, 2017.</li>

 <li>Jack J. Dongarra, J. Du Croz, Sven Hammarling, and R. J. Hanson. An extended set of FORTRAN Basic Linear Algebra Subprograms. <em>ACM Trans. Math. Softw.</em>, 1(14):1–17, March 1988.</li>

 <li>Anderson, Z. Bai, C. Bischof, S. Blackford, J. Demmel, J. Dongarra, J. Du Croz, A.Greenbaum, S. Hammarling, A. McKenney, and D. Sorensen. <em>LAPACK Users’ Guide</em>. Society for Industrial and Applied Mathematics, 3 edition, 2000.</li>

 <li>Wachter and L. T. Biegler. On the implementation of a primal-dual interior point filter line search algorithm¨ for large-scale nonlinear programming. <em>Mathematical Programming</em>, 106(1):25–57, 2006.</li>

 <li>Satish Balay, Shrirang Abhyankar, Mark F. Adams, Jed Brown, Peter Brune, Kris Buschelman, Lisandro Dalcin, Alp Dener, Victor Eijkhout, William D. Gropp, Dmitry Karpeyev, Dinesh Kaushik, Matthew G. Knepley, Dave A. May, Lois Curfman McInnes, Richard Tran Mills, Todd Munson, Karl Rupp, Patrick Sanan, Barry F. Smith, Stefano Zampini, Hong Zhang, and Hong Zhang. PETSc Web page. <a href="https://www.mcs.anl.gov/petsc">https://www.mcs.anl.gov/ </a><a href="https://www.mcs.anl.gov/petsc">petsc</a><a href="https://www.mcs.anl.gov/petsc">,</a></li>

 <li>J. Dongarra, Jeremy Du Croz, Sven Hammarling, and I. S. Duff. A set of level 3 basic linear algebra subprograms. <em>ACM Trans. Math. Softw.</em>, 16(1):1–17, 1990.</li>

 <li>Patrick R. Amestoy, Alfredo Buttari, Jean-Yves L’Excellent, and Theo Mary. Performance and scalability of the block low-rank multifrontal factorization on multicore architectures. <em>ACM Trans. Math. Softw.</em>, 45(1), February 2019.</li>

 <li>Karypis and V. Kumar. A fast and high quality multilevel scheme for partitioning irregular graphs. <em>SIAM Journal on Scientific Computing</em>, 20(1):359–392, 1998.</li>

 <li>Xiaoye S. Li. An overview of Superlu: Algorithms, implementation, and user interface. <em>ACM Trans. Math. Softw.</em>, 31(3):302 – 325, September 2005.</li>

 <li>Schenk and K. Gartner. On fast factorization pivoting methods for symmetric indefinite systems.¨ <em>Electronic Transactions on Numerical Analysis</em>, 23(1):158–179, 2006.</li>

 <li>Brendan Gavin, Agnieszka Miedlar, and Eric Polizzi. Feast eigensolver for nonlinear eigenvalue problems. <em>Journal of Computational Science</em>, 27:107 – 117, 2018.</li>

 <li>Bhowmick, E. Boman, K. Devine, A. Gebremedhin, B. Hendrickson, P. Hovland, T. Munson, and A. Pothen. Combinatorial Algorithms Enabling Computational Science: Tales from the Front. In <em>Journal of Physics: Conference Series</em>, pages 453–457, 46 (2006).</li>

 <li>Dominique LaSalle, Md Mostofa Ali Patwary, Nadathur Satish, Narayanan Sundaram, Pradeep Dubey, and George Karypis. Improving graph partitioning for modern graphs and architectures. In <em>Proceedings of the 5th Workshop on Irregular Applications: Architectures and Algorithms</em>, New York, NY, USA, 2015. Association for Computing Machinery.</li>

 <li>A. Heroux, R.A. Bartlett, V.E. Howle, R.J. Hoekstra, J.J. Hu, T.G. Kolda, R.B. Lehoucq, K.R. Long, R.P. Pawlowski, E.T. Phipps, et al. An overview of the Trilinos project. <em>ACM Transactions on Mathematical Software (TOMS)</em>, 31(3):397–423, 2005.</li>

 <li>Katherine J. Evans, Mitchell T. Young, Benjamin S. Collins, Seth R. Johnson, Andrey V. Prokopenko, and Michael A. Heroux. Interfaces to trilinos in preparation for exascale developments.</li>

 <li>Ulrich Rude, Karen Willcox, Lois Curfman McInnes, and Hans De Sterck. Research and education in computa-¨ tional science and engineering. <em>SIAM Review</em>, 60(3):707–754, 2018.</li>

 <li>Andreas Wachter and Lorenz T. Biegler. Line search filter methods for nonlinear programming: motivation and¨ global convergence. <em>SIAM J. Optim.</em>, 16(1):1–31 (electronic), 2005.</li>

</ul>

<a href="#_ftnref1" name="_ftn1">[1]</a> Some of these software packages, e.g., IPOPT, METIS, PETSc, Trilinos, or PARDISO have been selected as important tools for future HPC applications.

<a href="#_ftnref2" name="_ftn2">[2]</a> PETSc is pronounced PET-see (the S is silent), is a suite of data structures and routines for the scalable parallel solution of scientific applications modeled by partial differential equations. It supports MPI, and GPUs, as well as hybrid MPI-GPU parallelism.

<a href="#_ftnref3" name="_ftn3">[3]</a> Trilinos is a collection of open-source software libraries, called packages, intended to be used as building blocks for the development of scientific applications. For more details please see, e.g., <a href="https://en.wikipedia.org/wiki/Trilinos">https://en.wikipedia.org/wiki/Trilinos</a>

<a href="#_ftnref4" name="_ftn4">[4]</a> <a href="https://jupyter-notebook.readthedocs.io/en/stable/notebook.html">https://jupyter-notebook.readthedocs.io/en/stable/notebook.html</a>

<a href="#_ftnref5" name="_ftn5">[5]</a> <a href="https://scipy.github.io/old-wiki/pages/ParallelProgramming">https://scipy.github.io/old-wiki/pages/ParallelProgramming</a>

<a href="#_ftnref6" name="_ftn6">[6]</a> <a href="https://software.intel.com/content/www/us/en/develop/tools/math-kernel-library.html">https://software.intel.com/content/www/us/en/develop/tools/math-kernel-library.html</a>

<a href="#_ftnref7" name="_ftn7">[7]</a> <a href="https://pypi.org/project/ipopt/">https://pypi.org/project/ipopt/</a>

<a href="#_ftnref8" name="_ftn8">[8]</a> <a href="https://pythonhosted.org/ipopt/">https://pythonhosted.org/ipopt/</a>

<a href="#_ftnref9" name="_ftn9">[9]</a> <a href="https://pythonhosted.org/ipopt/">https://pythonhosted.org/ipopt/</a>

<a href="#_ftnref10" name="_ftn10">[10]</a> Let us omit the nonequality constraints <strong>h </strong>in what follows in order to simplify the notation for better readability.

<a href="#_ftnref11" name="_ftn11">[11]</a> <a href="https://coin-or.github.io/Ipopt/OPTIONS.html#OPT_NLP">https://coin-or.github.io/Ipopt/OPTIONS.html#OPT_NLP</a>