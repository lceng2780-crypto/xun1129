學號：11428229  姓名：曾立薰
模擬頁面網址： https://lceng2780-crypto.github.io/xun1129/sort-menu.html
# 排序法綜合報告

## 前言

本報告針對五種常見排序法進行理論說明、複雜度分析、程式模擬與效能比較，並結合實驗數據與個人心得，期望能深入理解各種排序演算法的特性與適用情境。

---

## 1. 五個排序法介紹

本章說明氣泡排序法、選擇排序法、插入排序法、合併排序法、快速排序法的基本原理與操作方式：

- **氣泡排序法（Bubble Sort）**：反覆走訪數列，兩兩比較相鄰元素，若順序錯誤則交換，每輪將最大（或最小）元素「浮」到末端，直到無需交換。
- **選擇排序法（Selection Sort）**：每輪從未排序區間選出最小值，與未排序區第一個元素交換，已排序區擴大，未排序區縮小，重複直到全部排序完成。
- **插入排序法（Insertion Sort）**：將第一個元素視為已排序，依序取出未排序元素，與已排序區從右向左比較，找到正確位置插入，較大元素右移。
- **合併排序法（Merge Sort）**：採用分治法，將序列不斷對半分割，直到每個子序列只剩一個元素，再將子序列兩兩合併成有序序列，最終合併成完整排序結果。
- **快速排序法（Quick Sort）**：採用分治法，選定一個基準值（pivot），將數列分為小於與大於基準值的兩部分，分別對兩部分遞迴排序，最後合併。

---

## 2. 複雜度分析

| 排序法       | 時間複雜度(最佳/平均/最壞) | 空間複雜度 | 穩定性 |
|--------------|----------------------------|------------|--------|
| 氣泡排序法   | O(n) / O(n²) / O(n²)       | O(1)       | 穩定   |
| 選擇排序法   | O(n²) / O(n²) / O(n²)      | O(1)       | 不穩定 |
| 插入排序法   | O(n) / O(n²) / O(n²)       | O(1)       | 穩定   |
| 合併排序法   | O(n log n) / O(n log n) / O(n log n) | O(n) | 穩定   |
| 快速排序法   | O(n log n) / O(n log n) / O(n²) | O(log n) | 不穩定 |

---

## 3. 數據模擬

以下以 Python 實作五種排序法，並以 10,000 個隨機整數進行排序，測量各自所需時間（單位：毫秒，僅供參考，實際請以本機執行結果為準）：

```python
import random, time

def bubble_sort(arr):
    n = len(arr)
    for i in range(n-1):
        for j in range(n-1-i):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]

def selection_sort(arr):
    n = len(arr)
    for i in range(n-1):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]

def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j]
            j -= 1
        arr[j+1] = key

def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result

def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[0]
    left = [x for x in arr[1:] if x < pivot]
    right = [x for x in arr[1:] if x >= pivot]
    return quick_sort(left) + [pivot] + quick_sort(right)

# 產生隨機數列
N = 10000
raw = [random.randint(1, 100000) for _ in range(N)]

for name, func in [
    ("Bubble Sort", bubble_sort),
    ("Selection Sort", selection_sort),
    ("Insertion Sort", insertion_sort),
]:
    arr = raw[:]
    t0 = time.time()
    func(arr)
    t1 = time.time()
    print(f"{name}: {1000*(t1-t0):.2f} ms")

for name, func in [
    ("Merge Sort", lambda arr: merge_sort(arr)),
    ("Quick Sort", lambda arr: quick_sort(arr)),
]:
    arr = raw[:]
    t0 = time.time()
    func(arr)
    t1 = time.time()
    print(f"{name}: {1000*(t1-t0):.2f} ms")
```

---

## 4. 結果呈現與比較

| 排序法       | 10,000 個數排序時間 (ms) | 時間複雜度 | 空間複雜度 | 穩定性 |
|--------------|--------------------------|------------|------------|--------|
| 氣泡排序法   | 依實測，約數萬~數十萬   | O(n²)      | O(1)       | 穩定   |
| 選擇排序法   | 依實測，約數萬~數十萬   | O(n²)      | O(1)       | 不穩定 |
| 插入排序法   | 依實測，約數萬~數十萬   | O(n²)      | O(1)       | 穩定   |
| 合併排序法   | 依實測，數十~數百        | O(n log n) | O(n)       | 穩定   |
| 快速排序法   | 依實測，數十~數百        | O(n log n) | O(log n)   | 不穩定 |

> 註：實際時間依電腦效能、Python 實作與資料分布而異，僅供參考。

---

## 5. 心得與結論

經過老師這次的教學，我更加的理解這五個排序法的原理，再加上可視化演示，更容易的理解各個排序法之間的差異性。經過網路上的資料顯示:氣泡、選擇、插入排序法雖然原理簡單、易於理解，適合教學與小型資料，但在大數據下效率極低。合併排序與快速排序法則能有效處理大量資料，尤其快速排序平均效率最佳，但最壞情況下可能退化。合併排序則在穩定性與效率間取得平衡，但需額外空間。選擇排序與快速排序皆為不穩定排序，若資料穩定性有要求需注意。實務上，排序法的選擇應根據資料規模、特性與應用需求做出判斷。除此之外，我還發現除了我查到的快速排序法，還有其他種排序法，例如:堆積排序法，將來如果有時間的話可以在上網了解一下原理。