Download Link: https://assignmentchef.com/product/solved-cs1xc3-assignment-8-makefiles
<br>
<ul>

 <li>Create a makefile to compile code and conduct automated testing.</li>

 <li>Write C programs that utilize multiple files and command-line arguments.</li>

</ul>

<strong> </strong>Requirements

You are given a starter.zip file that includes the following:

<ul>

 <li>A Makefile very similar to that covered in week 10 lab.</li>

 <li>A src folder containing a library.h, library.c, and files max.c, mulitply.c and square.c o The library.h and library.c files are a common library of functions that are to be used in the implementation of max.c, multiply.c and square.c. We’ll call this “the library” in this document.</li>

</ul>

o multiply.c has already been implemented for you.

<ul>

 <li>A test_data folder that contains input and expected results txt files to be used in the creation of automated tests for max, multiply and square.</li>

</ul>

You should unzip the folder and copy the contents to the pascal server with an SFTP client (see the videos posted for accessing the pascal server in order to use an SFTP client).

In this assignment you will do the following:

<ul>

 <li>Implement max.c and square.c to find a max number, calculate square areas using the library functions and command-line arguments.</li>

 <li>Modify the makefile so that it is able to build max.c, square.c and multiply.c using the library.</li>

 <li>Modify the makefile so that it conducts automated tests of max.c, square.c and multiply.c using diff and the given input and expected results txt files.</li>

</ul>

It is <strong><u>highly recommended</u></strong> that you complete the assignment in the order it is described below, and that you get one section working before proceeding with the next.  The assignment description is long because how to complete each step is described very explicitly with examples provided, but the total lines of code that either need to be created or modified is only 34 to complete the assignment.




<h1>Implementing Programs: multiply, square and max</h1>

The multiply program is implemented in multiply.c.  It requires one int command-line argument that it uses to multiply numbers provided via standard input until EOF is reached.  This program has been implemented for you already.  Assuming that you have unzipped the starter code on the pascal server, if you navigate to the src folder you should be able to compile and run the program like this:




Notice how we compile the program using <strong><em>both </em></strong>multiply.c and library.c, similar to the example done in-lecture during week 9.  We run the program using the multiply_input.txt file for standard input, and we see that the content matches the multiply _expected.txt file.

Implement square.c such that it reads ints from standard input that represent the side of a square until EOF is reached, and for each side outputs the area of a square with that side.  The square program should utilize the relevant library method to help implement this functionality.  The square program should accept a command-line argument that is the units (e.g. inches, cm, feet, etc.).  This unit should be included with the output of each square area.  The finished square program should work like this if used with the square_input.txt file as standard input (and we see that the output matches the square_expected.txt file):




Implement max.c such that it accepts “any” number of ints as command-line arguments.  It should use the relevant library method to find the maximum of these ints, and it should output this maximum int.  Note that because command-line arguments are a string (char array) in C, they will need to be converted to ints using atio(), as done in the provided multiply program (see: <a href="https://www.tutorialspoint.com/c_standard_library/c_function_atoi.htm">https://www.tutorialspoint.com/c_standard_library/c_function_atoi.htm</a><a href="https://www.tutorialspoint.com/c_standard_library/c_function_atoi.htm">)</a>.  Dynamically allocate space for an array of ints that is large enough to hold the number of ints required (argc can tell you how much space is required).  The finished max program should work like this if used with the following command-line arguments:







<h1>Modify the makefile to build multiply, square, max</h1>

Next modify the provided makefile so that if the command <strong>make all </strong>is run from the commandline, it will compile the multiply, square and max programs, with the resulting executables placed in a directory called <strong>build </strong>(that as with test_data and src, is a subdirectory of the directory containing the makefile).

Your makefile must compile the library.o object file <strong><em>before </em></strong>compiling multiply, square and max, and your makefile must use it as a prerequisite in the compilation of multiply, square and max, rather than compiling multiply, square and max with library.c each time as in the above examples.  This is so that the same library is not compiled repeatedly and unnecessarily.  After running <strong>make all</strong>, in addition to the executables for multiply, square and max, the build directory should also contain library.o.

In order to implement this functionality, modify the existing makefile rule for $(all_targets) to compile the targets using library.o, and add an additional rule to compile the target  library.o object file.  These are the only changes required to implement this functionality and in total requires writing or modifying 4-5 lines of code  Whenever possible while writing this code, use the variables for BUILD_DIR, SRC_DIR, as well as the special variables for the compiler and compiler flags.  An implementation that “hardcodes” all of this functionality will not receive full marks.

Note that we can compile library.c separately to library.o, and then use it in the compilation of multiply, square and max.  Here is an example of compiling library.o and then using it to compile the max, multiply and square executables, done manually from the src folder.  Your makefile code is essentially doing this, but library.o and the executables should be put into the build folder.







<h1>Modify the makefile to automatically test multiply, square and max</h1>

Create a new target in the makefile called <strong>test </strong>that will run automated tests of each executable using diff (as discussed in the week 10 lab) when we run <strong>make test</strong>.  diff will be used to compare the actual program output to the expected program output, knowing that if diff finds differences, then make will cease execution (again as discussed in the week 10 labs).  You can assume that <strong>make test</strong> is being run after <strong>make all</strong> has already been run.

Your test target recipe (i.e. list of commands) should begin by creating a new subdirectory called <strong>test</strong> similar to how the <strong>build</strong> folder is created when we run <strong>make all</strong>.  This <strong>test</strong> folder should temporarily contain text files of the actual output from programs run during automated testing.  The folder should be removed as the last command of the recipe after all testing has completed (very similar to how we remove the build folder with the clean target).  The exact test directory name to use should be configurable via a variable, in the same way the build and src directory names are configurable with a variable.

After the test subdirectory is created, each executable should be tested:

<ul>

 <li>The square executable should be run with the command-line argument “inches” using the square_input.txt file contained in the test_data subdirectory as standard input, and the output should be redirected to a file called square_output.txt in the test subdirectory. A diff should be done to ensure that square_output.txt in the test subdirectory is equivalent to square_expected.txt file in the test_data subdirectory.</li>

 <li>The multiply executable should be run with the command-line argument “2” using the multiply_input.txt file contained in the test_data subdirectory as standard input, and the output should be redirected to a file called multiply_output.txt in the test subdirectory. A diff should be done to ensure that the multiply_output.txt file in the test subdirectory is equivalent to the multiply_expected.txt file in the test_data subdirectory.</li>

 <li>The max executable should be run with “4 3 2 1 5 7 8 10 6” as command-line arguments and the output should be redirected to a file called max_output.txt in the test subdirectory. A diff should be done to ensure that the max_output.txt file in the test subdirectory is equivalent to the max_expected.txt file in the test_data subdirectory.</li>

</ul>

After each executable is tested as above, the test folder and all contents should be deleted.  All of this should be done with the relevant variables for directories rather than hardcoding directory names.

Add a command to the clean target recipe that will remove the test subdirectory as well… if a test fails due to a diff finding differences, we may want to examine the results in the test subdirectory, but after doing so, we will want to remove the subdirectory.

<strong> </strong>

After completing the above it should be possible to build and test the programs by running <strong>make all</strong> followed by <strong>make test</strong> followed by <strong>make clean</strong> or by running <strong>make all test clean</strong>.  Example output for doing so is provided after the marking scheme using the instructor’s solution.  <strong>This example output can be used to help create your makefile code, <em><u>as it effectively</u> <u>tells you what each command should look like for each of the above requirements</u>.</em> </strong>

<strong> </strong>