# cs1302-ce28 More Paired Sorting Algorithm Analysis

In this class exercise, students will continue with their implementation 
and timing analysis of different algorithms for sorting an array.
Code and notes will be shared between group members via a private Git repository. 

## References and Prerequisites

* [Setting up your own GitHub Account](https://github.com/cs1302uga/cs1302-tutorials/blob/master/github-setup.md)
* [CSCI 1302 Algorithm Analysis Tutorial](https://github.com/cs1302uga/cs1302-tutorials/blob/master/algo-analysis.md)

## Questions

In your notes, clearly answer the following questions. These instructions assume that you are 
logged into the Nike server. 

**NOTE:** If a step requires you to enter in a command, please provide in your notes the full 
command that you typed to make the related action happen. If context is necessary (e.g., the 
command depends on your present working directory), then please note that context as well.

### Getting Started

1. **This exercise picks up from the end of [`cs1302-ce27`](https://github.com/cs1302uga/cs1302-ce27/).** 
   If your group did not complete it, then you should do that now.  

1. **If you have a new group member,** then please add them as a collaborator via the
   repository's GitHub website (click "Settings" → "Collaborators"). This will send them
   an invite that they can accept either via email or by visiting the repository's website 
   on GitHub. **After accepting the invite,** the new group member should clone the repository
   via the the "SSH" URL under "Quick setup" on the repository's GitHub website.

1. Once each group member has completed the Getting Started steps, 
   pick an ordering for the group members (e.g., Group Member 1, Group Member 2, etc.).
   If a step is being performed by one group member, then everyone is expected
   to watch, pay attention, and take notes.

<hr/>

## Exercise Steps

For this next checkpoint, we will have you implement a simple sorting algorithm called
[Selection Sort](https://en.wikipedia.org/wiki/Selection_sort). There are many different ways to
explain the execution of this algorithm. We will take the approach of breaking up the
algorithm into two methods, `selectMin` and `selectionSort` that work together to sort an array.

1. **Select Min Algo:** This method takes an array, two valid index positions `lo` and `hi` (both inclusive) 
   within the array such that `lo <= hi` and a `Comparator` that is used to perform comparisions. 
   The method iterates over the array from `lo` to `hi` (inclusive) and finds the minimum element 
   according to the ordering induced by the comparator (i.e., calling `c.compare`), then swaps that
   element with the element as index `lo`. 
   Here is the signature for the method:
   
   ```java
   public static <T> void selectMin(T[] array, int lo, int hi, Comparator<T> c)
   ```
   
   1. Here is an example of before and after calling `selectMin(array, 0, 4, Integer::compareTo)`
      on an array with elements `[ 2, 3, 1, 4, 5 ]`:
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 2, 3, 1, 4, 5 ]
      selectMin(array, 0, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
      ```
      
   1. Here is another example before and after calling `selectMin(array, 1, 4, Integer::compareTo)`
      on an array with elements `[ 1, 3, 2, 4, 5 ]` :
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
      selectMin(array, 1, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 1, 2, 3, 4, 5 ]
      ```
   
   This method gets its name from the idea that it repeatedly selects a minimum in the 
   specified range (i.e., from `lo` to `hi`). After a call to `selectMin`, 
   **the smallest value in the range is guaranteed to be at index `lo`.**

1. As a group, pick a **DRIVER.** (no repeats), then the have the **DRIVER** implement the `selectMin` method
   in `SelectionSort.java`. You may want to implement a static `swap` method to help you perform
   the swaps. Be sure to include some code in the `main` method to test the 
   implementation. Once your group is confident that the code compiles and runs correctly,
   have the **DRIVER** stage and commit `SelectionSort.java` to their local repository, then
   push the changes up to the repository on GitHub. Everyone else should pull the changes
   after that.

1. **Selection Sort Algo**: This method also takes an array, two valid index positions `lo` and `hi` (both inclusive) 
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
   
   1. Here is a trace of the algorithm, one row for each call to `selectMin`:
   
      | Before              | Call                         | After (Sorted `/` Unorted) |
      |---------------------|------------------------------|-----------------------------|
      | `[ 2, 5, 1, 3, 4 ]` | `selectMin(array, 0, 4, c);` | `[ 1/ 5, 2, 3, 4 ]`         |  
      | `[ 1, 5, 2, 3, 4 ]` | `selectMin(array, 1, 4, c);` | `[ 1, 2/ 5, 3, 4 ]`         |
      | `[ 1, 2, 5, 3, 4 ]` | `selectMin(array, 2, 4, c);` | `[ 1, 2, 3/ 5, 4 ]`         |
      | `[ 1, 2, 3, 5, 4 ]` | `selectMin(array, 3, 4, c);` | `[ 1, 2, 3, 4/ 5 ]`         |
      
   1. Here is an example before and after calling `selectionSort(array, 0, 4, Integer::compareTo)`
      on an array with elements `[ 5, 4, 2, 3, 1 ]`:
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 5, 4, 2, 3, 1 ]
      selectionSort(array, 0, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 1, 2, 3, 4, 5 ]
      ```
1. As a group, pick a _new_ **DRIVER.** (no repeats), then the have the **DRIVER** implement the `selectionSort` 
   method in `SelectionSort.java`. Be sure to include some code in the `main` method to test the 
   implementation. Once your group is confident that the code compiles and runs correctly,
   have the **DRIVER** stage and commit `SelectionSort.java` to their local repository, then
   push the changes up to the repository on GitHub. Everyone else should pull the changes
   after that.
   
1. View the condensed, graphical version of your Git log using `git adog`.

**CHECKPOINT**

1. In your notes, write down the source code for `selectMin` and `selectionSort`, then derive the
   timing functions for two different algorithm analyses of the **Selection Sort Algo**. Here,
   let the problem size be defined as `n = hi - lo + 1`. 
   
   1. What is `T(n)` for a call to `selectionSort` if the set of key processing steps includes
      only swap operations? Include the diagram for your derivation. As `selectionSort` calls
      `selectMin`, this will involve mathematical function composition.
      
   1. What is `T(n)` for a call to `selectionSort` if the set of key processing steps includes
      only comparison operations (i.e., calls to `c.compare`)? Include the diagram
      for your derivation. As `selectionSort` calls
      `selectMin`, this will involve mathematical function composition.
      
**CHECKPOINT**

<hr/>

For this next checkpoint, we will have you implement a simple sorting algorithm called
[Quicksort](https://en.wikipedia.org/wiki/Quicksort). There are many different ways to
explain the execution of this algorithm. We will take the approach of breaking up the
algorithm into two methods, `partition` and `quickSort` that work together to sort an array.

1. As a group, pick a **DRIVER.** (no repeats), then the have the **DRIVER** create the skeleton code
   for a basic driver class called `cs1302.sorting.QuickSort` (we'll write the algorithm in 
   there later), ensuring that the package statement is correct and the file compiles and 
   runs using Maven. Then, stage and commit the change to your local repository 
   **and push those changes to GitHub**.

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
   
   1. Here is an example of before and after calling `partition(array, 0, 2, 4, Integer::compareTo)`
      on an array with elements `[ 1, 3, 2, 4, 5 ]`:
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
      int newPivot = partition(array, 0, 2, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 1, 2, 5, 4, 3 ]
      System.out.println(newPivot);               // 1
      ```
      
   1. Here is an example of before and after calling `partition(array, 0, 1, 4, Integer::compareTo)`
      on an array with elements `[ 1, 3, 2, 4, 5 ]`:
      
      ```java
      System.out.println(Arrays.toString(array)); // [ 1, 3, 2, 4, 5 ]
      int newPivot = partition(array, 0, 1, 4, Integer::compareTo);
      System.out.println(Arrays.toString(array)); // [ 1, 2, 3, 4, 5 ]
      System.out.println(newPivot);               // 2
      ```
      
   1. Here is another example of before and after calling `partition(array, 0, 0, 5, Integer::compareTo)`
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
   **in its correct sorted position within the range.** Everything to left is less or equal to the 
   pivot value and everything to the right is greater than.

1. As a group, pick a **DRIVER.** (no repeats), then the have the **DRIVER** implement the `partition` 
   method in `QuickSort.java`. You may want to implement a static `swap` method to help you perform
   the swaps. Be sure to include some code in the `main` method to test the 
   implementation. Once your group is confident that the code compiles and runs correctly,
   have the **DRIVER** stage and commit `QuickSort.java` to their local repository, then
   push the changes up to the repository on GitHub. Everyone else should pull the changes
   after that.
   
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
   
   * **Midpoint:** Let `pivot = hi / 2 - lo / 2`. 
   
   * **Median of Three:** Let `pivot` be the median of `lo`, `hi`, and the midpoint `hi / 2 + lo / 2`.
   
   * **Randomized:** Let `pivot` be a random integer in the range `lo` to `hi` (both inclusive).
   
   In practice, the randomized version performs better *on average* than the other two techniques, 
   however, it is a little harder to analyze.

1. As a group, pick a _new_ **DRIVER.**, then the have the **DRIVER** implement the `quickSort` 
   method in `QuickSort.java`. Be sure to include some code in the `main` method to test the 
   implementation. Once your group is confident that the code compiles and runs correctly,
   have the **DRIVER** stage and commit `QuickSort.java` to their local repository, then
   push the changes up to the repository on GitHub. Everyone else should pull the changes
   after that.
   
1. View the condensed, graphical version of your Git log using `git adog`.

**CHECKPOINT**

1. In your notes, write down the source code for `partition`, then derive the
   timing functions for two different algorithm analyses of the **Partition Algo**. Here,
   let the problem size be defined as `n = hi - lo + 1`. 
   
   1. **Count Swaps:** What is `T(n)` for a call to `partition` if the set of key processing steps includes
      only swap operations? Include the diagram for your derivation. 
      
   1. **Count Comparisons:** What is `T(n)` for a call to `partition` if the set of key processing steps includes
      only comparison operations (i.e., calls to `c.compare`)? Include the diagram
      for your derivation. 
      
**CHECKPOINT**

<hr/>

[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

<small>
Copyright &copy; Michael E. Cotterell, Brad Barnes, and the University of Georgia.
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License</a> to students and the public.
The content and opinions expressed on this Web page do not necessarily reflect the views of nor are they endorsed by the University of Georgia or the University System of Georgia.
</small>
