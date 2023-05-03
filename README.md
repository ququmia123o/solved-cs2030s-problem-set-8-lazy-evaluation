Download Link: https://assignmentchef.com/product/solved-cs2030s-problem-set-8-lazy-evaluation
<br>



<ol>

 <li>Let’s explore the difference in computational efficiency between using ArrayList (which employs eager computation) and LazyList (which employs lazy computation). To do this, we will find the <em>k<sup>th </sup></em>prime number in a given interval of integers [<em>a,b</em>] (eg. the 4<em><sup>th </sup></em>prime number in [2<em>,</em>100] is 7) using the two different types of lists.</li>

</ol>

Study the program listed in Primes.java; in particular, compare the methods findKthPrimeLL and findKthPrimeArr. Both use the same functional programming approach: (i) generate a list of integers in the given range, (ii) filter the list using the isPrime predicate, and (iii) retrieve the <em>k<sup>th </sup></em>element from the filtered list.

The program is already written and compiled for you; you just need to run it. To do so:

<ol>

 <li>Read the Appendix for instructions on how to setup.</li>

 <li>In the processed/ directory, run the program:</li>

</ol>

java Primes 100000 200000 5 1

This will find the 5<em><sup>th </sup></em>prime in the interval [100000<em>,</em>200000] using LazyList. The program will report the prime number found, and also the number of times isPrime was called:

The 5-th prime is 100057 isPrime was called 58 times

<ol start="3">

 <li>Type: java Primes to read further instructions on how to run the program.</li>

 <li>By choosing between the 2 methods of finding primes, compare how many timesisPrime is invoked. Try large ranges to see the difference, eg. there are over 8000 primes in the interval [100000<em>,</em>200000].<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a></li>

</ol>

Answer these questions:

<ul>

 <li>To find the 5<em><sup>th </sup></em>prime in the interval [100000<em>,</em>200000], why does findKthPrimeLL make so many fewer calls to isPrime compared to findKthPrimeArr ?</li>

 <li>Does the number of calls depend on <em>k</em>, or on the interval [<em>a,b</em>], or both?</li>

 <li>Can findKthPrimeLL make more calls to isPrimes than findKthPrimeArr? Epxlain.</li>

</ul>

<ol start="2">

 <li>Two LazyLists may be <em>concatenated</em>, ie. a new LazyList is created whose elements are those in the first list, in order, followed by those in the second list, in order. Example:</li>

</ol>

var s1 = LazyList.fromList(1, 2, 3); var s2 = LazyList.fromList(4, 5); s1.print();

=&gt; (* 1, 2, 3, *)

s2.print();

=&gt; (* 4, 5, *)

s1.concat(s2).print();

=&gt; (* 1, 2, 3, 4, 5, *)

Note that concat will not work if the lists are infinite. Here’s the code:

public LazyList&lt;T&gt; concat(LazyList&lt;T&gt; other) { if (this.isEmpty()) return other;

else return LLmake(this.head(), this.tail().concat(other));

}

<ul>

 <li>Using concat, add an instance method reverse() to the LazyList class. Since LazyLists are immutable, reverse() should return a new list, whose elements are the elements of this list, but in reverse order.</li>

 <li>Try your code on: fromList(1, 2, 3, 4, 5).reverse().print()</li>

 <li>Complete the code below to create a flatmap instance method. <em>Hint: </em>Use concat.</li>

</ul>

/** **************

Apply the function f onto each element of this list, and return a new LazyList containing all the flattened mapped elements. Note that f produces a list for each element. But the returned list flattens them all, ie. removes nested lists.

*/

&lt;R&gt; LazyList&lt;R&gt; flatmap(Function&lt;T, LazyList&lt;R&gt;&gt; f) { if (this.isEmpty()) return LazyList.makeEmpty();

else

// insert code here

}

<ul>

 <li>An <em>r</em>−Permutation of a list of <em>n </em>integers is a arrangement <em>r </em>integers, taken without repetition from the list. Example: (3<em>,</em>1<em>,</em>2) is a 3-Permutation of (1<em>,</em>2<em>,</em>3<em>,</em>4); a different 3-Permutation is (2<em>,</em>3<em>,</em>1). Here’s how we may generate all the <em>r</em>−Permutations of a list <em>L </em>of <em>n </em>

  <ol>

   <li>If <em>r </em>= 1, then this is a list of singleton lists of each of the elements in <em>L</em>. Example: for <em>L </em>= (1<em>,</em>2<em>,</em>3<em>,</em>4), there are four 1-Permutations: ((1)<em>,</em>(2)<em>,</em>(3)<em>,</em>(4)).</li>

   <li>If <em>r &gt; </em>1, take each element <em>x </em>in turn from <em>L</em>, recursively compute the (<em>r </em>− 1)Permutation of <em>L </em>− <em>x </em>(ie. <em>L </em>with <em>x </em>removed). This will give a list of (<em>r </em>− 1)−Permutations. We now insert <em>x </em>to the front of each of these.</li>

  </ol></li>

