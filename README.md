Download Link: https://assignmentchef.com/product/solved-cs537-project-1a
<br>
<h6 id="cs537-spring-2020-project-1a"><span style="font-size: 2em; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', sans-serif;">Updates</span></h6>

<ul>

 <li>For wis-untar if you are provided more than one argument you should print “wis-untar: tar-file” (followed by a newline) and exit with status 1.</li>

</ul>

<h2 id="administrivia">Administrivia</h2>

<ul>

 <li>Questions: We will be using Piazza for all questions.</li>

 <li>Collaboration: The assignment has to be done by yourself. Copying code (from others) is considered cheating. <a href="http://pages.cs.wisc.edu/~remzi/Classes/537/Spring2018/dontcheat.html">Read this</a> for more info on what is OK and what is not. Please help us all have a good semester by not doing this.</li>

 <li>This project is to be done on the <a href="https://csl.cs.wisc.edu/services/instructional-facilities">lab machines</a>, so you can learn more about programming in C on a typical UNIX-based platform (Linux).</li>

 <li>Some tests will be provided at <em>~cs537-1/tests/p1a</em>. Read more about the tests, including how to run them, by executing the command <code>cat ~cs537-1/tests/p1a/README</code> on any lab machine. Note these test cases are not complete, and you are encouraged to create more on your own.</li>

 <li>Handing it in: Copy your files to <em>~cs537-1/handin/login/p1a</em> where login is your CS login.</li>

</ul>

<h2 id="unix-utilities">Unix utilities</h2>

In this assignment, you will build a set of linux utilities but much simpler versions of common used commands like <strong>ls</strong>, <strong>cat</strong>, <strong>grep</strong> etc. We say simpler because to be mildly put the original are quite complicated! We will call each of these utilities slightly different to avoid confusion – <strong>wis-grep</strong>, <strong>wis-tar</strong>, <strong>wis-untar</strong>.

Learning Objectives:

<ul>

 <li>Re-familiarize yourself with the C programming language</li>

 <li>Familiarize yourself with a shell / terminal / command-line of UNIX</li>

 <li>Learn about how UNIX utilities are implemented</li>

</ul>

Summary of what gets turned in:

<ul>

 <li>Bunch of <em>.c</em> files, one each for a utility : <strong>wis-grep.c</strong>, <strong>wis-tar.c</strong>, <strong>wis-untar.c</strong></li>

 <li>Each should compile successfully when compiled with the -Wall and -Werror flags.</li>

 <li>Each should (hopefully) pass tests we supply.</li>

 <li>Include a single README.md for all the files describing the implementation.</li>

 <li>Assume that input files for each utility can be larger than RAM available on the system.</li>

</ul>

<strong>Before beginning</strong>: Read this <a href="http://pages.cs.wisc.edu/~remzi/OSTEP/lab-tutorial.pdf">lab tutorial</a>; it has some useful tips for programming in the C environment. Also read the <a href="http://pages.cs.wisc.edu/~shivaram/cs537-sp20/p1a-hints.html">hints</a> document to help you get started.

<h3 id="wis-grep">wis-grep</h3>

The first utility you will build is called <strong>wis-grep</strong>, a variant of the UNIX tool grep. This tool looks through a file, line by line, trying to find a user-specified search term in the line. If a line has the word within it, the line is printed out, otherwise it is not.

Here is how a user would look for the term foo in the file bar.txt:

<pre><code>	prompt&gt; ./wis-grep foo bar.txt	this line has foo in it	so does this foolish line; do you see where?	even this line, which has barfood in it, will be printed.</code></pre>

For hints on how to get started you can read more about how to open a file and read it in the <a href="http://pages.cs.wisc.edu/~shivaram/cs537-sp20/p1a-hints.html">hints document</a>.

Details

<ul>

 <li>Your program wis-grep is always passed a search term and zero or more files to grep through (thus, more than one is possible). It should go through each line and see if the search term is in it; if so, the line should be printed, and if not, the line should be skipped.</li>

 <li>The matching is case sensitive. Thus, if searching for <strong>foo</strong>, lines with <strong>Foo</strong> will not match.</li>

 <li>Lines can be arbitrarily long (that is, you may see many many characters before you encounter a newline character, 
). wis-grep should work as expected even with very long lines. For this, you might want to look into the <code>getline()</code> library call.</li>

 <li>If wis-grep is passed no command-line arguments, it should print “wis-grep: searchterm [file …]” (followed by a newline) and exit with status 1.</li>

 <li>If wis-grep encounters a file that it cannot open, it should print “wis-grep: cannot open file” (followed by a newline) and exit with status 1.</li>

 <li>In all other cases, wis-grep should exit with return code 0.</li>

 <li>If a search term, but no file, is specified, wis-grep should work, but instead of reading from a file, wis-grep should read from standard input. Doing so is easy, because the file stream stdin is already open; you can use fgets() (or similar routines) to read from it.</li>

 <li>For simplicity, if passed the empty string as a search string, wis-grep can either match NO lines or match ALL lines, both are acceptable.</li>

</ul>

<h2 id="wis-tar-and-wis-untar">wis-tar and wis-untar</h2>

