---
layout: post
title: Searching Algorithms
image: 
tags: [python, search, algorithms]
---

## 1. Linear or sequential search
The linear search is used to find an item in a list. The items do not have to be in order. To search for an item, start at the beginning of the list and continue searching until either the end of the list is reached or the item is found.


```python

#Given a list called 'list_of_items' and looking for an item called 'item'

def linear_search(list_of_items, item):
    '''
    Returns whether the item is present in the list or not and the position of item in the list.
    If the item is not found the position returned is equal to the lenght of the list.
    
    '''
    
    #initially position=0 and item is not found
    position=0
    found=False
    
    #position is less than the length of list and item is not found 
    while position < len(list_of_items) and not found:
        if list_of_items[position] == item:
            found = True
        else:
            position = position + 1
    
    return found, position


#test the linear_search() method
my_list=[1,2,3,4,6,7,8, 'apple', 'orange', 'w']
my_items=[2, 5, 'apple', 'q']

for item in my_items:
    print(linear_search(my_list, item))

    
```

output:
```
(True, 1)
(False, 10)
(True, 7)
(False, 10)

```
## 2. Binary Search
The binary search is used to find an item in an ordered list.
To search for an item, look in the middle of the list and see if the 
number you want is in the middle, above the middle or below the middle. 
If it is in the middle, you have found the item. 
If it is higher than the middle value, then adjust the bottom of the list so that 
you search in a smaller list starting one above the middle of the list. If the 
number is lower than the middle value, then adjust the top of the list so that you 
search in a smaller list which has its highest position one less than the middle position.
---
```python
#Given a list called 'list_of_items' and looking for an item called 'item'

def binary_search(list_of_items, item):
    '''Returns whether the given item is found in the list or not using binary search. 
    The list has to be sorted.
    '''
    #intially not found
    found=False
    #set initial first and last
    left = 0
    right = len(list_of_items)-1
    
    
    while left <= right and not found:
        
        mid = ( left + right ) // 2
        
        # if middle of the list is equal to the item
        if list_of_items[mid] == item:
            found = True
        
        else:
            #if middle of list is smaller than the item
            if list_of_items[mid] < item:
                left = mid + 1
            #if middle of the list is larger than the item    
            else:
                right = mid - 1
    
    return found


#testing binary search
my_list = [2, 7, 9, 23, 56, 90, 132]

my_items=[11, 2, 56, 156]

for item in my_items:
    isFound = binary_search(my_list, item)
    if isFound:
        print("{} is found".format(item))
    else:
        print("{} is not found".format(item))

```

Output:
```
11 is not found
2 is found
56 is found
156 is not found

```

Video references:

<iframe
            width="400"
            height="300"
            src="https://www.youtube.com/embed/Ui97-_n5xjo"
            frameborder="0"
            allowfullscreen
        ></iframe>
