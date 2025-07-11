
# 🪄 Tableau Bootcamp 고급편 1일차 & 2일차 학습 및 퀘스트 안내

## 👋 환영 인사

Tableau Champion을 향한 여정에 함께할 **Challenger 여러분을 환영합니다!**

**Day 1 & Day 2**에서는 **날짜 함수**를 활용한 "시간의 데이터 스킬"을 학습합니다.  
학습 후 퀘스트(과제)를 **다음주 월요일까지 제출**해주세요!

---

## 📁 실습 파일 다운로드

- [DAY1 DAY2 학습 실습_데이터 유형 변경하기](https://korea-tableau-users.slack.com/files/U08D6PP1P0Q/F093A2N7LCC)
- [DAY1 DAY2 학습 실습_필드를 조합하여 날짜 생성하기](https://korea-tableau-users.slack.com/files/U08D6PP1P0Q/F0933CETB5L)
- [DAY1 DAY2 학습 실습_날짜 함수가 매개변수를 만났을 때_1](https://korea-tableau-users.slack.com/files/U08D6PP1P0Q/F0933TDN07Q)
- [DAY1 DAY2 학습 실습_날짜 함수가 매개변수를 만났을 때_2](https://korea-tableau-users.slack.com/files/U08D6PP1P0Q/F0939SK7R34)

---

## 1️⃣ 데이터 유형 변경하기

### A. 기본 기능 사용

- Date1(yyyyMM), Date2(yyyy-MM): **데이터 유형 → 날짜로 변경**
- Date4: 월/일이 뒤바뀐 경우 → **DATEPARSE 함수 활용 필요**

### B. DATEPARSE 함수 활용

```plaintext
DATEPARSE("M/d/yyyy", STR([Date4]))
DATEPARSE("MMM.yyyy", STR([Date5]))
```

---

## 2️⃣ 필드를 조합하여 날짜 생성하기

### 예: 연도/월/일 분리된 데이터를 날짜로 만들기

1. 연도, 월, 일 → 문자열로 변환
2. 아래와 같은 문자열 결합

```plaintext
STR([연도]) + "-" + STR([월]) + "-" + STR([일])
```

3. DATEPARSE 함수 적용

```plaintext
DATEPARSE("yyyy-M-d", [문자열_결합필드])
```

---

## 3️⃣ 연속형 vs 불연속형 날짜 필드

- 🟢 연속형: Date1  
- 🔵 불연속형: Date2~5

**복습 영상 추천**: [연속형/불연속형 복습하기](https://help.tableau.com/)

---

## 4️⃣ 날짜 함수가 매개변수를 만났을 때

### A. DATEPART 함수 활용

#### 기준월_매출

```plaintext
IF DATEPART('year', [주문 날짜]) = DATEPART('year', [p_기준날짜])
   AND DATEPART('month', [주문 날짜]) = DATEPART('month', [p_기준날짜])
THEN [매출]
END
```

#### 전월_매출

```plaintext
IF DATEPART('year', [주문 날짜]) = DATEPART('year', [p_기준날짜])
   AND DATEPART('month', [주문 날짜]) = DATEPART('month', [p_기준날짜]) - 1
THEN [매출]
END
```

### 간단한 함수로 대체 가능:

```plaintext
IF YEAR([주문 날짜]) = YEAR([p_기준날짜]) AND MONTH([주문 날짜]) = MONTH([p_기준날짜])
THEN [매출]
END
```

---

### B. DATETRUNC 함수 활용

#### MTD (Month-to-Date)

```plaintext
IF DATETRUNC('month', [p_기준날짜]) <= [주문 날짜]
   AND [주문 날짜] <= [p_기준날짜]
THEN [매출]
END
```

#### YTD (Year-to-Date)

```plaintext
IF DATETRUNC('year', [p_기준날짜]) <= [주문 날짜]
   AND [주문 날짜] <= [p_기준날짜]
THEN [매출]
END
```

---

## 5️⃣ 날짜 속성 변경하기

- 기본 회계연도: 1월 → Salesforce 기준: **2월 시작으로 변경 필요**
- 방법:
  1. 데이터 탭 > 필드 우클릭 > 날짜 속성
  2. 회계연도 시작 달 선택 (예: 2월)
  3. 새 워크시트에서 시각화 적용 확인

🔗 도움말 링크: [Tableau 날짜 속성](https://help.tableau.com/current/pro/desktop/ko-kr/date_properties.htm)

---

## ✅ 수고하셨습니다!

이제 배운 내용을 바탕으로 **퀘스트(과제)**를 수행해보세요! 👾  
모든 날짜 처리 능력은 실제 업무에서도 강력한 무기가 될 수 있습니다.
