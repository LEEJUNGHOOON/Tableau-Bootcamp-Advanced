# 📆 Tableau Bootcamp 고급편 — Day 1 & Day 2 퀘스트 안내

> **주제:** 날짜 데이터 다루기 — 데이터 유형 변환 · 날짜 생성 · 연속/불연속 · 날짜 함수 × 매개변수 · MTD/YTD · 회계연도 속성

---

## 🎯 학습 목표
- 서로 다른 **날짜 포맷**을 정확히 변환하고 정규화한다.
- 분리된 연/월/일 필드로 **날짜 생성**을 수행한다.
- 날짜의 **연속형(초록) vs 불연속형(파랑)** 차이를 이해한다.
- `DATEPARSE / DATEPART / DATETRUNC / DATEADD` 등을 실무 시나리오에 적용한다.
- **매개변수(기준일)**와 결합하여 **MoM/MTD/YTD** 비교를 구현한다.
- 조직 표준(예: **회계연도 시작월**)에 맞게 **날짜 속성**을 설정한다.

---

## 1) 데이터 유형 변경하기

### A. Desktop 기본 기능으로 변경
- Tableau가 자동 유형을 부여하지만, 잘못 인식되는 경우가 있음.
- `yyyyMM`, `yyyy-MM` 등은 **데이터 유형 → 날짜**로 바로 변환 가능.

### B. `DATEPARSE` 함수로 문자열 → 날짜
```tableau
// 문법: DATEPARSE(<형식>, <문자열>)
DATEPARSE('M/d/yyyy', [Date4_String])   // 예: 9/1/2020 → 2020-09-01
DATEPARSE('MMM.yyyy', [Date5_String])   // 예: Sep.2020 → 2020-09-01
```
<img width="1050" height="610" alt="image (3)" src="https://github.com/user-attachments/assets/09b63c0b-203a-4a33-9ae5-0ede1580cbd4" />

> **TIP:** 대문자 `M`=월, 소문자 `m`=분. 형식 토큰이 포맷과 정확히 일치해야 함.

---

## 2) 필드를 조합해 날짜 생성하기 (연/월/일 분리)

1) 연/월/일을 문자열로 변환 후 연결 → `"YYYY-MM-DD"` 포맷 만들기  
2) `DATEPARSE`로 날짜 변환
```tableau
// 예: [Y](yyyy), [M](M), [D](d)
DATEPARSE('yyyy-MM-dd', STR([Y]) + '-' + STR([M]) + '-' + STR([D]))
```

---

## 3) 날짜의 연속형 vs 불연속형

<img width="1478" height="1346" alt="image (4)" src="https://github.com/user-attachments/assets/0c7b370b-5e77-444e-b6cd-5be10a525517" />

- **연속형(초록)**: 축(continuous scale) 기반 — 라인 차트, 트렌드에 적합  
- **불연속형(파랑)**: 카테고리 축 — 막대/테이블에 적합  
- 동일 날짜라도 연속/불연속 선택에 따라 **표현과 집계 방식**이 달라짐.

> 날짜 알약 더블클릭 → 내부적으로 사용하는 함수(예: `DATETRUNC`, `DATEPART`) 확인 가능

<img width="1400" height="674" alt="image (5)" src="https://github.com/user-attachments/assets/34984c3b-5975-4249-8a5c-05f4a5bb7ffd" />

---

## 4) 날짜 함수 핵심 레퍼런스

- **`DATEPARSE(format, string)`**: 문자열 → 날짜  
- **`DATEPART(part, date[, start_of_week])`**: 지정 파트의 정수 반환 (연/분기/월/주/일)  
- **`DATETRUNC(part, date[, start_of_week])`**: 해당 파트의 **시작 시점으로 절단**  
- **`DATEADD(part, n, date)`**: 날짜에 n 단위 더하기/빼기

```tableau
// 예시
DATEPART('year', [주문 날짜])          // 2021
DATETRUNC('month', [주문 날짜])        // 월의 1일 00:00
DATEADD('month', -1, [주문 날짜])      // 한 달 전
```

---

## 5) 매개변수 × 날짜 함수 (MoM/MTD/YTD)

### A. 기준 월 매출 (매개변수: `[p_기준날짜]`)
```tableau
// 기준월_매출
IF YEAR([주문 날짜]) = YEAR([p_기준날짜])
AND MONTH([주문 날짜]) = MONTH([p_기준날짜])
THEN [매출]
END
```

### B. 전월 매출
```tableau
// 전월_매출 — 단순화 버전
IF YEAR([주문 날짜]) = YEAR([p_기준날짜])
AND MONTH([주문 날짜]) = MONTH([p_기준날짜]) - 1
THEN [매출]
END

// 경계(1월→12월) 처리 권장: DATEADD로 안전하게
IF DATETRUNC('month',[주문 날짜])
 = DATETRUNC('month', DATEADD('month', -1, [p_기준날짜]))
THEN [매출]
END
```

### C. MTD (Month-To-Date)
```tableau
// MTD
IF DATETRUNC('month', [p_기준날짜]) <= [주문 날짜]
AND [주문 날짜] <= [p_기준날짜]
THEN [매출]
END
```

### D. YTD (Year-To-Date)
```tableau
// YTD
IF DATETRUNC('year', [p_기준날짜]) <= [주문 날짜]
AND [주문 날짜] <= [p_기준날짜]
THEN [매출]
END
```

---

## 6) 회계연도/주 시작 요일 등 날짜 속성 설정

- 기본 회계연도 시작: **1월** → 조직 기준(예: **2월 시작**)으로 변경 가능  
- **데이터 패널**에서 날짜 필드 우클릭 → **날짜 속성**  
  - 회계연도 시작월, 주 시작 요일 등 설정  
- 새 워크시트에서 적용 확인(기존 시트는 속성 갱신 필요)

> 공식 문서: https://help.tableau.com/current/pro/desktop/ko-kr/date_properties.htm

---

## 👾 Day 1 & 2 퀘스트(과제) 체크리스트

###1
<img width="904" height="314" alt="image (6)" src="https://github.com/user-attachments/assets/ab28869c-fcc1-4aff-bca5-182644b30e3c" />
###2
<img width="998" height="724" alt="image (7)" src="https://github.com/user-attachments/assets/aebfe47d-7258-4081-9b41-166a113cf18c" />
###3
<img width="1248" height="634" alt="image (8)" src="https://github.com/user-attachments/assets/37c8831c-2d47-4e0b-8c93-421a6e9beff2" />


- [ ] 서로 다른 포맷의 날짜를 **DATEPARSE**로 정확히 변환했다.  
- [ ] 분리된 연/월/일 필드로 날짜를 **생성**했다.  
- [ ] 연속형/불연속형 차이를 **시각화로 비교**했다.  
- [ ] `[p_기준날짜]` 매개변수로 **기준월/전월/MTD/YTD** 계산을 구현했다.  
- [ ] **DATEADD**를 이용해 경계(연말/연초) 이슈를 안전하게 처리했다.  
- [ ] 조직 표준에 맞게 **회계연도 시작월**을 설정했다.

---

