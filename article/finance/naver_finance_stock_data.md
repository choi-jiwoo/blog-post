# 네이버 금융 과거 주가 데이터 크롤링

## 데이터 소스
파이썬으로 주가 데이터를 사용하려 할 때 [yfinance](https://pypi.org/project/yfinance/)나 [financeDataReader](https://github.com/financedata/financedatareader)와 같은 좋은 패키지를 사용하면 간단하게 주가 데이터를 불러올 수 있다. 하지만 이번 포스팅에선 간단하게 [네이버 금융](https://finance.naver.com/)에서 직접 주가 데이터를 불러와 보고자 한다. 재밌으니깐 ㅎㅎ

## Let's Have A Look
네이버 금융에 접속한 후 검색창에서 삼성전자를 검색하고 결과 페이지에서 '차트' 탭을 누르면 [삼성전자의 주가 차트](https://finance.naver.com/item/fchart.naver?code=005930)를 볼 수 있다. ~~*제발 가즈아!*~~

![005930](https://velog.velcdn.com/images/choi-jiwoo/post/b1ab9fa4-d9b8-4d1c-a7bc-7575d0c07473/image.png)

여기서 크롬으로 **개발자 도구**를 열고 **네트워크**에서 `Fetch/XHR`을 누르면 브라우저가 서버와 실시간으로 주고 받은 데이터들을 확인할 수 있다. 주가 차트가 다 불러와진 상태에서는 불러온 데이터를 볼 수 없으므로 **개발자 도구**를 열어둔 상태에서 새로고침을 해보면 뭔가 줄줄이 생기는걸 볼 수 있다.

![network](https://velog.velcdn.com/images/choi-jiwoo/post/ec3e01d6-7946-44df-867f-fbd3a67356a4/image.png)

이것들 중 주가 데이터를 담고 있는것은 siseJson.naver?로 시작하는것으로 Headers에 요청보낸 URL을 확인하고 이 URL로 GET request를 보내면 데이터를 불러올 수 있다.

## Let's Code!

```python
import ast
import pandas as pd
import requests

url = 'https://api.finance.naver.com/siseJson.naver'
params = {
    'symbol': '005930',  # 삼성전자
    'requestType': 1,  # 무엇인지 모르겠음
    'startTime': '20220101',
    'endTime': '20220122',
    'timeframe': 'day',  # 'week', 'month'
}
```

날짜 범위는 YYYYMMDD 형식으로 해주고 파라미터 중 하나라도 빠지면 데이터가 불러와지지 않으니 다 채워넣어 주자.

이제 GET request를 보내고 응답코드가 `200`이 반환되면 성공적으로 데이터를 불러온 것이다.

```python
res = requests.get(url, params=params)
print(res)  # or print(res.status_code)
# Output: <Response [200]>
```

```python
print(res.text)
```

![res.text](https://velog.velcdn.com/images/choi-jiwoo/post/7433f241-3560-47c2-aff6-62b0e73a47e3/image.png)

응답 받은 데이터를 쓸만한 데이터로 만들으려면 추가적으로 전처리 과정이 필요하다.

## Cleaning Time
먼저 응답 받은 문자열 데이터에서 구두점들을 제거해주고 데이터프레임으로 변환해 주었다.

```python
data = res.text.strip()
stock = ast.literal_eval(data)  # 문자열을 배열로 변환
stock = pd.DataFrame(stock, columns=stock[0])
stock.drop(0, inplace=True)
```
그리고 각 Column 마다 알맞은 데이터 타입으로 변환해 주었다.
```python
stock['날짜'] = pd.to_datetime(stock['날짜'])
ohlcv = ['시가', '고가', '저가', '종가', '거래량']
stock[ohlcv] = stock[ohlcv].apply(pd.to_numeric)
stock['외국인소진율'] = stock['외국인소진율'].astype('float64')
```

## Result

![df](https://velog.velcdn.com/images/choi-jiwoo/post/bc79ecfd-5863-4f17-a391-334d81000eb4/image.jpeg)

## Closing Words

이렇게 불러온 주가 데이터는 실시간 데이터가 아니기 때문에 트레이딩에 사용하기에는 적합하지 않고 시계열 분석이나 백테스팅 목적으로 사용할 수 있을것이다. 그리고 비공식적인 방법으로 데이터를 불러오는 것이기 때문에 개인적인 목적 외에 사용하는 것은 권장하지 않는다.
