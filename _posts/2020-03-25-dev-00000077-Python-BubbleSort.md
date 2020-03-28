---
date: 2020-03-25 22:00:00
layout: post
title: Python Bubble Sort
subtitle: Python 버블 정렬
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

# ** 버블 정렬 **

- 서로 인접한 두 원소를 검사하여 정렬하는 알고리즘
- 인접한 2개의 레코드를 비교하여 크기가 순서대로 되어 있지 않으면 서로 교환한다.
- 선택 정렬과 기본 개념이 유사하다.

- 버블 정렬은 1회전후 가장큰 데이터가 가장 마지막 위치에 정렬된다.
- 이렇게 회전수 만큼 비교 횟수를 - 해준다.
- 예) 비교값 5개 면 1회전후 비교횟수는 3번만 하면된다.

- 시간복잡도 : O(n^2)

```python
def bubbleSort(list):
    limit = len(list)
    temp = 0
    i = 0
    j = 0
    cnt = 0

    for i in range(0, limit-i):
        # print(list[i], end=' ')
        for j in range(0, limit-(i+1)):
            if list[j] > list[j+1]:
                temp = list[j]
                list[j] = list[j+1]
                list[j+1] = temp
    
    return list
            
    
arrayList = [1, 9, 7, 8, 6, 5, 2, 4, 3, 0]

arrayList = bubbleSort(arrayList)
print(arrayList)
```