# 미국 상장 기업 공시 데이터

## SEC EDGAR API

**SEC**는 **Securities and Exchange Commission**의 약자로 미국 증권거래위원회다. [SEC EDGAR](https://www.sec.gov/edgar/search-and-access) 는 미국 증시에 상장되어있는 회사들이 자신들의 공시자료를 올리는 시스템이다. 한국에 전자공시시스템 [DART](http://dart.fss.or.kr/) 같은곳이다.

이곳에서 EDGAR 시스템에 있는 데이터를 사용할 수 있게 [공식 API](https://www.sec.gov/edgar/sec-api-documentation)를 공개하기 시작했는데 따로 API key를 발급 받지 않아도 사용이 가능하다.

유로 API가 있긴 있다.
개발자 등록을 하고 API key를 받고 사용할 수 있고, Companies, Core Financials, Insider Trades, Institutional Ownership, Desciptions 데이터셋을 제공하고 있다. 자세한 사항은 [이곳](https://developer.edgar-online.com/docs)에서 확인할 수 있다.

## Let's Have A Look!

사이트에 설명을 보면 회사들이 제출한 공시 자료들과 여러 재무 정보가 담겨있는 XBRL 형식의 문서에서 추출한 데이터들을 REST API 형태로 제공한다.

> Submissions by company and extracted XBRL data are available via RESTful APIs on data.sec.gov, offering JSON formatted data.
>
> Currently included in the APIs are the submissions history by filer and the XBRL data from financial statements (forms 10-Q, 10-K,8-K, 20-F, 40-F, 6-K, and their variants).

**XBRL** 이란 기업 재무 정보를 표준화된 방식으로 보고할 수 있도록 만들어진 XML 형식의 기업보고용 언어이다. SEC 뿐만 아니라 다른 국가에서도 기업이 재무 정보를 보고할 때 XBRL을 사용하여 보고한다.

> Extensible Business Markup Language (XBRL) is an XML-based format for reporting financial statements used by the SEC and financial regulatory agencies across the world.

## Data Offered

SEC에서는 현재 4가지 정도의 데이터를 제공 하고 있다.

|데이터|링크|
|----|---|
|기업이 제출한 보고서 목록|https://data.sec.gov/submissions/CIK##########.json|
|기업의 특정 재무 항목에 대한 과거 데이터 <br> (예: 매입채무)|https://data.sec.gov/api/xbrl/companyconcept/CIK##########/us-gaap/AccountsPayableCurrent.json|
|기업의 모든 과거 재무 데이터|https://data.sec.gov/api/xbrl/companyfacts/CIK##########.json|
|특정 시점에 기업별 특정 재무 데이터 <br> (예: 2019년 1분기 매입채무)|https://data.sec.gov/api/xbrl/frames/us-gaap/AccountsPayableCurrent/USD/CY2019Q1I.json|
|전체 데이터 다운로드|http://www.sec.gov/Archives/edgar/daily-index/xbrl/companyfacts.zip|

`CIK##########.json` 이란것이 계속해서 나오는데 **CIK** 는 **Central Index Key**의 약자로 10개의 숫자로 구성된 회사마다 부여되는 유니크한 번호다. 예를들어 애플의 CIK는 *0000320193* 이다.

그런데 해당 회사의 데이터를 불러오려면 이 CIK를 알아야 하는데 어떻게 알 수 있을까?
방법은 여러가지가 있지만 가장 간단하게는 [SEC 사이트](https://www.sec.gov/edgar/searchedgar/companysearch.html)에서 원하는 회사 이름으로 검색하면 알 수 있다.
![CIK of Apple](https://velog.velcdn.com/images/choi-jiwoo/post/21c95433-d24f-48c0-b395-80dfefda4bb9/image.png)

다른 방법은 지금 소개하는 API에 포함되진 않지만 SEC에서 제공하는 또다른 방식으로 모든 등록된 회사의 CIK, 티커, 거래소 데이터를 가져올 수도 있다. 자세한 사항은 [이곳](https://www.sec.gov/os/accessing-edgar-data)에서 확인할 수 있다.

파이썬으로 가져와 보자
```python
import requests

url = 'https://www.sec.gov/files/company_tickers_exchange.json'
headers = {'User-Agent': 'Mozilla'}
res = requests.get(url, headers=headers)
cik_list = res.json()
```
![cik_list](https://velog.velcdn.com/images/choi-jiwoo/post/b9a10593-9d1d-40f3-a99b-657a19979983/image.png)

추가로 dictionary로 되어있는 cik_list를 pandas dataframe으로 만들어서 사용하면 손쉽게 검색이 가능해진다.
```python
import pandas as pd

cik_df = pd.DataFrame(cik_list['data'], columns=cik_list['fields'])
```
![result dataframe](https://velog.velcdn.com/images/choi-jiwoo/post/ffbc54c8-9e42-4501-a43a-040fde811624/image.png)

## Closing Words
API를 제공하기 시작한게 그렇게 오래 되지 않은것으로 알고 있는데 그래서인지 아직은 부족한 부분이 있는것 같다.

그리고 이 데이터들은 모두 회사의 재무와 관련된 데이터들 이어서 재무회계와 US-GAAP에 능통한 분들과 같이 사용하면 데이터에 가치를 최대치로 끌어올릴 수 있을것 같다.

## Reference
- [Accessing EDGAR Data](https://www.sec.gov/os/accessing-edgar-data)
- [Developer Resources](https://www.sec.gov/developer)
- [XBRL이란?](http://xbrl.or.kr/xbrl이란/)