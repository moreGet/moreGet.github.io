---
layout: post
title:  "빅데이터 분석 기초"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# BigData 기초 이론

> 빅데이터는 스케일이 큰 자료를 말한다.<br>
> 이것을 토대로 분석가들은 직관적으로 빅 데이터를 대하여야 한다.<br>

<br>

---
## 빅데이터 분석 순서
- 수집 -> 처리 -> 저장 -> 분석 -> 시각화<br><br>

- 전처리 : 결측치, 극단치, 이상치
  - 결측치 : 수집한 데이터의 중요한 정보가 결여되어 데이터가 쓸모 없는 경우 (사람 정보에 나이가 없는경우)
  - 극단치 : 수집한 데이터의 중요한 컬럼의 수치가 극단적으로 수치가 변형된경우 (나이가 500살로 잘못 수집 된경우)
  - 이상치 : 데이터가 모든 면에서 이상적인 경우
  
- 필터링 : 튜플 단위의 필요한 데이터만 추출하는 기술(정규식)
- 데이터 분리 : 필터링 개념
- 데이터 구조 : 벡터, 행렬, DataFrame<br><br>
- 데이터 유형의 따른 저장
  - 정형 데이터(maria db, oracle 등 순서가 명확한, 스프레드시트)
  - 반정형 데이터(HTML, XML, JSON 등)
  - 비정형 데이터(소셜 데이터, 문서(워드, 한글), 이미지, 오디오, 비디오 등...)
- 분류규칙 : 분류별 특성을 찾아 모형을 만들고 이를 토대로 분류 값을 예측 함
- 군집화 : 다양한 특성을 가진 다수의 집단에 대하여 특성을 가진 집단으로 분류
- 상관 관계 분석 : 하나의 변수가 다른 변수와 얼마 정도의 관련 성이 있는 지를 개관 하는 분석 방법
- 텍스트 마이닝 : 연관규칙 / 의사 결정 트리
- 머신러닝(기계학습) : 선형회귀/로지스틱 회귀/소프트 맥스/인공 신경망/CNN/RNN
- 추천 : 특정 데이터를 기반으로 다른 데이터를 추천하는 방법