The next two utilities you will build are simpler versions of tar and untar, which are commonly used UNIX utilities to combine (or expand) a collection of files into one file (one file into a collection of files). This functionality is useful in a number of scenarios e.g. offering a single file to download for software. (If you’ve heard the phrase tarball, that comes from using tar!)

The input to your <strong>wis-tar</strong> program will be the name of the tar file followed by a list of files that need to be archived (Fun Fact: The name tar comes from tape archives!). Example:

<pre><code>  prompt&gt; echo abcd &gt; a.txt # creates the file a.txt  prompt&gt; echo efgh &gt; b.txt # creates the file b.txt  prompt&gt; ./wis-tar test.tar a.txt b.txt</code></pre>

<h3 id="wis-tar-format">wis-tar format</h3>

For the purpose of this assignment we will use a simple file format for our tar file. Our format will be

<pre><code>  file1 name [100 bytes in ASCII]   file1 size [8 bytes as binary]  contents of file1 [in ASCII]  file2 name [100 bytes]  file2 size [8 bytes]  contents of file2 [in ASCII]  ...</code></pre>

Here are a few points that will help you with your implementation

<ol>

 <li>You can assume that the files provided as an input exist in the directory where the program is run from (no need to handle ways to store pesky path names).</li>

 <li>You can also assume the files provided as inputs only contain ASCII characters.</li>

 <li>You can read more about how to find the size of a file in <a href="http://pages.cs.wisc.edu/~shivaram/cs537-sp20/p1a-hints.html">hints document</a>.</li>

</ol>

Details

<ul>

 <li>If fewer than two arguments are supplied to your program then you should print “wis-tar: tar-file file […]” (followed by a newline) and exit with status 1.</li>

 <li>If any of the input files that should be a part of the tar file are not found you should print “wis-tar: cannot open file” (followed by a newline) and with exit status 1.</li>

 <li>If a tar-file of the same name already exists you can overwrite it with the new contents specified.</li>

 <li>If any of the input file names are longer than 100 characters, you can only use the first 100 characters as the filename to store in the tar format.</li>

 <li>If any of the input file names are shorter than 100 characters, you can pad the filename such that it uses 100 bytes. For example if the file name was “a.txt”, that only has 5 characters. In this case you can append 95 NULL i.e., <code> </code> characters to make the name use 100 bytes in the tar file. (Remember <code>' '</code> is not the same as <code>'0'</code>. The first one represents NULL while the second one represents the character 0!)</li>

</ul>

<strong>EXAMPLE</strong>: Lets look at a complete example to make sure we understand the format and how to understand the contents of a valid tar file. In the following example we first create a text file which contains the string <code>hey</code>. So its size is 3 (Remember this!).

Next we run <code>wis-tar</code> to create <code>a.tar</code> as shown below. Finally we print the contents of <code>a.tar</code> using <code>hexdump</code>, a utility to print the contents of a binary file. The comments to the right explain the output of hexdump. Remember that the bytes are represented in hexadecimal format, so handy table like <a href="https://www.ascii-code.com/">this</a> will help you lookup the ASCII values for strings. Try to see if you can decode the contents based on the comments on the right!

<pre><code>➜  p1a cat a.txthey%➜  p1a ./wis-tar a.tar a.txt➜  p1a hexdump -v a.tar0000000 2e61 7874 0074 0000 0000 0000 0000 0000  --&gt; The first five bytes here contain the file name a.txt0000010 0000 0000 0000 0000 0000 0000 0000 00000000020 0000 0000 0000 0000 0000 0000 0000 00000000030 0000 0000 0000 0000 0000 0000 0000 00000000040 0000 0000 0000 0000 0000 0000 0000 00000000050 0000 0000 0000 0000 0000 0000 0000 0000  ---&gt; We have padded using   till we hit 100 bytes0000060 0000 0000 0003 0000 0000 0000 6568 0079  ---&gt; The byte containing 03 indicates the file size is 3. Note that this is not in ASCII!----------------------------------------------------&gt; The last three bytes contain the string hey, the contents of the file</code></pre>

As you can see from the above description the fields in our tar file are packed together and there are no new lines or other separators between them.

<h3 id="wis-untar">wis-untar</h3>

As the name suggests, the wis-untar program will do the reverse of the wis-tar program. Here you will take in the name of an archive created using <strong>wis-tar</strong> and the program will create files corresponding to those in the archive in the same directory where the program is run from. For example

<pre><code>  prompt&gt; ./wis-untar test.tar  prompt&gt; ls # should contain a.txt and b.txt</code></pre>

Details

<ul>

 <li>If no arguments are provided then you should print “wis-untar: tar-file” (followed by a newline) and exit with status 1.</li>

 <li>If the tar filename provided on the command line doesn’t exist you should print “wis-untar: cannot open file” (followed by a newline) and exit with status 1.</li>

 <li>To simplify this assignment, you can assume that if a tar file is provided and if the file exists, it matches the wis-tar format described above.</li>

 <li>While extracting files, if there already exist files with the same name as those in the archive, you can overwrite them.</li>

</ul>