# 데이터 정의서

- 모든 데이터는 실습을 위해 가상으로 만든 더미 데이터이며, 실제 실적이 아닙니다.
- 파일 형식은 CSV(01 실습에서 만드는 `store_month_clean.pkl` 제외), 인코딩은 `utf-8-sig`입니다. Excel에서 열어도 한글이 깨지지 않습니다.
- 금액 단위는 변수마다 다르므로 각 표의 **형식** 열을 확인합니다.

## 파일 목록

| 파일 | 행 수 | 한 행의 단위 | 사용하는 실습 |
|---|---|---|---|
| `store_info.csv` | 100 | 점포 | 01 |
| `store_month.csv` | 2,400 | 점포 × 월 (2024-01 ~ 2025-12) | 01 |
| `store_month_clean.pkl` | 2,400 | 점포 × 월 (위 두 파일을 결합·정제, 01 실습에서 생성, 부록B 훈련셋) | 02, 04, 부록B |
| `store_month_valid.csv` | 600 | 점포 × 월 (2026-01 ~ 2026-06, 검증셋) | 부록B |
| `store_month_test.csv` | 400 | 점포 × 월 (2026-07 ~ 2026-10, 시험셋) | 부록B |
| `ab_coupon.csv` | 10,000 | 고객 | 03 |
| `new_customers.csv` | 10,000 | 신규 고객 | 부록A |
| `transactions.csv` | 53,448 | 거래 1건 | 부록A |
| `repurchase.csv` | 9,173 | 신규 고객 (06 훈련셋) | 02, 05, 06, 부록A |
| `repurchase_valid.csv` | 4,604 | 신규 고객 (검증셋) | 06 |
| `repurchase_test.csv` | 1,562 | 신규 고객 (시험셋) | 06 |

[참고] 01 실습에서 `store_info.csv`와 `store_month.csv`를 결합·정제해 `store_month_clean.pkl`로 저장합니다. 01 실습을 먼저 실행해야 이 파일이 만들어집니다.

[참고] `repurchase.csv`는 02 실습에서 교차분석에, 05 실습에서 로지스틱 회귀 모형의 분석 데이터로 사용하고, 06 실습에서 분류 모델의 훈련셋으로 사용합니다.

**테이블 연결**

- `store_info.csv` ─ `store_id` ─ `store_month.csv`
- `new_customers.csv` ─ `customer_id` ─ `transactions.csv`, `repurchase.csv`
- `repurchase_valid.csv`, `repurchase_test.csv`는 훈련셋과 다른 고객이라 다른 파일과 연결되지 않습니다.

---

## 1. store_info.csv — 점포 마스터

| 변수 | 형식 | 설명 |
|---|---|---|
| `store_id` | 문자 | 점포 ID (S001 ~ S100) |
| `store_type` | 범주 | 점포 유형. 매장 면적 기준으로 소형 30(하위 30%) / 중형 40 / 대형 30(상위 30%) |
| `region` | 범주 | 지역. 수도권 40 / 영남권 25 / 충청·강원권 20 / 호남권 15 |
| `floor_area` | 정수(㎡) | 매장 면적 |
| `sku_count` | 정수 | 취급 품목 수 |
| `parking` | 정수 | 주차면수 |
| `closure_day` | 범주 | 의무휴업 요일. 일요일 74 / 평일 26 (둘째·넷째 주) |

## 2. store_month.csv, store_month_valid.csv, store_month_test.csv — 점포 월 매출

`store_month_valid.csv`(2026-01 ~ 2026-06)와 `store_month_test.csv`(2026-07 ~ 2026-10)는 예측 모델의 검증셋·시험셋으로 따로 제공합니다.<br>
두 파일은 훈련셋(`store_month_clean.pkl`)과 열 구성이 같도록 아래 열 뒤에 점포 마스터의 열(`store_type` ~ `closure_day`)이 결합되어 있습니다.

| 변수 | 형식 | 설명 |
|---|---|---|
| `store_id` | 문자 | 점포 ID |
| `ym` | 문자 | 연월 (예: 2025-03) |
| `open_days` | 정수 | 영업일수. 그 달의 일수에서 의무휴업일(2일), 설·추석 당일, 공사 등으로 휴업한 날을 뺀 일수 |
| `weekend_days` | 정수 | 영업한 토·일요일 수 |
| `holiday_days` | 정수 | 영업한 평일 공휴일 수 (대체·임시공휴일 포함). 휴업일이 아니므로 `open_days`에서 빼지 않음 |
| `holiday_season` | 0/1 | 설·추석이 포함된 달이면 1 |
| `event_days` | 정수 | 행사일수 |
| `rain_days` | 정수 | 강수일수 |
| `extreme_days` | 정수 | 폭염·한파일수 |
| `renovation` | 0/1 | 리뉴얼 공사로 휴업한 기간이 있는 달이면 1 |
| `sales` | 실수(백만 원) | 월 매출 |
| `transactions` | 정수 | 구매 건수 |
| `avg_ticket` | 정수(원) | 객단가 = `sales` × 1,000,000 ÷ `transactions` |

## 3. ab_coupon.csv — 쿠폰 A/B 테스트

