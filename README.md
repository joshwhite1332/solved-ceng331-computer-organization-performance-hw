Download Link: https://assignmentchef.com/product/solved-ceng331-computer-organization-performance-hw
<br>
<h1>1             Objectives</h1>

Many algorithms, such as the ones that are used in deep learning and image processing, require the application of a basic operation repeatedly. Hence, their performance depend on the performance of those basic operations. In this homework we will consider two such operations, namely <strong>Matrix Multiplication </strong>and <strong>Convolution</strong>.

Your objective in this homework is to optimize these functions as much as possible by using the concepts you have learned in class.

<h1>2             Specifications</h1>

Start by copying Optimization.tar to a protected directory in which you plan to do your work. Then give the command: tar xvf Optimization.tar . This will cause a number of files to be unpacked into the directory. The only file you will be modifying and handing in is kernels.c. The driver.c program is a driver program that allows you to evaluate the performance of your solutions. Use the command make driver to generate the driver code and run it with the command ./driver.

Looking at the file kernels.c you’ll notice a C structure team into which you should insert the requested identifying information about you. <strong>Do this right away so you don’t forget.</strong>

<h1>3             Implementation Overview</h1>

<h2>Matrix Multiplication</h2>

The naive approach for matrix multiplication is given in kernels.c as

void naive_matrix_multiplication(int dim, int *src, int *src2, int *dst) { int i,j,k;

for(i = 0; i &lt; dim; i++) for(j = 0; j &lt; dim; j++) { dst[j*dim+i]=0;

for(k = 0; k &lt; dim; k++)

dst[j*dim+i] = dst[j*dim+i] + src[j*dim+k]*src2[i+k*dim];

}

}

where the arguments to the procedure are pointers to the destination (<em>dst</em>) and two sources (<em>src</em>,<em>src</em>2) matrices, as well as the matrix size <em>N </em>(<em>dim</em>). Overall what this function does is multiplying <em>src </em>with <em>src</em>2 and obtaining <em>dst </em>(<em>dst </em>= <em>src </em>∗ <em>src</em>2)

Your task is to rewrite this code and minimize its CPE, by using techniques like code motion, loop unrolling and blocking.

<h2>Convolution</h2>

Convolution is used to modify and capture the spatial characteristics in both image processing and deep learning. A convolution is done by selecting a region on your main matrix and multiplying the values in that region by another smaller matrix, which is called as kernel.

For example, lets say you have a source matrix (<em>src</em>) and kernel matrix (<em>ker</em>)

Your output matrix (<em>dst</em>) will look like

In normal Image processing convolution kernel first rotated, and after that it applied on the source image. However, you are not required to do that in this homework. For additional information and examples about convolution operation click on <a href="http://setosa.io/ev/image-kernels/">here.</a>

The naive approach for Convolution is given in kernels.c as

void naive_conv(int dim,int *src, int *ker,int *dst) { int i,j,k,l;

for(i = 0; i &lt; dim-8+1; i++) for(j = 0; j &lt; dim-8+1; j++) { dst[j*dim+i] = 0; for(k = 0; k &lt; 8; k++) for(l = 0; l &lt; 8; l++) {

dst[j*dim+i] = dst[j*dim+i] +src[(j+l)*dim+(i+k)]*ker[l*dim+k];

}

}

}

where the arguments to the procedure are pointers to the destination (<em>dst</em>), source (<em>src</em>) and kernel(<em>ker</em>)

matrices, as well as the matrix size <em>N </em>(<em>dim</em>). You will only be using top left most 8<em>x</em>8 region of <em>ker </em>matrix for your kernel.

Your task is to rewrite this code and minimize its CPE, by using techniques like code motion, loop unrolling and blocking.

<h2>Performance measures</h2>

Our main performance measure is <em>CPE </em>or <em>Cycles per Element</em>. If a function takes <em>C </em>cycles to run for an matrix of size <em>N </em>× <em>N</em>, the CPE value is <em>C/N</em><sup>2</sup>.

The ratios (speedups) of the optimized implementation over the naive one will constitute a <em>score </em>of your implementation. To summarize the overall effect over different values of <em>N</em>, we will compute the <em>geometric mean </em>of the results for these 5 values. That is, if the measured speedups for <em>N </em>= {32<em>,</em>64<em>,</em>128<em>,</em>256<em>,</em>512} are <em>R</em><sub>32</sub>, <em>R</em><sub>64</sub>, <em>R</em><sub>128</sub>, <em>R</em><sub>256</sub>, and <em>R</em><sub>512 </sub>then we compute the overall performance as

<em>R             </em>= p5 <em>R</em>32× <em>R</em>64× <em>R</em>128× <em>R</em>256× <em>R</em>512

<h2>Assumptions</h2>

Assume that <em>N </em>is a multiple of 32 and the dimensions of kernel as 8<em>x</em>8. For convolution, dimensions of destination matrix can be determined by <em>N </em>− <em>K </em>+ 1 where <em>K </em>is equal to dimension size of the kernel. Your code must run correctly for all such values of <em>N </em>, but we will measure its performance only for <em>N </em>= {32<em>,</em>64<em>,</em>128<em>,</em>256<em>,</em>512}

<h1>4             Infrastructure</h1>

We have provided support code to help you test the correctness of your implementations and measure their performance. This section describes how to use this infrastructure. The exact details of each part of the assignment is described in the following section.

<strong>Note: </strong>The only source file you will be modifying is kernels.c.

<h2>Versioning</h2>

You will be writing many versions of the matrix multiplication and convolution functions. To help you compare the performance of all the different versions you’ve written, we provide a way of “registering” functions.

