# cs1302-ce28 More Paired Sorting Algorithm Analysis

![Approved for: Fall 2020](https://img.shields.io/badge/Approved%20for-Fall%202020-blueviolet)

In this class exercise, students will continue with their implementation 
and timing analysis of different algorithms for sorting an array.
Code and notes will be shared between group members via a private Git repository. 

## Course-Specific Learning Outcomes
* **LO3.d:** Apply pair-programming principles in a software-based project.
* **LO5.a:** Utilize a version control tool such as Git or Subversion to store 
and update source code in a multi-programmer software solution.
* **LO6.c:** (Partial) Implement, analyze, and assess combinations of searching/sorting 
algorithms such as linear search, binary search, quadratic sorts, and linearithmic sorts.

## References and Prerequisites

* [Setting up your own GitHub Account](https://github.com/cs1302uga/cs1302-tutorials/blob/master/github-setup.md)
* [CSCI 1302 Algorithm Analysis Tutorial](https://github.com/cs1302uga/cs1302-tutorials/blob/master/algo-analysis.md)

## Questions

In your notes, clearly answer the following questions. These instructions assume that you are 
logged into the Nike server. 

**NOTE:** If a step requires you to enter in a command, please provide in your notes the full 
command that you typed to make the related action happen. If context is necessary (e.g., the 
command depends on your present working directory), then please note that context as well.

<!--

![Extra Credit](ecop.png)

We will **double the points** that you earn for this exercise if you meet the following criteria:

1. Met with another student in the Spring 2020 CSCI 1302 online (e.g., via Zoom) to complete
   the pair programming aspects of this exercise; and
   
1. Completed all checkpoints.

The Git log in your submitted exercise will help us determine if you met these criteria.

-->

### Getting Started

1. **This exercise picks up from the end of [`cs1302-ce27`](https://github.com/cs1302uga/cs1302-ce27/).** 
   If you did not complete it, then you should do that now.  

1. To get the most out of this exercise, we encourage you to
   **form a group of exactly two people for this exercise.**
   
   * **Working in a group?** Some steps in this exercise need be done by each group member individually.
   If a step is being performed by one group member, then everyone is expected
   to watch, pay attention, and take notes.
   
   * **Working by yourself?** That's okay. If the instructions ask you to switch, then don't switch.
   
1. **If you are working with a new person,** then please add them as a collaborator via the
   repository's GitHub website (click "Settings" → "Collaborators"). This will send them
   an invite that they can accept either via email or by visiting the repository's website 
   on GitHub. **After accepting the invite,** the new group member should clone the repository
   via the the "SSH" URL under "Quick setup" on the repository's GitHub website.

<hr/>

## Exercise Steps

### Checkpoint 1 Steps

For this checkpoint, we will have you implement a simple sorting algorithm called
[Selection Sort](https://en.wikipedia.org/wiki/Selection_sort). **There are many different ways to
explain the execution of this algorithm.** We will take the approach of breaking up the
algorithm into two methods, `selectMin` and `selectionSort` that work together to sort an array.

**Select Min Algo:** This method takes an array, two valid index positions `lo` and `hi` (both inclusive) 
within the array such that `lo <= hi` and a `Comparator` that is used to perform comparisions. 
The method iterates over the array from `lo` to `hi` (inclusive) and finds the minimum element 
according to the ordering induced by the comparator (i.e., calling `c.compare`), then swaps that
element with the element as index `lo`. 

Here is the signature for the method:
   
   ```java
   public static <T> void selectMin(T[] array, int lo, int hi, Comparator<T> c)
   ```
   
   * Here is an example of before and after calling `selectMin(array, 0, 4, Integer::compareTo)`
     on an array with elements `[ 2, 3, 1, 4, 5 ]`:
      
     ```java
     System.out.println(Arrays.toString(array)); // [ 2, 3, 1, 4, 5 ]
     selectMin(array, 0, 4, Integer::compareTo);
     System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
     ```
      
   * Here is another example before and after calling `selectMin(array, 1, 4, Integer::compareTo)`
     on an array with elements `[ 1, 3, 2, 4, 5 ]` :
      
     ```java
     System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
     selectMin(array, 1, 4, Integer::compareTo);
     System.out.println(Arrays.toString(array)); // [ 1, 2, 3, 4, 5 ]
     ```
   
This method gets its name from the idea that it repeatedly selects a minimum in the 
specified range (i.e., from `lo` to `hi`). After a call to `selectMin`, 
**the smallest value in the range is guaranteed to be at index `lo`.**

1. **GROUP MEMBER 1:** implement the `selectMin` method in `SelectionSort.java`. 
   You may want to implement a static `swap` method to help you perform the swaps. 
   
1. **GROUP MEMBER 1:** Write some code in the `main` method of `SelectionSort.java` to test the 
   implementation of `selectMin`. Make sure you
   test a few different dataypes and vary the starting (`lo`) and ending (`hi`) indices. The output
   of your program should describe the test cases that are executing by including at least: 
      * the array contents before and after the call to `selectMin`
      * `hi`, `lo`
      * Descriptive text around the output describing what the user (TA or instructor) is seeing.
      
1. Once your group is confident that the code compiles and runs correctly,
   make sure your code passes the `checkstyle` audit, then stage and commit `SelectionSort.java` 
   to your local repository with tag `"28-checkpoint-1"`, then **push the changes** up to the repository 
   on GitHub. 
   
1. **GROUP MEMBER 2:** Pull the changes to your local copy of the repository.

1. **EVERYONE:** View the condensed, graphical version of your Git log using `git adog`.

<hr/>

![CP](https://img.shields.io/badge/Just%20Finished%20Checkpoint-1-success?style=for-the-badge)

<hr/>

### Checkpoint 2 Steps

**Selection Sort Algo**: This method also takes an array, two valid index positions `lo` and `hi` (both inclusive) 
within the array such that `lo <= hi` and a `Comparator` that is used to perform comparisions.
The method simply calls `selectMin(array, i, hi)` for all valid `i` values starting with `lo`, 
**in order**, except for `array.length - 1`. Here is the signature for the method:

   ```java
   public static <T> void selectionSort(T[] array, int lo, int hi, Comparator<T> c)
   ```

To sort an entire array of integers referred to by `array`, for example, you might call:
   
   ```java
   selectionSort(array, 0, array.length - 1, Integer::compareTo);
   ```
   
This method gets its name from the fact that uses repeated calls `selectMin` in order to sort the array. 
Visually, the algorithm works by breaking up the array into two subsequences: sorted and unsorted.
Initially, the sorted sequence contains a single element (i.e., the element as index `lo`) and the 
unsorted sequence is the remaining elements in the array. After each call to `selectMin`, we know that 
the smallest value in the range is guaranteed to be at the `lo` index passed to `selectMin`.
   
   * Here is a trace of the algorithm, one row for each call to `selectMin`:
   
     | Before              | Call                         | After (Sorted `/` Unorted) |
     |---------------------|------------------------------|-----------------------------|
     | `[ 2, 5, 1, 3, 4 ]` | `selectMin(array, 0, 4, c);` | `[ 1/ 5, 2, 3, 4 ]`         |  
     | `[ 1, 5, 2, 3, 4 ]` | `selectMin(array, 1, 4, c);` | `[ 1, 2/ 5, 3, 4 ]`         |
     | `[ 1, 2, 5, 3, 4 ]` | `selectMin(array, 2, 4, c);` | `[ 1, 2, 3/ 5, 4 ]`         |
     | `[ 1, 2, 3, 5, 4 ]` | `selectMin(array, 3, 4, c);` | `[ 1, 2, 3, 4/ 5 ]`         |
      
   * Here is an example before and after calling `selectionSort(array, 0, 4, Integer::compareTo)`
     on an array with elements `[ 5, 4, 2, 3, 1 ]`:
      
     ```java
     System.out.println(Arrays.toString(array)); // [ 5, 4, 2, 3, 1 ]
     selectionSort(array, 0, 4, Integer::compareTo);
     System.out.println(Arrays.toString(array)); // [ 1, 2, 3, 4, 5 ]
     ```
1. **GROUP MEMBER 2:** Implement the `selectionSort` method in `SelectionSort.java`. 

1. **GROUP MEMBER 2:** Write some code in the `main` method to test the implementation of `selectionSort`. 
   You can most likely use your `selectMin` tests - just change them to do a full `selectionSort`. Make sure 
   you test a few different dataypes and vary the starting (`lo`) and ending (`hi`) indices. The output
   of your program should describe the test cases that are executing by including at least: 
      * the array contents for each iteration of the algorithm
      * `hi`, `lo`
      * Descriptive text around the output describing what the user (TA or instructor) is seeing.

1. Once your group is confident that the code compiles and runs correctly,
   make sure your code passes the `checkstyle` audit, then stage and commit `SelectionSort.java` 
   to your local repository with tag `"28-checkpoint-2"`, then **push the changes** up to GitHub. 
   
1. **GROUP MEMBER 1:** Pull the changes to your local copy of the repository.
   
1. **EVERYONE:** View the condensed, graphical version of your Git log using `git adog`.
   
<hr/>

![CP](https://img.shields.io/badge/Just%20Finished%20Checkpoint-2-success?style=for-the-badge)

<hr/>

### Checkpoint 3 Steps

1. **GROUP MEMBER 1:** Open your `NOTES.md` file and add the following to the end of your Bubble Sort analysis (from `ce27`):

   ````

   ## Selection Sort Algo
   
   ```java
   void selectMin(array, lo, hi, c) {
       // REMEMBER, n = hi - lo + 1
       // REPLACE: WITH INSIDE OF YOUR selectMin METHOD
   } // selectMin
   ```
   
   ```java
   void selectionSort(array, lo, hi, c)
       // REMEMBER, n = hi - lo + 1
       // REPLACE: WITH INSIDE OF YOUR selectionSort METHOD
   } // selectionSort
   ```
   ````
   
   In this file, replace the comments labeled `REPLACE` with the bodies of your `selectMin` and `selectionSort`
   methods.
   
   * In Emacs? Use `C-x 3` to open a second buffer, `C-x o` to move to the other buffer, then
     `C-x f` to find/open the relevant `.java` file. Select and copy text as usual, then use
     `C-x o` to move back to the first buffer to paste.
   
   In this file, derive timing functions for two different algorithm analyses of the **Selection Sort Algo**. 
   Here, let the problem size be defined as `n = hi - lo + 1`. Use the following as a guide:
   
   1. What is `T(n)` for a call to `selectionSort` if the set of key processing steps includes
      only swap operations? Include the diagram for your derivation. As `selectionSort` calls
      `selectMin`, this will involve mathematical function composition.
      
   1. What is `T(n)` for a call to `selectionSort` if the set of key processing steps includes
      only comparison operations (i.e., calls to `c.compare`)? Include the diagram
      for your derivation. As `selectionSort` calls`selectMin`, this will involve mathematical 
      function composition.
      
1. **GROUP MEMBER 1:** Once your group is confident that your analysis is correct,
   stage and commit `NOTES.md` to your local repository
   with tag `"28-checkpoint-3"`, then **push the changes** up to GitHub.
   
1. **GROUP MEMBER 2:** Pull the changes to your local copy of the repository.
   
1. **EVERYONE:** Look at the `NOTES.md` file on GitHub.

<hr/>

![CP](https://img.shields.io/badge/Just%20Finished%20Checkpoint-3-success?style=for-the-badge)

<hr/>

### Checkpoint 4 Steps

For this checkpoint, we will have you implement a simple sorting algorithm called
[Quicksort](https://en.wikipedia.org/wiki/Quicksort). There are many different ways to
explain the execution of this algorithm. We will take the approach of breaking up the
algorithm into two methods, `partition` and `quickSort` that work together to sort an array.

1. **Partition Algo:** This method takes an array, three valid index positions `lo`, `pivot`, and 
   `hi` (all inclusive) within the array such that `lo <= pivot <= hi` and a `Comparator` that is 
   used to perform comparisions. The method records the element at `array[pivot]` then iterates over
   the array from `lo` to `hi` (inclusive), performing swaps in such a way that all values less than
   or equal to the recorded value are to left of that value in the range and all values greater
   than the recorded value are to the right of that value in the range. The process potentially causes 
   the position of the recorded value to change, so the method returns the new index of the recorded
   value. Here is the signature for the method:
   
   ```java
   public static <T> int partition(T[] array, int lo, int pivot, int hi, Comparator<T> c)
   ```
   
   Here is the pseudo code for the algorithm:
   
   ```
   algo partition(A, lo, pivot, hi) is
       swap A[pivot] with A[hi]
       i := lo
       for j := lo to hi - 1 do
           if A[j] < a[hi] then
               swap A[i] with A[j]
               i := i + 1
       swap A[i] with A[hi]
       return i
   ```
   
   * Here is an example of before and after calling `partition(array, 0, 2, 4, Integer::compareTo)`
     on an array with elements `[ 1, 3, 2, 4, 5 ]`:
      
     ```java
     System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
     int newPivot = partition(array, 0, 2, 4, Integer::compareTo);
     System.out.println(Arrays.toString(array)); // [ 1, 2, 5, 4, 3 ]
     System.out.println(newPivot);               // 1
     ```
      
   * Here is an example of before and after calling `partition(array, 0, 1, 4, Integer::compareTo)`
     on an array with elements `[ 1, 3, 2, 4, 5 ]`:
      
     ```java
     System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
     int newPivot = partition(array, 0, 1, 4, Integer::compareTo);
     System.out.println(Arrays.toString(array)); // [ 1, 2, 3, 4, 5 ]
     System.out.println(newPivot);               // 2
     ```
      
   * Here is another example of before and after calling `partition(array, 0, 0, 5, Integer::compareTo)`
     on an array with elements `[ 6, 11, 2, 4, 17, 5 ]`:
     
     ```java
     System.out.println(Arrays.toString(array)); // [ 6, 11, 2, 4, 17, 5 ]
     int newPivot = partition(array, 0, 0, 5, Integer::compareTo);
     System.out.println(Arrays.toString(array)); // [ 5, 2, 4, 6, 17, 11 ]
     System.out.println(newPivot);               // 3
     ```
   
   This method gets its name from the idea that it breaks up the array into two "partitions" on
   either side of the pivot value in the specified range (i.e., from `lo` to `hi`). After a call to
   `partition`, **the element at the index position returned by the method is guaranteed to be**
   **in its correct sorted position within the range.** Everything to left is less than or equal to the 
   pivot value and everything to the right is greater than.

1. **GROUP MEMBER 1:** create the skeleton code for a basic driver class called 
   `cs1302.sorting.QuickSort` (we'll write the algorithm in there later), ensuring that the 
   package statement is correct and the file compiles and 
   runs using Maven. 
   
1. **GROUP MEMBER 1:** implement the `partition` method in `QuickSort.java`. You may want to implement 
   a static `swap` method to help you perform the swaps. 
   
1. **GROUP MEMBER 1:** Write some code in the `main` method of `QuickSort.java` to test the 
   implementation of `partition`. Make sure you test a few different datatypes and vary the
   starting (`lo`) and (`hi`) indices. The output of your program should describe the test cases
   that are executing by including at least:
      * the array contents before and after the call to `partition`
      * the value for `pivot`, `hi`, and `lo`
      * Descriptive text around the output describing what the user (TA or instructor) is seeing.
      
1. Once your group is confident that the code compiles and runs correctly,
   make sure your code passes the `checkstyle` audit, then stage and commit `QuickSort.java` 
   to your local repository with tag `"28-checkpoint-4"`, then **push the changes** up to GitHub. 

1. **GROUP MEMBER 2:** Pull the changes to your local copy of the repository.
   
1. **EVERYONE:** View the condensed, graphical version of your Git log using `git adog`.

<hr/>

![CP](https://img.shields.io/badge/Just%20Finished%20Checkpoint-4-success?style=for-the-badge)

<hr/>
      
### Checkpoint 5 Steps

1. **Quicksort Algo:** This method also takes an array, two valid index positions `lo` and `hi` (both inclusive) 
   within the array such that `lo <= hi` and a `Comparator` that is used to perform comparisions.
   The method simply calls `partition(array, lo, pivot, hi)` to partition the array into two parts
   on either side of the new pivot index returned by `partition`, then sorts both sides recursively.
   Here is the signature for the method:

   ```java
   public static <T> void quickSort(T[] array, int lo, int hi, Comparator<T> c)
   ```

   To sort an entire array of integers referred to by `array`, for example, you might call:
   
   ```java
   quickSort(array, 0, array.length - 1, Integer::compareTo);
   ```
   
   Before you can call `partition` in `quickSort`, you need to pick an initial pivot index. There are multiple
   strategies for doing this. The hope is that whatever strategy you pick, the call to `partition`
   will split the range roughly in half (i.e., the new pivot index will be in the middle of the
   range):
   
   * **Midpoint:** Let `pivot = hi / 2 + lo / 2`. 
   
   * **Median of Three:** Let `pivot` be the median of `lo`, `hi`, and the midpoint `hi / 2 + lo / 2`.
   
   * **Randomized:** Let `pivot` be a random integer in the range `lo` to `hi` (both inclusive).
   
   In practice, the randomized version performs better *on average* than the other two techniques, 
   however, it is a little harder to analyze.

1. **GROUP MEMBER 2:** Implement the `quickSort` method in `QuickSort.java`. 
   
1. **GROUP MEMBER 2:** Write some code in the `main` method to test the implementation of `quickSort`. Make sure 
   you test a few different dataypes and vary the starting (`lo`) and ending (`hi`) indices. The output
   of your program should describe the test cases that are executing by including at least: 
      * the array contents before and after sorting
      * `hi`, `lo`
      * Descriptive text around the output describing what the user (TA or instructor) is seeing.

1. Once your group is confident that the code compiles and runs correctly,
   make sure your code passes the `checkstyle` audit, then stage and commit `QuickSort.java` 
   to your local repository with tag `"28-checkpoint-5"`, then **push the changes** up to GitHub. 
   
1. **GROUP MEMBER 1:** Pull the changes to your local copy of the repository.
   
1. **EVERYONE:** View the condensed, graphical version of your Git log using `git adog`.

<hr/>

![CP](https://img.shields.io/badge/Just%20Finished%20Checkpoint-5-success?style=for-the-badge)

<hr/>

### Checkpoint 6 Steps

1. **GROUP MEMBER 1:** Open your `NOTES.md` file and add the following to the end:

   ````

   ## Partition Algo
   
   ```java
   void partition(array, lo, pivot, hi, c) {
       // REMEMBER, n = hi - lo + 1
       // REPLACE: WITH INSIDE OF YOUR partition METHOD
   } // partition
   ```
   
   ````
   
   In this file, replace the comments labeled `REPLACE` with the body of your `partition` method.

   In this file, derive the timing functions for two different algorithm analyses of the **Partition Algo**. 
   Here, let the problem size be defined as `n = hi - lo + 1`. Use the following as a guide:
   
   1. **Count Swaps:** What is `T(n)` for a call to `partition` if the set of key processing steps includes
      only swap operations? Include the diagram for your derivation. 
      
   1. **Count Comparisons:** What is `T(n)` for a call to `partition` if the set of key processing steps includes
      only comparison operations (i.e., calls to `c.compare`)? Include the diagram
      for your derivation. 
   
1. **GROUP MEMBER 1:** Once your group is confident that your analysis is correct,
   stage and commit `NOTES.md` to your local repository
   with tag `"28-checkpoint-6"`, then **push the changes** up to GitHub.
   
1. **GROUP MEMBER 2:** Pull the changes to your local copy of the repository.
   
1. **EVERYONE:** Look at the `NOTES.md` file on GitHub.

<hr/>

![CP](https://img.shields.io/badge/Just%20Finished%20Checkpoint-6-success?style=for-the-badge)

<hr/>

### Submission Steps

**Each student needs to individually submit their own work.**

1. Create a plain text file called `SUBMISSION.md` directly inside the `cs1302-ce27-ce28`
   directory with the following information.

   1. Your name and UGA ID number;
   1. Collaborator names, if any; and
   1. If you created the API website, include the full link to the site you generated.
   
   Here is an example of the contents of `SUBMISSION.md`.
   
   ```
   1. Sally Smith (811-000-999)
   2. Collaborators: Joe Allen, Stacie Mack
   3. https://webwork.cs.uga.edu/~user/cs1302-ce27-ce28-doc
   ```

1. Change directories to the parent of `cs1302-ce27-ce28` (e.g., `cd ..` from `cs1302-ce27-ce28`). If you would like
   to make a backup tar file, the instructions are in the submissions steps for [ce02](https://github.com/cs1302uga/cs1302-ce02).
   We won't repeat those steps here and you can view them as optional.
   
1. Use the `submit` command to submit this exercise to `csci-1302`:
   
   ```
   $ submit cs1302-ce27-ce28 csci-1302
   ```
   
   Read the output of the submit command very carefully. If there is an error while submitting, then it will displayed 
   in that output. Additionally, if successful, the submit command creates a new receipt file in the directory you 
   submitted. The receipt file begins with rec and contains a detailed list of all files that were successfully submitted. 
   Look through the contents of the rec file and always remember to keep that file in case there is an issue with your submission.

   **Note:** You must be on Odin to submit.

<hr/>

![CP](https://img.shields.io/badge/Just%20Finished-Submission-success?style=for-the-badge)

<hr/>

[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

<small>
Copyright &copy; Michael E. Cotterell, Brad Barnes, and the University of Georgia.
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License</a> to students and the public.
The content and opinions expressed on this Web page do not necessarily reflect the views of nor are they endorsed by the University of Georgia or the University System of Georgia.
</small>
