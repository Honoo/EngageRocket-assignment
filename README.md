# EngageRocket Assignment

## Question 1 - TDD

For the first 2 test cases: 

```
def sort(array)
  return array
end
```

It returns the original array, so the first 2 cases will pass.

For the third test case:

```
def sort(array)
  if array[0] > array[1]
    array = swap(array, 0, 1)
  end

  return array
end

def swap(array, i, j)
  temp = array[i]
  array[i] = array[j]
  array[j] = temp

  return array
end
```

This algorithm simply compares and swaps 2 adjacent items into ascending order. Since the third test case only has 2 items, it will pass.

For the fourth test case:

```
def sort(array)
  for i in 0...array.size-1
    if array[i] > array[i+1]
      array = swap(array, i, i+1)
    end
  end

  return array
end

def swap(array, i, j)
  temp = array[i]
  array[i] = array[j]
  array[j] = temp

  return array
end
```

The algorithm has been generalized to an array of any size. It will finalize the position of the biggest item in the array. Since the test case only has 1 item that needs to be shifted (3), it will pass.

The next test case should require shifting more than 1 item. This can be demonstrated using:

```
assert_equal([1,2,3], sort(3,2,1))
```

This test case requires shifting both 3 and 1. The next iteration of the code is:

```
def sort(array)
  for i in 0...array.size-1
    for j in 0...array.size-(1+i)
      if array[j] > array[j+1]
        array = swap(array, j, j+1)
      end
    end
  end

  return array
end

def swap(array, i, j)
  temp = array[i]
  array[i] = array[j]
  array[j] = temp

  return array
end
```

The algorithm can now shift multiple items. It will repeatedly shift the biggest items to the right side of the array until there are no items left. The algorithm is now complete and no more test cases are required.
