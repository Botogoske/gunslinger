# Gunslinger Containers
This document shows off all available containers and data structures in gunslinger.

## Dynamic Array

`gs_dyn_array` is a generic, dynamic array of type T, which is defined by the user and is inspired GREATLY from Shawn Barret's [stretchy buffer](https://github.com/nothings/stb/blob/master/stretchy_buffer.h) implementation:

```c
gs_dyn_array(float) arr = NULL;  // Dyanmic array of type float
```

Since c99 does not provide templates, generics must be achieved using macros. `gs_dyn_array(T)` is a macro that evalutes to `T*`. The user must initialize it to `NULL` before using it, that way the underlying implementation knows to internally initialize it.

`gs_dyn_array` stores header information right before the actual array in memory for a table to describe properties of the array: 

```txt
array => [header_info][actual array data]
```

Where the header is a define as:
```c
typedef struct gs_array_header_t {
  uint32_t size;
  uint32_t capacity;
} gs_array_header_t;
```
The user maintains a pointer to the beginning of the data array in memory at all times.

Since `gs_dyn_array` evaluates to a simple pointer of type `T`, the array can be randomly accessed using the `[]` operator as you would with any stadnard c array. 

```c
float v = arr[v];
```

There are also provided functions for accessing information using this provided table. `gs_dyn_array` is the baseline for all other containers provided in gunslinger.

# API:
* Declaring array: 
```c
gs_dyn_array(T) arr = NULL;   // Create dynamic array of type T.
```
* Inserting/Randomly accessing data: 
```c
gs_dyn_array_push(array, T);    // Push data of type `T` into array
T val = array[i];               // Access data of type `T` at index `i`
T* valp = &array[i];            // Access pointer of data type `T` at index `i`
```
* Size/Capacity/Empty/Reserve/Clear/Free:
```c
uint32_t sz = gs_dyn_array_size(arr);       // Gets size of array. Return 0 if NULL.
uint32_t cap = gs_dyn_array_capacity(arr);  // Gets capacity of array. Return 0 if NULL.
bool is_empty = gs_dyn_array_empty(arr);    // Returns whether array is empty. Return true if NULL.
gs_dyn_array_reserve(arr, N);               // Reserves internal space in the array for N (uint32_t), non-initialized elements.
gs_dyn_array_clear(arr);                    // Clears all elements. Simply sets array size to 0.
gs_dyn_array_free(arr);                     // Frees array data calling `gs_free()` internally.
```
* Iterating data: 
```c
for (uint32_t i = 0; i < gs_dyn_array_size(arr); ++i) {     // Iterate size of array, access elements via index `i`
  T* vp = &arr[i];
}
```
## Hash Table
`gs_hash_table` is a generic hash table of key `K` and value `V`, and is inspired by Shawn Barret's [ds](https://github.com/nothings/stb/blob/master/stb_ds.h) library implementation. Both `K` and `V` can be defined by the user: 

```c
gs_hash_table(uint32_t, float) ht = NULL;   // Declares a hash table with K = uint32_t, V = float
```

## Slot Array

## Slot Map

## Byte Buffer

## Command Buffer