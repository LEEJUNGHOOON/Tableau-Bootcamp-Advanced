# 📊 Tableau Bootcamp [DAY 3–5]
## 차원의 지배자 1편: 세부 수준 계산식 (LOD)

---


## 🔑 핵심 요약
**LOD는 “시각화 수준(View LOD)”과 무관하게, 원하는 차원 수준으로 집계를 고정하는 기능**입니다.  
👉 계산 수준 ≠ 시각화 수준일 때, 정확한 값을 보장합니다.

---

<img width="1579" height="984" alt="2021-08-30_15-33-54" src="https://github.com/user-attachments/assets/f3655ac7-57e4-4953-b2bf-d722295110fb" />


## 📌 1. 왜 LOD가 필요한가
- Tableau는 기본적으로 **시트에 올린 차원**에 따라 집계 수행
- 분석에서 **계산에 필요한 수준 ≠ 시각화 수준**일 경우 결과가 틀어짐

### 📍 예시: 설치 → 첫 이벤트까지 걸린 시간
- 고객별 **최소 설치시간**, **최소 이벤트시간** 차이를 구해야 함
- 뷰에서 [Customer ID]를 제외하면 계산 수준이 전체로 바뀌어 잘못된 결과 발생
- ✅ **해결:** `FIXED [Customer ID]`로 고객 단위에 고정 후, 분포 시각화

---

## 📌 2. LOD 작성법 (Syntax)

```tableau
{ INCLUDE | EXCLUDE | FIXED [차원] , ... : 집계 함수 ( [측정값] ) }
```

### 🖋 실전 예제
```tableau
// 고객별 최초 설치일
{ FIXED [Customer ID] : MIN([Install Time]) }

// 고객별 최초 이벤트일
{ FIXED [Customer ID] : MIN([Event Time]) }


// 소요 기간(주 단위)
DATEDIFF('week', [고객별 최초 설치일], [고객별 최초 이벤트일])
```
// 고객별 최초 이벤트일
<img width="1042" height="594" alt="2021-08-31_13-58-27" src="https://github.com/user-attachments/assets/c8575ff0-1152-4762-86cc-24275b5e69f0" />

// 소요 기간(주 단위)
<img width="1242" height="743" alt="2021-09-01_10-06-43" src="https://github.com/user-attachments/assets/542389a8-4502-4a2b-b326-35106ee07818" />

---

## 📌 3. FIXED / INCLUDE / EXCLUDE 차이

| 옵션      | 설명 | 특징 |
|-----------|------|------|
| **FIXED** | 뷰 수준과 관계없이 지정 차원에 고정 | 가장 예측 가능 |
| **EXCLUDE** | 뷰 차원 중 일부를 제외 | 특정 차원 제외 후 집계 |
| **INCLUDE** | 뷰 차원에 추가 차원을 포함 | 더 세분화된 집계 |

---

## 📌 4. Tableau 작업 순서 이해 (Order of Operations)

<img width="800" height="535" alt="image (2)" src="https://github.com/user-attachments/assets/3698426c-3faa-4e8d-9c48-970f34fdb775" />

- **문제 상황:** Top N과 차원 필터 동시 적용 시 원하는 결과가 나오지 않음  
- **원인:** Top N이 차원 필터보다 우선 → 전체 Top 10 → 이후 차원 필터 적용  
- **해결:** 필터를 **Context Filter**로 변경해 순서 조정

```text
전체 Top 10 → 가구 필터 적용 ❌
가구 Context Filter → 그 결과에서 Top 10 ✅
```
<img width="860" height="456" alt="2021-09-03_00-34-14" src="https://github.com/user-attachments/assets/4c423129-43c4-4900-9f1a-c9813a8ba0d3" />

---

## 📌 5. 베스트 프랙티스
- 필요한 경우에만 LOD 필드 생성 → 성능 최적화
- 필드에 주석을 남겨 **계산 목적/수준** 명확히 기록
- 평균 계산 시 **뷰 행수 vs LOD 기준 행수 차이**에 주의
- INCLUDE/EXCLUDE는 뷰 의존성이 높음 → 실무에선 FIXED 선호

---

## 📌 6. 실습 과제 포인트
- `FIXED [Customer ID]`로 최초 설치/이벤트 고정 → 소요 기간 계산  
- 고객별 소요 기간을 **분포(히스토그램)**로 시각화  
- Top N + 차원 필터 충돌 → Context Filter로 해결

---

