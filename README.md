# 통계·머신러닝 실습

대형마트 가상 데이터로 진행한 '파이썬 활용 데이터 통계' 과정(4일)의 실습 노트북과 데이터입니다.

- 모든 데이터는 실습을 위해 가상으로 만든 더미 데이터이며, 실제 실적이 아닙니다.
- 실습 노트북은 완성 코드입니다. 직접 실행해 결과를 확인하고, 코드를 읽고 결과를 해석하는 데 집중합니다.

## 시작하기

1. **실습 환경 준비**: [실습 환경 준비 안내서](setup/setup_guide.pdf)대로 [`stat_ml_setup.zip`](setup/stat_ml_setup.zip)을 내려받아 `C:\stat_ml` 폴더에 파이썬 3.14 가상환경을 만들고 점검 노트북까지 확인합니다.
2. **실습 자료 내려받기**: 이 페이지 위쪽의 **Code → Download ZIP**을 눌러 내려받고, 압축 안의 `code`, `data`, `docs` 폴더를 `C:\stat_ml`에 복사합니다.
3. **실습 시작**: `C:\stat_ml\start_jupyter.bat`을 더블클릭하고 `code` 폴더의 노트북을 01부터 순서대로 엽니다.

## 복습 퀴즈

- [말로 푸는 통계 복습 퀴즈](https://hellodatascience.github.io/retail-stat-ml/quiz/)에서 강의 내용을 객관식 30문항과 토의 과제 5개로 복습합니다.
- 보기를 고르면 그 방법을 실습 데이터로 실행한 결과와 풀이가 나옵니다. 설치나 로그인 없이 브라우저에서 바로 풀 수 있습니다.
- 본인의 Anthropic API 키를 넣으면 튜터와 대화하며 풀 수도 있습니다(선택, 대화 비용은 키 소유자 부담).

## 강의 구성

| 일차 | 주제 | 주요 내용 | 실습 |
|---|---|---|---|
| 1일차 | 통계 기초 | 대푯값과 퍼짐 정도, 분포의 모양, 로그 변환, 표준화, 표본과 모집단, 표준오차, 신뢰구간 | 01 점포 매출 기술통계 |
| 2일차 | 가설검정 | 귀무가설과 대립가설, 유의확률, 제1종·제2종 오류, 검정력, 표본 크기, t-검정, Mann-Whitney U 검정, 분산분석과 사후분석, 카이제곱 검정, 비율 검정, A/B 테스트 | 02 가설검정 기초<br>03 쿠폰 A/B 테스트 |
| 3일차 | 회귀분석 | 상관분석, 선형 회귀, 더미 변수, 회귀 진단, 영향점, 다중공선성, 로지스틱 회귀, 오즈비, 예측 확률 | 04 점포 월 매출 특성 분석<br>05 신규 고객 재구매 특성 분석 |
| 4일차 | 머신러닝 | 훈련셋·검증셋·시험셋, 과적합, 교차검증, 의사결정나무, 가지치기, 랜덤 포레스트, 그리드 서치, 분류 모델 성능 지표, 임계값, 특성 중요도, 부분 의존도 | 06 신규 고객 재구매 예측 |
| 부록 | — | 거래 내역으로 분석 테이블 만들기, 머신러닝 회귀 모델로 점포 매출 예측하기 | 부록A RFM 변수 만들기<br>부록B 점포 월 매출 예측 |

## 실습과 분석 질문

| 실습 | 분석 질문 | 분석 단위 | 사용 데이터 |
|---|---|---|---|
| [01 점포 매출 기술통계](code/01_descriptive_statistics_store_sales.ipynb) | 대형마트 점포의 월 매출 수준은 어느 정도이고, 점포 사이에 얼마나 차이가 나는가? | 점포 | `store_info.csv`<br>`store_month.csv` |
| [02 가설검정 기초](code/02_hypothesis_testing_basics.ipynb) | 점포 유형과 지역에 따라 월평균 매출이 다른가? 고객 특성에 따라 재구매율이 다른가? | 점포<br>신규 고객 | `store_month_clean.pkl`<br>`repurchase.csv` |
| [03 쿠폰 A/B 테스트](code/03_ab_test_discount_coupon.ipynb) | 장바구니 고객에게 5,000원 할인 쿠폰을 보내면 구매가 늘어나는가? 늘어난다면 회사에 이익인가? | 고객 | `ab_coupon.csv` |
| [04 점포 월 매출 특성 분석](code/04_linear_regression_store_sales.ipynb) | 점포의 월 매출은 어떤 특성(영업일수, 행사, 날씨, 면적, 지역 등)에 따라 달라지는가? | 점포 × 월 | `store_month_clean.pkl` |
| [05 신규 고객 재구매 특성 분석](code/05_logistic_regression_repurchase.ipynb) | 첫 구매 후 6차월 말까지의 구매 행동 중 어떤 특성이 7차월 구매와 관련 있는가? | 신규 고객 | `repurchase.csv` |
| [06 신규 고객 재구매 예측](code/06_machine_learning_repurchase.ipynb) | 6차월 말까지의 구매 행동으로 7차월에 구매할 고객을 얼마나 잘 골라낼 수 있는가? | 신규 고객 | `repurchase.csv`<br>`repurchase_valid.csv`<br>`repurchase_test.csv` |
| [부록A RFM 변수 만들기](code/appendix_a_rfm_features.ipynb) | 거래 내역으로 고객별 RFM 변수와 행동 변수를 만들어 재구매 분석 테이블을 직접 완성할 수 있는가? | 신규 고객 | `new_customers.csv`<br>`transactions.csv`<br>`repurchase.csv` |
| [부록B 점포 월 매출 예측](code/appendix_b_machine_learning_store_sales.ipynb) | 다음 달 점포 월 매출을 얼마나 정확하게 예측할 수 있는가? | 점포 × 월 | `store_month_clean.pkl`<br>`store_month_valid.csv`<br>`store_month_test.csv` |

- 실습마다 무엇을 확인하는지는 [실습 안내](docs/practice_guide.md)에 정리되어 있습니다.
- 질문에서 의사결정까지 인사이트를 얻는 흐름은 [데이터에서 인사이트를 얻는 5단계](docs/practice_guide.md#데이터에서-인사이트를-얻는-5단계)에 정리되어 있습니다.
- **01 실습을 먼저 실행해야** 02, 04, 부록B에서 사용하는 정제 파일(`store_month_clean.pkl`)이 만들어집니다.
- 같은 데이터를 여러 분석 방법으로 다시 사용합니다. 분석 방법이 바뀌면 질문과 해석이 어떻게 달라지는지 비교해 봅니다.

## 실습 데이터

| 주제 | 파일 | 내용 |
|---|---|---|
| 점포 | `store_info.csv` | 점포 100곳의 유형, 지역, 면적, 품목 수 등 |
| 점포 월 매출 | `store_month.csv`, `store_month_valid.csv`, `store_month_test.csv` | 점포별 월 매출과 영업일수·행사·날씨 등 (2024-01 ~ 2026-10) |
| 쿠폰 A/B 테스트 | `ab_coupon.csv` | 장바구니 고객의 쿠폰 발송 여부와 14일 구매 결과 |
| 신규 고객 재구매 | `new_customers.csv`, `transactions.csv` | 신규 고객 정보와 거래 내역 |
| 신규 고객 재구매 | `repurchase.csv`, `repurchase_valid.csv`, `repurchase_test.csv` | 첫 6개월 구매 행동과 7차월 재구매 여부 |

변수의 뜻과 행 수, 테이블 연결은 [데이터 정의서](docs/data_dictionary.md)를 확인합니다.

## 폴더 구조

```
retail-stat-ml/
├── code/      # 실습 노트북
├── data/      # 실습 데이터 (CSV)
├── docs/      # 실습 안내, 데이터 정의서
├── quiz/      # 복습 퀴즈 페이지
└── setup/     # 실습 환경 준비 안내서와 설치 파일
```
