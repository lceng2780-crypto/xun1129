# 排序法報告

本報告介紹五種常見排序法：氣泡排序法、選擇排序法、插入排序法、合併排序法、快速排序法。內容包含簡述原理、複雜度分析與模擬實作。

---

## 1. 氣泡排序法（Bubble Sort）

**原理簡述：**
- 反覆走訪數列，兩兩比較相鄰元素，若順序錯誤則交換。
- 每輪將最大（或最小）元素「浮」到末端。
- 重複直到無需交換。

**複雜度分析：**
- 最佳：$O(n)$（已排序）
- 平均/最壞：$O(n^2)$
- 空間：$O(1)$
- 穩定排序

**模擬實作（Python）：**
```python
arr = [5, 2, 8, 1, 9]
n = len(arr)
for i in range(n-1):
    for j in range(n-1-i):
        if arr[j] > arr[j+1]:
            arr[j], arr[j+1] = arr[j+1], arr[j]
print(arr)
```

---

## 2. 選擇排序法（Selection Sort）

**原理簡述：**
- 每輪從未排序區間選出最小值，與未排序區第一個元素交換。
- 已排序區擴大，未排序區縮小。
- 重複直到全部排序完成。

**複雜度分析：**
- 最佳/平均/最壞：$O(n^2)$
- 空間：$O(1)$
- 不穩定排序

**模擬實作（Python）：**
```python
arr = [5, 2, 8, 1, 9]
n = len(arr)
for i in range(n-1):
    min_idx = i
    for j in range(i+1, n):
        if arr[j] < arr[min_idx]:
            min_idx = j
    arr[i], arr[min_idx] = arr[min_idx], arr[i]
print(arr)
```

---

## 3. 插入排序法（Insertion Sort）

**原理簡述：**
- 將第一個元素視為已排序。
- 依序取出未排序元素，與已排序區從右向左比較，找到正確位置插入。
- 較大元素右移，重複直到全部排序完成。

**複雜度分析：**
- 最佳：$O(n)$（已排序）
- 平均/最壞：$O(n^2)$
- 空間：$O(1)$
- 穩定排序

**模擬實作（Python）：**
```python
arr = [5, 2, 8, 1, 9]
for i in range(1, len(arr)):
    key = arr[i]
    j = i - 1
    while j >= 0 and arr[j] > key:
        arr[j+1] = arr[j]
        j -= 1
    arr[j+1] = key
print(arr)
```

---

## 4. 合併排序法（Merge Sort）

**原理簡述：**
- 採用分治法，將序列不斷對半分割，直到每個子序列只剩一個元素。
- 將子序列兩兩合併成有序序列，最終合併成完整排序結果。

**複雜度分析：**
- 最佳/平均/最壞：$O(n \log n)$
- 空間：$O(n)$
- 穩定排序

**模擬實作（Python）：**
```python
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

arr = [5, 2, 8, 1, 9]
print(merge_sort(arr))
```

---

## 5. 快速排序法（Quick Sort）

**原理簡述：**
- 採用分治法，選定一個基準值（pivot），將數列分為小於與大於基準值的兩部分。
- 分別對兩部分遞迴排序，最後合併。

**複雜度分析：**
- 最佳/平均：$O(n \log n)$
- 最壞：$O(n^2)$（已排序或逆序時）
- 空間：$O(\log n)$（遞迴棧）
- 不穩定排序

**模擬實作（Python）：**
```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[0]
    left = [x for x in arr[1:] if x < pivot]
    right = [x for x in arr[1:] if x >= pivot]
    return quick_sort(left) + [pivot] + quick_sort(right)

arr = [5, 2, 8, 1, 9]
print(quick_sort(arr))
```

---

## 綜合比較

| 排序法       | 時間複雜度(最佳/平均/最壞) | 空間複雜度 | 穩定性 | 適用情境           |
|--------------|----------------------------|------------|--------|--------------------|
| 氣泡排序法   | O(n) / O(n²) / O(n²)       | O(1)       | 穩定   | 教學、小型資料     |
| 選擇排序法   | O(n²) / O(n²) / O(n²)      | O(1)       | 不穩定 | 簡單場景           |
| 插入排序法   | O(n) / O(n²) / O(n²)       | O(1)       | 穩定   | 小型、幾乎已排序   |
| 合併排序法   | O(n log n) / O(n log n) / O(n log n) | O(n) | 穩定   | 大型資料、穩定需求 |
| 快速排序法   | O(n log n) / O(n log n) / O(n²) | O(log n) | 不穩定 | 大型資料、效率需求 |

- **氣泡排序法**：最直觀，適合初學者理解排序概念，但效率最低。
- **選擇排序法**：交換次數少，實作簡單，但同樣效率不高且不穩定。
- **插入排序法**：對小型或幾乎已排序資料表現佳，簡單且穩定。
- **合併排序法**：分治法代表，效率穩定且穩定排序，但需額外空間。
- **快速排序法**：平均效率最佳，實務應用廣泛，但最壞情況下效率低且不穩定。

## 心得

透過本次五種排序法的學習與模擬實作，可以明顯感受到不同演算法在設計思路、效率與適用場景上的差異。氣泡、選擇、插入排序法雖然簡單易懂，適合教學與小型資料，但在資料量大時效率明顯不足。合併與快速排序法則展現了分治法的強大威力，能有效處理大量資料，尤其快速排序在實務上應用最廣。不過，選擇排序與快速排序都屬於不穩定排序，若資料穩定性有要求則需特別注意。整體而言，選擇合適的排序法需根據資料特性、規模與應用需求做出判斷。