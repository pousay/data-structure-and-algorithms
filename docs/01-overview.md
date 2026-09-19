# Chapter 1, Overview

### Suppose you have a group of `N` numbers and would like to determine the `k`th largest.

This is known as the **selection problem**. Most students who have had a programming course or two would have no difficulty writing a program to solve this problem. There are quite a few “obvious” solutions.

#### 1 way to solve this problem would be to read the `N` numbers into an array, sort the array in decreasing order by some simple algorithm such as bubble sort, and then return the element in position `k`.

#### A somewhat better algorithm might be to read the first `k` elements into an array and sort them (in decreasing order). Next, each remaining element is read one by one. As a new element arrives, it is ignored if it is smaller than the kth element in the array. Otherwise, it is placed in its correct spot in the array, bumping one element out of the array. When the algorithm ends, the element in the kth position is returned as the answer.

> Both algorithms are simple to code, and you are encouraged to do so. The natural questions, then, are: Which algorithm is better? 

more important, Is either algorithm good enough? A simulation using a random file of 30 million elements and k = 15,000,000 will show that neither algorithm finishes in a reasonable amount of time; each requires several days of computer processing to terminate (albeit eventually with a correct answer).

An alternative method, discussed in **Chapter 7**, gives a solution in about a second. Thus, although our proposed algorithms work, they cannot be considered good algorithms, because they are entirely impractical for input sizes that a third algorithm can handle in a
reasonable amount of time.



## Summary

This chapter sets the stage for the rest of the book. The time taken by an algorithm confronted with large amounts of input will be an important criterion for deciding if it is a good algorithm. (Of course, correctness is most important.) We will begin to address these issues in the next chapter and will use the mathematics discussed here to establish a formal model.