</ul>

In the file Puzzle.java, complete the code implement the permute function.

LazyList&lt;Integer&gt; remove(LazyList&lt;Integer&gt; LL, int n) { return LL.filter(x-&gt; x != n);

}

LazyList&lt;LazyList&lt;Integer&gt;&gt; permute(LazyList&lt;Integer&gt; LL, int r) { if (r == 1)

return LL.map(x-&gt; LLmake(x, LazyList.makeEmpty()));

else

// Insert code here

}

<ul>

 <li>Try it on: permute(LazyList.intRange(1,5), 3).forEach(LazyList::print);</li>

</ul>

This should print <sup>4</sup><em>P</em><sub>3 </sub>= 24 3-permutations. Note that the forEach instance method applies the given Consumer function onto each element of the list.

Read the Appendix to see how to compile your code.

<ol start="3">

 <li>A cryptarithmetic puzzle is shown below. Each letter represents a distinct numeric digit. The problem is to find what each letter represents so that the mathematical statement is true.</li>

</ol>

In this example: <em>A </em>= 1<em>,B </em>= 8<em>,C </em>= 4<em>,X </em>= 0<em>,Y </em>= 2 is a solution to the puzzle. Other solutions are also possible, as you can easily determine.

(a) Let’s try to solve the cryptarithmetic problem below by brute force, ie. checking all the possibilities. First, generate all the 6-Permutations of the list of 10 digits (0 to 9). These 6-Permutations correspond to our choice of <em>C,H,M,P,U,Z</em>. Next, check each 6-Permutation to see if it satisfies the given equation.

Complete the definition of pzczSatisfies in Puzzle.java to solve the problem. To run it, increase the stack size of the Java Virtual Machine (JVM) to 8Mb as follows:

java -Xss8m Puzzle. Otherwise, you may run out of stack space. public class Puzzle {

static boolean pzczSatisfies(LazyList&lt;Integer&gt; term) {

// term is a list of 6 digits int c = term.get(0); int h = term.get(1); int m = term.get(2); int p = term.get(3); int u = term.get(4); int z = term.get(5);

//Insert your code here.

//Return true only when both m,p are not 0, //and the given equation is satisfied.

}

public static void main(String[] args) { permute(LazyList.intRange(1,10), 6)

.filter(Puzzle::pzczSatisfies)

.forEach(LazyList::print);

}

}

(b) Using a similar strategy, solve this problem:

<h1>Appendix: How to Compile</h1>

Code that use LLmake or freeze are non-standard Java. Special care is needed to compile it. This page explains how.

<ol>

 <li>Use bash on Windows Subsystem for Linux (WSL), or MacOS, or Ubuntu, and cd to your $HOME directory: cd ~</li>

 <li>Create two directories: mkdir bin recit8</li>

 <li>Set permission: chmod 755 bin; chmod 755 recit8</li>

 <li>Download recit8-code.zip from Luminus Files and copy it to ~/recit8/, which will henceforth be called $BASE_DIR. On WSL, it is best to copy using cp instead of the Windows File Explorer (which may corrupt the file).</li>

 <li>Unzip the file: unzip rec8-code.zip. You should see these files, and another directory called processed:</li>

</ol>

~/recit8=&gt; ls -R .:

LazyList.java Primes.java Puzzle.java jpp processed

./processed:

LazyList.class Primes.class

<ol start="6">

 <li>Make the jpp script executable: chmod 755 jpp</li>

 <li>Move the file jpp to your ~/bin directory: mv jpp ~/bin/</li>

 <li>Add bin to your PATH: export PATH=~/bin:$PATH</li>

 <li>Set your CLASSPATH to $BASE_DIR/processed.</li>

</ol>

export CLASSPATH=~/recit8/processed:$CLASSPATH

<ol start="10">

 <li>The last 2 export commands can be made permanent if you write them into your ~/.bashrc Otherwise you will have to export again the next time you start the bash shell.</li>

 <li>Use the jpp script instead of javac to compile your .java This will translate LLmake and freeze (if any) into standard Java code, and then invoke the javac compiler for you. The class files will be placed in the processed/ directory. For example:</li>

</ol>

jpp Puzzle.java

This will handle LLmake and freeze, <strong>AND </strong>invoke javac on Puzzle.java.

<ol start="12">

 <li>Always edit your .java files in $BASE_DIR/ and not those in processed/ (which will be overwritten by jpp). But if you wish to run javac with -Xlint:unchecked, then do this on the files in processed/.</li>

 <li>Note that the 2 class files in processed/ have been compiled, and are ready to run.</li>

</ol>

Type: java Primes to run it.

<a href="#_ftnref1" name="_ftn1">[1]</a> The Prime Counting Function, <em>π</em>(<em>x</em>), is the number of primes up to and including integer <em>x</em>. Try it here:

https://www.dcode.fr/prime-number-pi-count