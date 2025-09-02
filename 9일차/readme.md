
# 📊 Tableau Bootcamp 고급편 - 9일차 퀘스트(과제)

## 📚 9일차 학습 안내
Tableau Bootcamp 9일차에 도착했습니다!  

오늘의 주제는 **집합(Set)** 입니다.  
수학에서의 집합처럼, Tableau에서도 특정 조건을 만족하는 데이터를 **IN / OUT** 으로 구분하여 분석할 수 있습니다.  

---

## 1. Tableau 집합이란?
- 집합(Set): 특정 조건을 만족하는 데이터 묶음  
- IN: 조건을 만족하는 데이터(멤버)  
- OUT: 조건을 만족하지 않는 데이터  

예시 질문:
- Tableau Bootcamp 수료자인가요? → IN/OUT
- Tableau 사용 6개월 이상인가요? → IN/OUT
- 2025년 주문 고객인가요? → IN/OUT

Tableau에서 집합을 생성하면, **IN/OUT** 또는 **멤버만 표시** 두 가지 방식으로 나타낼 수 있습니다.
<img width="640" height="480" alt="image (21)" src="https://github.com/user-attachments/assets/4d29888e-46b8-47dd-adad-f20dc60fd650" />

---

## 2. 필터 vs 집합
- **필터(Filter):** 조건에 맞는 데이터만 분석  
- **집합(Set):** 조건에 맞는 데이터(IN)와 맞지 않는 데이터(OUT) 모두 분석에 활용  

예시:  
- 필터로 ‘가구’만 선택하면 → 가구 매출만 확인 가능  
- 집합으로 ‘가구=IN’, 나머지=OUT 설정하면 → 가구 vs 나머지의 매출 기여도 비교 가능  
<img width="1690" height="1232" alt="image (22)" src="https://github.com/user-attachments/assets/49e0f8ef-3413-46ec-adac-b83de7324edb" />

---

## 3. 집합 생성 방법
### 3.1 정적 집합
수동으로 선택한 멤버로 집합을 생성
![집합](https://github.com/user-attachments/assets/b9c76b74-df63-4c15-b09a-ef8b797b9e00)

1) **시각화에서 직접 선택**
   - 뷰에서 마크 선택 → 메뉴 → 집합 만들기
   - 선택한 멤버=IN, 나머지=OUT

2) **차원에서 생성**
   - 차원 → 마우스 오른쪽 → 만들기 → 집합 → 멤버 수동 선택  

### 3.2 동적 집합
조건이나 순위를 기반으로 멤버가 자동으로 변경되는 집합  
![동적집합3](https://github.com/user-attachments/assets/6248e953-3cf7-451a-8d7a-db8bfa6744b5)

#### 조건을 이용한 동적 집합
- **필드 기준:** 예) 합계 수익 < 0 인 고객 집합 생성
- **수식 기준:** 계산식을 이용하여 조건 지정

#### 상위를 이용한 동적 집합
- 예) 매출 Top 10 고객을 집합으로 생성  

---

## 📝 오늘의 핵심
- 집합은 **조건에 맞는 값(IN)** 과 **맞지 않는 값(OUT)** 을 모두 활용할 수 있는 강력한 기능  
- 정적/동적 집합을 적절히 활용하면, 필터보다 유연한 분석 가능  
- 동적 집합은 **조건/수식/순위** 기준으로 실무 활용도가 높음  

---

## 과제

### 과제1
![2021-09-17_13-27-31 (1)](https://github.com/user-attachments/assets/eab9ca78-d1aa-4d91-b1a5-5099cd3be50e)

### 과제2
<img width="2732" height="1536" alt="image (23)" src="https://github.com/user-attachments/assets/b927549a-01c9-4215-9352-d9937e55f4a4" />

