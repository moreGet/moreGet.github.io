---
date: 2020-03-28 22:00:00
layout: post
title: Python Insert Sort
subtitle: Python 삽입 정렬
description: "Current Python : == 3.8.1(32bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - VisualStudioCode
  - VsCode
  - 자료구조
  - Collection
  - 알고리즘
  - 컬렉션
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

# ** 삽입 정렬 **

- 자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교 하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘
- 매 순서마다 해당 원소를 삽입할 수 있는 위치를 찾아 해당 위치에 넣는다.

## 삽입 정렬(insertion sort) 알고리즘의 구체적인 개념
- 삽입 정렬은 두 번째 자료부터 시작하여 그 앞(왼쪽)의 자료들과 비교하여 삽입할 위치를 지정한 후 자료를 뒤로 옮기고 지정한 자리에 자료를 삽입하여 정렬하는 알고리즘이다.
- 즉, 두 번째 자료는 첫 번째 자료, 세 번째 자료는 두 번째와 첫 번째 자료, 네 번째 자료는 세 번째, 두 번째, 첫 번째 자료와 비교한 후 자료가 삽입될 위치를 찾는다. 자료가 삽입될 위치를 찾았다면 그 위치에 자료를 삽입하기 위해 자료를 한 칸씩 뒤로 이동시킨다.
- 처음 Key 값은 두 번째 자료부터 시작한다.
- 이론 출처 : https://gmlwjd9405.github.io/2018/05/06/algorithm-insertion-sort.html

![삽입정렬](../assets/sources/insertSort.png "insertSort")


```python
def insertSort(list):
    length = len(list)
    i = 0
    j = 0
    temp = 0
    key = 0

    for i in range(1, length):
        key = list[i]
        # j가 정방향 일 경우
        # for j in range(0, i):
        #     if list[(i-1)-j] > key:
        #         temp = list[(i-1)-j]
        #         list[(i-1)-j] = key
        #         list[i-j] = temp

        # j가 역방향 일 경우
        for j in range(i-1, -2, -1):
            if (list[j] > key and j >= 0):
                list[j+1] = list[j]
            else:
                break
        list[j+1] = key

    return list

list = [1, 9, 7, 8, 6, 5, 2, 4, 3, 0]
list = insertSort(list)
print(list)
```