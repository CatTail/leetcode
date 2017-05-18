Rotate an array of n elements to the right by k steps.

For example, with n = 7 and k = 3, the array `[1,2,3,4,5,6,7]` is rotated to `[5,6,7,1,2,3,4]`.

Note:
Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.

### Simple loop

problem: Time Limit Exceeded

```python
def rotate(self, nums, k):
    n = len(nums)
    k = k % n
    for _ in range(k):
        last = nums[-1]
        for index in range(n - 1, 0, -1):
            nums[index] = nums[index - 1]
        nums[0] = last
```

### Extra clone

```python
def rotate(self, nums, k):
    clone = nums[:]
    n = len(nums)
    k = k % n
    for index in range(n):
        nums[index] = clone[index - k if (index - k) >= 0 else (index - k + n)]
```

### Who take my place

```python
def rotate(self, nums, k):
    # @var n, length of list
    # @var cursor, current calculate position
    # @var index, index will be swap into current position
    # (index + k) % n = cursor
    n = len(nums)
    k = k % n
    if k > 0:
        for cursor in range(n):
            index = cursor - k if (cursor - k) >= 0 else (cursor - k + n)
            while index < cursor:
                index = index - k if (index - k) >= 0 else (index - k + n)
            tmp = nums[index]
            nums[index] = nums[cursor]
            nums[cursor] = tmp
```

### GCD

```python
def GCD(self, max, min):
    if max < min:
        tmp = max
        max = min
        min = tmp
    return min if max % min == 0 else self.GCD(min, max % min)

def rotate(self, nums, k):
    n = len(nums)
    k = k % n
    if k > 0 and n > 1:
        for cursor in range(self.GCD(n, k)):
            last = nums[cursor]
            index = (cursor + k) % n
            while index != cursor:
                tmp = nums[index]
                nums[index] = last
                last = tmp
                index = (index + k) % n
            nums[cursor] = last
```

```
n = 3
k = 2
n / k = 1
n % k = 1

1 3 2

---

n = 3
k = 3
n / k = 1
n % k = 0

1 0 0
1 2 0
1 2 3

n = 4
k = 3
n / k = 1
n % k = 1

1 4 3 2

n = 5
k = 3
n / k = 1
n % k = 2

1 3 5 2 4

n = 6
k = 3
n / k = 2
n % k = 0

1 0 0 2 0 0
1 3 0 2 4 0
1 3 5 2 4 6

---

n = 5
k = 4
n / k = 1
n % k = 1

1 5 4 3 2

n = 6
k = 4
n / k = 1
n % k = 2

1 0 3 0 2 0
1 4 3 6 2 5

n = 7
k = 4
n / k = 1
n % k = 3

1 3 5 7 2 4 6

---

n = 6
k = 5
n / k = 1
n % k = 1

1 6 5 4 3 2

n = 7
k = 5
n / k = 1
n % k = 2

1 4 7 3 6 2 5

n = 8
k = 5
n / k = 1
n % k = 3

1 6 3 8 5 2 7 4

n = 9
k = 5
n / k = 1
n % k = 4

1 3 5 7 9 2 4 6 8

---

n = 8
k = 6
n / k = 1
n % k = 2

1 0 4 0 3 0 2 0
1 5 4 8 3 7 2 6

---

n = 12
k = 8
n / k = 1
n % k = 4

1 0 0 0 3 0 0 0 2 0 0 0
1 4 0 0 3 6 0 0 2 5 0 0
1 4 7 0 3 6 9 0 2 5 8 0
1 4 7 10 3 6 9 12 2 5 8 11
```

### Conclusion

Only G.C.D \(Greatest Common Divisor\) number of calculation required to rotate array.

