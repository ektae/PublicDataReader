# PublicDataReader

![PNG](./img_logo.png)


## Open Source Project

- **기획/개발/관리: 정우일(Wooil Jeong)**
- **e-mail: wooil@kakao.com**


## 소개

- 최신 버전: ![](https://img.shields.io/badge/PublicDataReader-0.1.1-blue.svg)  
- 요구 사항: ![](https://img.shields.io/badge/Python-3.7.4-yellow.svg) ![](https://img.shields.io/badge/Pandas-0.25.3-red.svg)

PublicDataReader는 [공공데이터포털](https://data.go.kr)에서 제공하는 OpenAPI 서비스를 Python으로 쉽게 이용할 수 있도록 도와주는 데이터 수집 라이브러리입니다. 2020년 04월 현재 [국토교통부 실거래가 정보](https://www.data.go.kr/dataset/3050988/openapi.do) 조회 서비스 중 '아파트매매 실거래자료', '아파트매매 실거래 상세 자료', '아파트 전월세 자료' 그리고 '아파트 분양권판매 신고 자료' 조회 서비스에 대한 인터페이스를 제공하고 있습니다. 추후 수요가 높은 Open API 서비스에 대한 인터페이스도 지속적으로 업데이트할 예정입니다.


## 설치 방법

```bash
pip install PublicDataReader
```

## 사용 방법
### (예시) 국토교통부 실거래가 정보 조회 서비스

```python
# 라이브러리 임포트 및 버전 확인
import PublicDataReader as pdr
print(pdr.__version__)

# 공공 데이터 포털 Open API 서비스 인증키
serviceKey = "<<YOUR API SERVICE KEY>>"

# 아파트매매 실거래자료 조회 인스턴스 생성
AptTrade = pdr.AptTradeReader(serviceKey)
# 아파트매매 실거래 상세 자료 조회 인스턴스 생성
AptTradeDetail = pdr.AptTradeDetailReader(serviceKey)
# 아파트 전월세 자료 조회 인스턴스 생성
AptRent = pdr.AptRentReader(serviceKey)
# 아파트 분양권전매 신고 자료 조회 인스턴스 생성
AptOwnership = pdr.AptOwnershipReader(serviceKey)


# 특정 월 데이터 조회
df_code = AptTrade.CodeFinder("백현동")                            # 지역코드 : 41135
df_code.head()

# 데이터 프레임 만들기
# Function("지역코드 5자리", "계약월(YYYYMM)")
# 2020년 04월 백현동에 해당하는 자료를 Pandas DataFrame 으로 생성
df_AptTrade = AptTrade.DataReader("41135", "202003")             # 아파트매매 실거래자료 조회
df_AptTradeDetail = AptTradeDetail.DataReader("41135", "202003") # 아파트매매 실거래 상세 자료 조회

df_AptRent = AptRent.DataReader("41135", "202003")               # 아파트 전월세 자료 조회
df_AptOwnership = AptOwnership.DataReader("41135", "202003")     # 아파트 분양권전매 신고 자료 조회

df_OffiTrade = OffiTrade.DataReader("41135", "202003")           # 오피스텔 매매 신고 조회
df_OffiRent = OffiRent.DataReader("41135", "202003")             # 오피스텔 전월세 신고 조회

df_RHTrade = RHTrade.DataReader("41135", "202003")               # 연립다세대 매매 실거래자료 조회
df_RHRent = RHRent.DataReader("41135", "202003")                 # 연립다세대 전월세 실거래자료 조회

df_DHTrade = DHTrade.DataReader("41135", "202003")               # 단독/다가구 매매 실거래 조회
df_DHRent = DHRent.DataReader("41135", "202003")                 # 단독/다가구 전월세 자료 조회

df_LandTrade = LandTrade.DataReader("41135", "202003")           # 토지 매매 신고 조회
df_BizTrade = BizTrade.DataReader("41135", "202003")             # 상업업무용 부동산 매매 신고 자료 조회


# 기간 설정하여 데이터 프레임 만들기
# Function("지역코드 5자리", "시작 년월(YYYY-MM)", "종료 년월(YYYY-MM)")
df_AptTradeSum = AptTrade.DataCollector("41135", "2020-01", "2020-03")

```