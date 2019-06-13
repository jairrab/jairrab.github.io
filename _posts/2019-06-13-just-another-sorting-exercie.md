---
layout: post
title:  "Just Another Sorting Exercise (Kotlin)"
date:   2019-06-13 09:00:00
categories: [algorithm, kotlin]
comments: true
---
I've been doing some reviewing of time and space complexity analysis to assess diferrent ways of solving a problem. I decided to look at the age old ways of sorting an array of numbers. For this exercise, I have created a Github repository at [jairrab/KotlinSortingAlgorithmsExercise](https://github.com/jairrab/KotlinSortingAlgorithmsExercise). The code is written in Kotlin, so it was also an opportunity for me to update plenty of code resources available on the internet to the latest modern language trend.

Running the test driver file at [SortingExercise.kt](https://github.com/jairrab/KotlinSortingAlgorithmsExercise/blob/master/testlibrary/src/main/java/com/jairrab/testlibrary/SortingExercise.kt) will yield the following type of result:
```
Array size = 1000

Test #1= [BubbleSort, BuiltInSort, MergeSort, InsertionSort, QuickSortRandom, QuickSortEnd, QuickSortMiddle, QuickSortMiddle2], fastest=QuickSortMiddle2 slowest=InsertionSort
Test #2= [InsertionSort, MergeSort, QuickSortRandom, QuickSortEnd, BubbleSort, QuickSortMiddle, QuickSortMiddle2, BuiltInSort], fastest=QuickSortMiddle2 slowest=BubbleSort
Test #3= [BubbleSort, QuickSortRandom, QuickSortMiddle2, MergeSort, QuickSortEnd, BuiltInSort, InsertionSort, QuickSortMiddle], fastest=QuickSortEnd slowest=BubbleSort
Test #4= [InsertionSort, QuickSortMiddle, QuickSortMiddle2, BubbleSort, QuickSortEnd, QuickSortRandom, BuiltInSort, MergeSort], fastest=QuickSortEnd slowest=BubbleSort
Test #5= [QuickSortEnd, QuickSortMiddle, QuickSortMiddle2, InsertionSort, BuiltInSort, MergeSort, BubbleSort, QuickSortRandom], fastest=QuickSortEnd slowest=BubbleSort
Test #6= [QuickSortRandom, QuickSortMiddle2, InsertionSort, BubbleSort, BuiltInSort, QuickSortEnd, QuickSortMiddle, MergeSort], fastest=QuickSortEnd slowest=BubbleSort
Test #7= [QuickSortMiddle, BuiltInSort, BubbleSort, InsertionSort, MergeSort, QuickSortMiddle2, QuickSortEnd, QuickSortRandom], fastest=QuickSortMiddle2 slowest=BubbleSort
Test #8= [MergeSort, BubbleSort, QuickSortMiddle, QuickSortRandom, QuickSortEnd, QuickSortMiddle2, BuiltInSort, InsertionSort], fastest=QuickSortEnd slowest=BubbleSort
Test #9= [BubbleSort, QuickSortMiddle2, MergeSort, QuickSortRandom, BuiltInSort, InsertionSort, QuickSortEnd, QuickSortMiddle], fastest=QuickSortEnd slowest=BubbleSort
Test #10= [BuiltInSort, BubbleSort, QuickSortRandom, QuickSortMiddle2, QuickSortEnd, InsertionSort, QuickSortMiddle, MergeSort], fastest=QuickSortEnd slowest=BubbleSort
Test #11= [MergeSort, QuickSortMiddle, QuickSortEnd, QuickSortRandom, BubbleSort, InsertionSort, QuickSortMiddle2, BuiltInSort], fastest=QuickSortEnd slowest=BubbleSort
Test #12= [QuickSortMiddle, QuickSortMiddle2, BubbleSort, InsertionSort, MergeSort, QuickSortRandom, BuiltInSort, QuickSortEnd], fastest=QuickSortEnd slowest=BubbleSort
Test #13= [BubbleSort, InsertionSort, QuickSortEnd, BuiltInSort, QuickSortMiddle2, MergeSort, QuickSortMiddle, QuickSortRandom], fastest=QuickSortEnd slowest=BubbleSort
Test #14= [QuickSortMiddle2, BubbleSort, BuiltInSort, QuickSortMiddle, InsertionSort, QuickSortRandom, QuickSortEnd, MergeSort], fastest=QuickSortEnd slowest=BubbleSort
Test #15= [MergeSort, BuiltInSort, QuickSortMiddle2, QuickSortMiddle, InsertionSort, QuickSortRandom, BubbleSort, QuickSortEnd], fastest=QuickSortEnd slowest=BubbleSort

0.049ms for QuickSortEnd, 0.0% slower. 100.0% correct
0.055ms for QuickSortMiddle2, 13.0% slower. 100.0% correct
0.061ms for QuickSortMiddle, 24.8% slower. 100.0% correct
0.063ms for BuiltInSort, 30.1% slower. 100.0% correct
0.067ms for QuickSortRandom, 38.3% slower. 100.0% correct
0.167ms for MergeSort, 243.7% slower. 100.0% correct
0.514ms for InsertionSort, 956.9% slower. 100.0% correct
0.825ms for BubbleSort, 1595.8% slower. 100.0% correct
```
The tests are randomized and warm-up tests are added to eliminate testing order bias. The size of the array, the number of tests and warm-up tests can be configures in the test driver.

# QuickSort
## Using end of array as pivot
~~~ kotlin
class QuickSortEnd {
    fun sort(array: IntArray): IntArray {
        sort(array, 0, array.size - 1)
        return array
    }

    private fun sort(array: IntArray, begin: Int, end: Int) {
        if (begin < end) {
            val partitionIndex = partition(array, begin, end)
            sort(array, begin, partitionIndex - 1)
            sort(array, partitionIndex + 1, end)
        }
    }

    private fun partition(array: IntArray, begin: Int, end: Int): Int {
        var i = begin

        for (j in begin until end) {
            if (array[j] <= array[end]) {
                swap(array, i, j)
                i++
            }
        }

        swap(array, i, end)
        return i
    }
}
~~~
## Using middle element as pivot
~~~ kotlin
class QuickSortMiddle {
    fun sort(array: IntArray): IntArray {
        qsort(array, 0, array.size - 1)
        return array
    }

    fun qsort(array: IntArray, left_index: Int, right_index: Int) {
        if (left_index >= right_index) return

        var left = left_index
        var right = right_index
        // pivot selection
        val pivot = array[(left_index + right_index) / 2]

        // partition
        while (left <= right) {
            while (array[left] < pivot) left++
            while (array[right] > pivot) right--
            if (left <= right) {
                swap(array, left, right)
                left++
                right--
            }
        }

        // recursion
        qsort(array, left_index, right)
        qsort(array, left, right_index)
    }
}
~~~
## Using random element as pivot
~~~kotlin
class QuickSortRandom {

    private val random: ThreadLocalRandom = ThreadLocalRandom.current()

    fun sort(array: IntArray): IntArray {
        sort(array, 0, array.size - 1)
        return array
    }

    private fun sort(array: IntArray, begin: Int, end: Int) {
        if (end - begin <= 0) {
            return
        } else if (end - begin == 1) {
            if (array[begin] > array[end]) swap(array, begin, end)
        } else {
            val index = partition(array, begin, end)
            sort(array, begin, index - 1)
            sort(array, index, end)
        }
    }

    private fun partition(array: IntArray, begin: Int, end: Int): Int {
        var i = begin
        var j = end
        val pivot = random.nextInt(begin, end)

        while (i < j) {
            while (array[i] <= array[pivot]) {
                if (i == end) break
                i++
            }

            while (array[pivot] <= array[j]) {
                if (j == i) break
                j--
            }

            if (i == j) {
                if (i > pivot) {
                    if (array[pivot] > array[i]) swap(array, i, pivot)
                } else if (i < pivot) {
                    if (array[pivot] < array[i]) swap(array, i, pivot)
                }
                return i
            }

            swap(array, i, j)
        }

        throw Exception("Sorting error...")
    }
}
~~~
