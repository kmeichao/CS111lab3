## UID: 905694345
(IMPORTANT: Only replace the above numbers with your true UID, do not modify spacing and newlines, otherwise your tarfile might not be created correctly)

# Hash Hash Hash

This program uses mutex locks in a hash table implementation that is safe to use concurrently, preventing race conditions when using multiple threads.

## Building

To build, run "make" in the lab 3 directory.

## Running

To run, use './hash-table-tester -t [# of threads] -s [# of items]'

Example with 4 threads, 100,000 items:
Input: 
> ./hash-table-tester -t 4 -s 100000
Output:
> Generation: 74,612 usec
> Hash table base: 1,133,575 usec
> - 0 missing
> Hash table v1: 2,414,850 usec
> - 0 missing
> Hash table v2: 438,485 usec
> - 0 missing


## First Implementation

The first implementation strategy is to have the entire add entry function be treated as a critical section. This made it so one thread was able to access the function at a time. When two threads try to access the function, only one will be able to, locking the critical section and preventing any race conditions. This way, only one thread will be able to add to the linked list, or change the key. In order to prevent deadlock, the thread unlocks the critical section when it gets to the end of the function. This will allow another thread to access the function.

### Performance

Test 1 -
Input:
> ./hash-table-tester -t 4 -s 100000
Output:
> Generation: 74,612 usec
> Hash table base: 1,133,575 usec
> - 0 missing
> Hash table v1: 2,414,850 usec
> - 0 missing
> Hash table v2: 438,485 usec
> - 0 missing

Test 2 - 
Input:
> ./hash-table-tester -t 2 -s 200000
> Generation: 69,919 usec
> Hash table base: 1,102,626 usec
> - 0 missing
> Hash table v1: 1,802,932 usec
> - 0 missing
> Hash table v2: 711,901 usec
> - 0 missing

From the results in these test cases, you see that the case with the 4 threads has a worse preformance time compared to the case with the two thread case using v1 implementation. We can also see that v1 preforms slower than the base in both cases. The speed up time is .61 for 2 threads and the speed up time is 0.47 for 4 threads. Both speed up times show that v1 is slower thanthe base implementation. This makes sense because with the v1 implentation haivng more threads will cause more contex switches, making it slower.


## Second Implementation

The second implementation has a mutex lock on each hash table entry. The race condition occurs if two threads try to access the same bucket by adding to the same bucket which could lead to problems with the key that is added when using SLIST. If the threads are accessing different buckets, then there will be no problem with concurrency because they will not be changing the same part of the hash table. This implementation allows multiple threads to access the add entry function as long as they aren't accessing the same bucket. This is a valid strategy because locking will still occur for each smaller critical section, and the race condition will be removed.

### Performance

Using the test cases above, the speed up time using 4 threads is 3.08. This is a big increase compared to the v1 speed up times. This is because using v2 implementation, multiple threads can acces the hash map at a time, decreasing the amount of time it takes to add items into the hash map. V2 preformed better when increasing the number of threads to the amount of cores, which is 4. This makes sense because more threads will be able to access the hash map, making it faster to add entries.


## Cleaning up

To clean up, run 'make clean' in the lab 3 directory. If you run the pyhton test suite, then run 'rm -r __pycache__' to delete the left over file. The test suite automatically runs 'make clean'.
