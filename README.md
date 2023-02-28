## UID: 005736849
(IMPORTANT: Only replace the above numbers with your true UID, do not modify spacing and newlines, otherwise your tarfile might not be created correctly)

# Hash Hash Hash

Implementation of hash tables supporting concurrency in C.

## Building

Run the following command:
    make

## Running

Show an example run of your (completed) program on using the `-t` and `-s` flags
of a run where the base hash table completes in between 1-2 seconds.

./hash-table-tester -t 8 -s 50000
    Hash table base: 1,137,738 usec
    - 0 missing
    Hash table v1: 1,448,593 usec
    - 0 missing
    Hash table v2: 354,305 usec
    - 0 missing

## First Implementation

In my first implementation, I used a single mutex to protect access to the hash table by locking the mutex in the hash_table_v1_add_entry function after initializing the list_head data structure. Protecting access to the list_head data structure ensures that only one thread can access the hash table at a time to prevent race conditions and support concurrency. The mutex is released at the end of the function, after all the modifications are made.

### Performance

Run the tester such that the base hash table completes in 1-2 seconds.
Report the relative speedup (or slow down) with a low number of threads and a
high number of threads. Note that the amount of work (`-t` times `-s`) should
remain constant. Explain any differences between the two.

./hash-table-tester -t 4 -s 100000
    Hash table base: 1,062,137 usec
    - 0 missing
    Hash table v1: 1,950,849 usec
    - 0 missing
    Hash table v2: 333,528 usec
    - 0 missing

./hash-table-tester -t 8 -s 50000
    Hash table base: 1,078,931 usec
    - 0 missing
    Hash table v1: 1,355,034 usec
    - 0 missing
    Hash table v2: 381,824 usec
    - 0 missing



## Second Implementation

In my second implementation, I used multiple mutexes. Specifically, I used one mutex for each hash table entry to protect all the entries associated with hash_table_entry. I added a separate mutex to each hash_table_entry struct to achieve this. I locked the mutex after defining the hash_table_entry in the hash_table_v2_add_entry function to protect the critical section for each individual entry. I unlock the mutex at the end of the function after the modifications have been made. Using multiple locks improves performance by allowing multiple threads to modify different parts of the hash table at the same time, while protecting them from being modified by more than one thread at once. 

### Performance

Run the tester such that the base hash table completes in 1-2 seconds.
Report the relative speedup with number of threads equal to the number of
physical cores on your machine (at least 4). Note again that the amount of work
(`-t` times `-s`) should remain constant. Report the speedup relative to the
base hash table implementation. Explain the difference between this
implementation and your first, which respect why you get a performance increase.

./hash-table-tester -t 4 -s 100000
    Hash table base: 1,040,000 usec
    - 0 missing
    Hash table v1: 1,819,217 usec
    - 0 missing
    Hash table v2: 311,852 usec
    - 0 missing

./hash-table-tester -t 8 -s 50000
    Hash table base: 1,026,903 usec
    - 0 missing
    Hash table v1: 1,313,065 usec
    - 0 missing
    Hash table v2: 455,527 usec
    - 0 missing



## Cleaning up

Run the following command:
    make clean