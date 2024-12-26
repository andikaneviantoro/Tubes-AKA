# <p align="center"> Perbandingan Efisiensi Algoritma Pengurutan Bubble Sort: Pendekatan Iteratif dan Rekursif  </p>
<p align="center"> Anggota Kelompok:  </p>
<p align="center"> Andika Neviantoro (2311102167) </p>
<p align="center"> Irshad Benaya Fardeca (2311102199) </p>

## Kode Program

### 1. Iteratif
```
from memory_profiler import memory_usage
import time
import random
import pandas as pd
import matplotlib.pyplot as plt

# Define the Bubble Sort Iterative algorithm
def bubble_sort_iterative(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr

# Function to measure performance and memory usage
def measure_performance(sizes=[100,1000,10000]):
    results = []

    for size in sizes:
        # Generate a random array
        arr = [random.randint(1, size) for _ in range(size)]

        # Measure memory usage for Bubble Sort Iterative
        mem_usage = memory_usage((bubble_sort_iterative, (arr,)), interval=0.01)
        mem_usage = max(mem_usage)  # Get maximum memory usage during sorting

        # Measure time for Bubble Sort Iterative
        start_time = time.time()
        bubble_sort_iterative(arr)
        exec_time = time.time() - start_time

        # Store the results
        results.append({
            'Size': size,
            'Execution Time (s)': exec_time,
            'Memory Usage (MB)': mem_usage
        })

    return pd.DataFrame(results)

# Run performance and memory usage comparison
df = measure_performance()
print("\nPerformance Comparison Table:")
print(df.to_string(index=False))

# Create a plot for execution time comparison
plt.figure(figsize=(10, 6))
plt.plot(df['Size'], df['Execution Time (s)'], marker='o', label='Execution Time')
plt.xlabel('Input Size')
plt.ylabel('Time (seconds)')
plt.title('Bubble Sort Iterative: Execution Time')
plt.legend()
plt.grid(True)
plt.show()

# Create a plot for memory usage comparison
plt.figure(figsize=(10, 6))
plt.plot(df['Size'], df['Memory Usage (MB)'], marker='o', label='Memory Usage')
plt.xlabel('Input Size')
plt.ylabel('Memory Usage (MB)')
plt.title('Bubble Sort Iterative: Memory Usage')
plt.legend()
plt.grid(True)
plt.show()
```

### 2. Rekursif
```
import sys
from memory_profiler import memory_usage
import time
import random
import pandas as pd
import matplotlib.pyplot as plt

# Define the Bubble Sort Recursive algorithm
def bubble_sort_recursive(arr, n=None):
    if n is None:
        n = len(arr)  # Correct indentation for this line

    # Base case: if array size is 1, it's already sorted
    if n == 1:
        return arr

    # Bubble up the largest element in the current array portion
    for i in range(n - 1):
        if arr[i] > arr[i + 1]:
            arr[i], arr[i + 1] = arr[i + 1], arr[i]

    # Recursively call the function with a smaller subarray
    return bubble_sort_recursive(arr, n - 1)

# Function to measure performance and memory usage
def measure_performance(sizes=[100,1000,10000]):
    results = []

    for size in sizes:
        # Generate a random array
        arr = [random.randint(1, size) for _ in range(size)]

        # Increase the recursion limit to handle deeper recursion
        sys.setrecursionlimit(size + 100)

        # Measure memory usage for Bubble Sort Recursive
        mem_usage = memory_usage((bubble_sort_recursive, (arr,)), interval=0.01)
        mem_usage = max(mem_usage)  # Get maximum memory usage during sorting

        # Measure time for Bubble Sort Recursive
        start_time = time.time()
        bubble_sort_recursive(arr)
        exec_time = time.time() - start_time

        # Store the results
        results.append({
            'Size': size,
            'Execution Time (s)': exec_time,
            'Memory Usage (MB)': mem_usage
        })

    return pd.DataFrame(results)

# Run performance and memory usage comparison
df = measure_performance()
print("\nPerformance Comparison Table:")
print(df.to_string(index=False))

# Create a plot for execution time comparison
plt.figure(figsize=(10, 6))
plt.plot(df['Size'], df['Execution Time (s)'], marker='o', label='Execution Time')
plt.xlabel('Input Size')
plt.ylabel('Time (seconds)')
plt.title('Bubble Sort Recursive: Execution Time')
plt.legend()
plt.grid(True)
plt.show()

# Create a plot for memory usage comparison
plt.figure(figsize=(10, 6))
plt.plot(df['Size'], df['Memory Usage (MB)'], marker='o', label='Memory Usage')
plt.xlabel('Input Size')
plt.ylabel('Memory Usage (MB)')
plt.title('Bubble Sort Recursive: Memory Usage')
plt.legend()
plt.grid(True)
plt.show()
```

### 3. Gabungan Iteratif dan Rekursif
```
def combine_performance_tables(iterative_df, recursive_df):
    # Add a column to each DataFrame indicating the algorithm type
    iterative_df['Algorithm'] = 'Iterative'
    recursive_df['Algorithm'] = 'Recursive'

    # Combine the two DataFrames
    combined_df = pd.concat([iterative_df, recursive_df], ignore_index=True)

    # Sort by input size and algorithm for better readability
    combined_df = combined_df.sort_values(by=['Size', 'Algorithm']).reset_index(drop=True)

    return combined_df

# Example usage:
iterative_df = measure_performance()  # Measure for iterative
recursive_df = measure_performance()  # Measure for recursive

# Combine the performance tables
combined_df = combine_performance_tables(iterative_df, recursive_df)

# Print the combined table
print("\nCombined Performance Table:")
print(combined_df.to_string(index=False))

# Create a plot comparing execution times
plt.figure(figsize=(10, 6))
for algo in combined_df['Algorithm'].unique():
    subset = combined_df[combined_df['Algorithm'] == algo]
    plt.plot(subset['Size'], subset['Execution Time (s)'], marker='o', label=f'{algo} Execution Time')
plt.xlabel('Input Size')
plt.ylabel('Time (seconds)')
plt.title('Bubble Sort: Execution Time Comparison')
plt.legend()
plt.grid(True)
plt.show()

# Create a plot comparing memory usages
plt.figure(figsize=(10, 6))
for algo in combined_df['Algorithm'].unique():
    subset = combined_df[combined_df['Algorithm'] == algo]
    plt.plot(subset['Size'], subset['Memory Usage (MB)'], marker='o', label=f'{algo} Memory Usage')
plt.xlabel('Input Size')
plt.ylabel('Memory Usage (MB)')
plt.title('Bubble Sort: Memory Usage Comparison')
plt.legend()
plt.grid(True)
plt.show()
```
