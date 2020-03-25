---
date: 2020-03-25 22:00:00
layout: post
title: Python Selection Sort
subtitle: Python 선택 정렬
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

# ** 선택 정렬 **
- Desc - O(n^2)의 복잡도를 가지며, 비교할 데이터가 많아지면 비교 횟수도 많아진다.
- 데이터가 많을때 피해야 하는 정렬 알고리즘 이다.
- Flow - 비교할 데이터 중 가장 작은 값을 배열 가장 앞으로 보낸다.

```python
temp = 0
cnt = 0
index = 0

intArray = [1, 9, 7, 8, 6, 5, 2, 4, 3, 0]

for i in range(0, len(intArray)):
    min = 9999

    for j in range(i, len(intArray)):
        if(min > intArray[j]):
            min = intArray[j]
            index = j
        cnt += 1
    # 2차 for문 내부에 들어가는것 보다는 연산이 좀 더 빠르다.
    temp = intArray[i]
    intArray[i] = intArray[index]
    intArray[index] = temp
        
    
print("비교횟수 : ", cnt)
print(intArray)
```