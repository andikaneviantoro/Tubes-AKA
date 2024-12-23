# Tugas Besar - Analisis Kompleksitas Algoritma
### Kode Program
```
import time
import random
import matplotlib.pyplot as plt
from prettytable import PrettyTable

# Bubble Sort Rekursif
def bubble_sort_recursive(arr, n=None):
    if n is None:
        n = len(arr)
    if n == 1:
        return
    for i in range(n - 1):
        if arr[i] > arr[i + 1]:
            arr[i], arr[i + 1] = arr[i + 1], arr[i]
    bubble_sort_recursive(arr, n - 1)

# Bubble Sort Iteratif
def bubble_sort_iterative(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]

# Grafik untuk menyimpan data
array_sizes = []
recursive_times = []
iterative_times = []

# Fungsi untuk memperbarui grafik
def update_graph():
    plt.figure(figsize=(8, 6))
    plt.plot(array_sizes, recursive_times, label='Recursive', marker='o', linestyle='-')
    plt.plot(array_sizes, iterative_times, label='Iterative', marker='o', linestyle='-')
    plt.title('Performance Comparison: Recursive vs Iterative Bubble Sort')
    plt.xlabel('Array Size')
    plt.ylabel('Execution Time (seconds)')
    plt.legend()
    plt.grid(True)
    plt.show()

# Fungsi untuk mencetak tabel waktu eksekusi
def print_execution_table():
    table = PrettyTable()
    table.field_names = ["Array Size", "Recursive Time (s)", "Iterative Time (s)"]
    min_len = min(len(array_sizes), len(recursive_times), len(iterative_times))
    for i in range(min_len):
        table.add_row([array_sizes[i], recursive_times[i], iterative_times[i]])
    print(table)

# Program utama
while True:
    try:
        size = int(input("Masukkan ukuran array (atau ketik -1 untuk keluar): "))
        if size == -1:
            print("Program selesai. Terima kasih!")
            break
        if size <= 0:
            print("Masukkan ukuran array yang positif!")
            continue

        array_sizes.append(size)
        
        # Buat array acak
        random_array = [random.randint(1, 1000) for _ in range(size)]
        
        # Ukur waktu eksekusi Bubble Sort rekursif
        arr_copy = random_array[:]
        start_time = time.time()
        bubble_sort_recursive(arr_copy)
        recursive_times.append(time.time() - start_time)

        # Ukur waktu eksekusi Bubble Sort iteratif
        arr_copy = random_array[:]
        start_time = time.time()
        bubble_sort_iterative(arr_copy)
        iterative_times.append(time.time() - start_time)

        # Cetak tabel waktu eksekusi
        print_execution_table()

        # Perbarui grafik
        update_graph()

    except ValueError:
        print("Masukkan ukuran array yang valid!")

```