For example, the file kernels.c that we have provided you contains the following function:

void register_conv_functions() { add_conv_function(&amp;convolution, convolution_descr);

/* … Register additional test functions here */

}

This function contains one or more calls to add conv function. In the above example,

add conv function registers the function convolution along with a string convolution descr which is an ASCII description of what the function does. See the file kernels.c to see how to create the string descriptions. This string can be at most 256 characters long.

A similar function for your matrix multiplication kernels is provided in the file kernels.c.

<h2>Driver</h2>

The source code you will write will be linked with object code that we supply into a driver binary. To create this binary, you will need to execute the command unix&gt; make driver

You will need to re-make driver each time you change the code in kernels.c. To test your implementations, you can then run the command:

unix&gt; ./driver

The driver can be run in four different modes:

<ul>

 <li><em>Default mode</em>, in which all versions of your implementation are run.</li>

 <li><em>Autograder mode</em>, in which only the matrix multiplication() and convolution() functions are run. This is the mode we will run in when we use the driver to grade your handin.</li>

 <li><em>File mode</em>, in which only versions that are mentioned in an input file are run.</li>

 <li><em>Dump mode</em>, in which a one-line description of each version is dumped to a text file. You can then edit this text file to keep only those versions that you’d like to test using the <em>file mode</em>. You can specify whether to quit after dumping the file or if your implementations are to be run.</li>

</ul>

If run without any arguments, driver will run all of your versions (<em>default mode</em>). Other modes and options can be specified by command-line arguments to driver, as listed below:

-g : Run only matrix multiplication() and convolution() functions (<em>autograder mode</em>).

-f &lt;funcfile&gt; : Execute only those versions specified in &lt;funcfile&gt; (<em>file mode</em>).

-d &lt;dumpfile&gt; : Dump the names of all versions to a dump file called &lt;dumpfile&gt;, <em>one line </em>to a version (<em>dump mode</em>).

-q : Quit after dumping version names to a dump file. To be used in tandem with -d. For example, to quit immediately after printing the dump file, type ./driver -qd dumpfile.

-h : Print the command line usage.

<h2>Student Information</h2>

<strong>Important: </strong>Before you start, you should fill in the struct in kernels.c with information about you(student name, student id, student email).

<h1>5             Assignment Details</h1>

<h2>Optimizing Convolution (50 points)</h2>

In this part, you will optimize convolution to achieve as low CPE as possible. You should compile driver and then run it with the appropriate arguments to test your implementations.

For example, running driver with the supplied naive version (for Convolution) generates the output shown below:

unix&gt; ./driver

ID: eXXXXXXX

Name: Fatih Can Kurnaz

Email: <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="dbbe838383838383839bb8beb5bcf5b6beafaef5bebfaef5afa9">[email protected]</a>

conv: Version = naive_conv: Naive baseline implementation:

Dim                                 32               64               128             256             512         Mean

Your CPEs                      187.5         241.8         272.6         295.6         315.8

Baseline CPEs              185.9         241.4         272.4         296.1         315.3

Speedup                         1.0              1.0              1.0              1.0              1.0              1.0

<h2>Optimizing Matrix Multiplication (50 points)</h2>

In this part, you will optimize matrix multiplication to achieve as low a CPE as possible. You should compile driver and then run it with the appropriate arguments to test your implementations.

For example, running driver with the supplied naive version (for matrix multiplication) generates the output shown below:

unix&gt; ./driver

ID: eXXXXXXX

Name: Fatih Can Kurnaz

Email: <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="72172a2a2a2a2a2a2a3211171c155c1f1706075c1716075c0600">[email protected]</a>

Multip: Version = Naive_matrix_multiplication: Naive baseline implementation:

Dim                                 32               64               128             256             512         Mean

Your CPEs                      141.2         306.6         645.6             1386.4 3376.7

Baseline CPEs              141.7         306.8         642.5             1388.7 3373.7

Speedup                         1.0              1.0              1.0              1.0              1.0              1.0

<h2>Coding Rules</h2>

You may write any code you want, as long as it satisfies the following:

<ul>

 <li>It must be in ANSI C. You may not use any embedded assembly language statements.</li>

 <li>It must not interfere with the time measurement mechanism. You will also be penalized if your code prints any extraneous information.</li>

 <li>It must solely belong to you.</li>

 <li><strong>Important: </strong>Please, work on department inek machines. Because, all <em>CPE </em>values has been configured according to inek’s CPU and we will evaluate your codes on inek machines.</li>

</ul>

You can only modify code in kernels.c. You are allowed to define macros, additional global variables, and other procedures in this file.

<h2>Evaluation</h2>

Your solutions for matrix multiplication and convolution will each count for 50% of your grade. The score for each will be based on the following:

<ul>

 <li>Correctness: You will get NO CREDIT for buggy code that causes the driver to complain! This includes code that correctly operates on the test sizes, but incorrectly on matrices of other sizes. As mentioned earlier, you may assume that the matrix dimension is a multiple of 32.</li>

 <li>Speed-up: For each part(Convolution and Matrix Multiplication) the 3 student with the lowest amount of CPE’s (highest speedup) will receive 50 points for their implementation. Rest of the grades will be scaled accordingly, based on their standing in between highest cpe score of the top 3 student and base line cpe count.</li>

 <li>Since there might be changes of performance regarding to CPU status, test the same code many times and take only the best into consideration. When your codes are evaluated, your codes will be tested in a closed environment many times and only your best speed-ups will be taken into account.</li>

</ul>