- 대상: 온라인몰에서 장바구니에 상품을 담은 고객
- 쿠폰: B집단에만 "3만 원 이상 구매 시 5,000원 할인" 쿠폰을 발송
- 관찰 기간: 배정일부터 14일
- 파일럿과 본 테스트는 서로 다른 고객입니다.

| 변수 | 형식 | 설명 |
|---|---|---|
| `customer_id` | 문자 | 고객 ID |
| `phase` | 범주 | 파일럿(집단별 1,000명, 2025-08-25 ~ 08-28 배정) / 본테스트(집단별 4,000명, 2025-09-15 ~ 09-30 배정) |
| `group` | 범주 | A(대조군, 쿠폰 미발송) / B(처리군, 쿠폰 발송) |
| `assign_date` | 날짜 | 배정일(쿠폰 발송일) |
| `tenure` | 범주 | 신규 / 기존 고객 |
| `device` | 범주 | 앱 / 웹 |
| `cart_value` | 정수(원) | 장바구니 금액 |
| `purchased_14d` | 0/1 | 14일 안에 구매했으면 1 |
| `revenue_14d` | 정수(원) | 14일 안의 매출(할인 전 금액). 구매하지 않은 고객은 0 |
| `coupon_used` | 0/1 | 쿠폰을 사용했으면 1 (B집단만 1이 될 수 있음) |
| `discount_amount` | 정수(원) | 쿠폰 할인 금액 (사용 시 5,000) |

## 4. 신규 고객 재구매 — 5개 파일

- 첫 구매월을 M1이라고 할 때, **기준일**(`cutoff_date`)은 M6의 말일이고 M7은 기준일 다음 달입니다.
- 데이터 추출일은 2026-10-31입니다.

| 구분 | 첫 구매월(M1) | 재구매 확인 월(M7) | 파일 |
|---|---|---|---|
| 훈련셋 | 2025-07 ~ 2025-12 | 2026-01 ~ 2026-06 | `new_customers.csv`(10,000명), `transactions.csv`, `repurchase.csv` |
| 검증셋 | 2026-01 ~ 2026-03 | 2026-07 ~ 2026-09 | `repurchase_valid.csv` |
| 시험셋 | 2026-04 | 2026-10 | `repurchase_test.csv` |

### 4-1. new_customers.csv — 신규 고객 마스터

| 변수 | 형식 | 설명 |
|---|---|---|
| `customer_id` | 문자 | 고객 ID |
| `cohort_ym` | 문자 | 첫 구매월(M1) |
| `first_purchase_date` | 날짜 | 첫 구매일 |
| `cutoff_date` | 날짜 | 기준일(M6 말일) |
| `age_group` | 범주 | 20대 / 30대 / 40대 / 50대 / 60대 이상 |
| `app_member` | 0/1 | 앱 회원이면 1 |
| `distance_km` | 실수(km) | 주 이용 점포까지 거리 |
| `withdraw_date` | 날짜 | 탈퇴일. 탈퇴하지 않았으면 빈 값 |
| `last_purchase_date` | 날짜 | 데이터 추출일 기준 마지막 구매일 |

### 4-2. transactions.csv — 거래 내역

| 변수 | 형식 | 설명 |
|---|---|---|
| `txn_id` | 문자 | 거래 ID |
| `customer_id` | 문자 | 고객 ID |
| `txn_date` | 날짜 | 구매일 |
| `channel` | 범주 | 마트 / 슈퍼 / 온라인 |
| `amount` | 정수(원) | 구매 금액 |
| `fresh_amount` | 정수(원) | 구매 금액 중 신선식품 금액 |
| `discount_amount` | 정수(원) | 구매 금액 중 할인·쿠폰으로 결제한 금액 |

### 4-3. repurchase.csv — 재구매 분석 테이블

기준일 전에 탈퇴한 고객을 제외하고, 기준일까지의 거래로 고객별 입력변수를 계산한 테이블입니다.<br>
`repurchase_valid.csv`, `repurchase_test.csv`도 열 구성이 같고, 첫 구매 시기만 다릅니다.

| 변수 | 형식 | 설명 |
|---|---|---|
| `customer_id`, `cohort_ym`, `cutoff_date`, `age_group`, `app_member`, `distance_km` | | 고객 마스터와 같음 |
| `frequency` | 정수 | 기준일까지 구매 횟수 |
| `monetary` | 정수(원) | 기준일까지 구매 금액 합계 |
| `active_months` | 정수 | 기준일까지 구매가 있었던 월 수 (1 ~ 6) |
| `recency_days` | 정수 | 기준일 − 마지막 구매일 (일) |
| `fresh_ratio` | 실수(소수점 3자리) | 신선식품 금액 합계 ÷ 구매 금액 합계 |
| `discount_ratio` | 실수(소수점 3자리) | 할인·쿠폰 결제 금액 합계 ÷ 구매 금액 합계 |
| `main_channel` | 범주 | 구매 횟수가 가장 많은 채널 (동률이면 먼저 이용한 채널) |
| `multi_channel` | 0/1 | 2개 이상 채널을 이용했으면 1 |
| `repurchase_m7` | 0/1 | **목표변수**. M7에 1회 이상 구매했으면 1 |
