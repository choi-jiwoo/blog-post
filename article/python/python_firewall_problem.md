# 'python' 응용프로그램이 들어오는 네트워크 연결을 허락하도록 하겠습니까?

<img width="261" alt="1" src="https://velog.velcdn.com/images/choi-jiwoo/post/02b5c595-06ff-4216-8ebe-f44c9300e9f2/image.png">

## 문제의 시작...
맥에서 [Streamlit](https://streamlit.io/)을 사용하여 웹앱을 만들고있는데 `streamlit run app.py` 명령어를 칠 때마다 위 사진과 같은 경고창이 뜬다.

매번 '허용' 눌러주기가 귀찮아서 해결방법을 인터넷에 찾아보았다.

그러다 이 [블로그](https://web.archive.org/web/20140228153242/http://silvanolte.com/blog/2011/01/18/do-you-want-the-application-to-accept-incoming-network-connections) 글을 발견하고 문제를 해결할 수 있었다.

Streamlit을 사용할 때 해결법은 아니지만 파이썬도 하나의 프로그램이니까 접근방식은 똑같았다.

## Let's Do It

우선 왜 이런 경고창이 계속 뜨는지 알아보자.

맥에서 잘모르는 앱에 대해서 네트워크 접근을 막으려는 보안정책 때문에 streamlit 앱을 실행시킬때 마다 방화벽에 걸려 경고창을 날리는것 같다.

그래서 위에 블로그에서 나온 해결방법이 해당 응용프로그램에 [코드서명](https://developer.apple.com/kr/support/code-signing/)을 추가하여 맥이 해당 앱을 안전하다고 판단하게 만다는것이다.

### 인증서 생성

#### '키체인 접근' > 인증서 지원 > 인증서 생성

<img width="617" alt="2" src="https://velog.velcdn.com/images/choi-jiwoo/post/2a4367b9-5123-47bd-9b72-5e6d1ddb2f35/image.png">


<img width="617" alt="3" src="https://velog.velcdn.com/images/choi-jiwoo/post/a98216e8-a686-4798-a24a-ca894a33cf14/image.png">


#### 인증서 생성 정보

<img width="617" alt="4" src="https://velog.velcdn.com/images/choi-jiwoo/post/c132dd63-cdd3-46c3-b8de-588181e040b6/image.png">

이름은 아무거나 원하는걸로 설정해주자.

<img width="617" alt="5" src="https://velog.velcdn.com/images/choi-jiwoo/post/0f8314b1-e2cc-4f9b-896c-4e282be7b930/image.png">

계속

<img width="617" alt="6" src="https://velog.velcdn.com/images/choi-jiwoo/post/2efde509-1b14-4a6f-b3bc-c014d8795c7a/image.png">

일련번호도 원하는걸로 설정.

<img width="617" alt="7" src="https://velog.velcdn.com/images/choi-jiwoo/post/89313fa3-bbeb-48f4-9b68-feeafe68f251/image.png">

원하는 정보를 입력하고 이후부턴 모두 '계속'으로 진행하면 된다.

<img width="617" alt="8" src="https://velog.velcdn.com/images/choi-jiwoo/post/ef4535f2-2858-4c8b-b57b-cb5aa1200099/image.png">

인증서 생성 완료!

인증서를 생성하였으니 인증서를 적용할 응용프로그램이 있는 위치를 알아보자.

### 파이썬 경로 확인

각자 파이썬이 설치되어있는 경로를 찾으면 된다. 근데 맥은 기본적으로 파이썬이 내장되어있기도 하고 Homebrew나 pyenv로 설치하기도 하고 참 여러 경로를 통해 설치될 때가 있다... 그리고 파이썬 설치 경로를 찾는 법도 여러 방법이 있을것이다. 나는 방화벽 문제니깐 방화벽 설정에 들어가봤을 때 아래 사진처럼 python3.10이 딱 등록되어있길래 우클릭해서 'Finder에서 보기'로 찾았다.

<img width="617" alt="10" src="https://velog.velcdn.com/images/choi-jiwoo/post/9d5b6424-ef55-4b75-b73c-eb9a1df861c3/image.png">

<img width="617" alt="11" src="https://velog.velcdn.com/images/choi-jiwoo/post/9ee739c0-629c-4359-a8ea-ca6085154f2d/image.png">

### 코드 서명

생성한 인증서와 파이썬 경로를 집어 넣어 코드 서명을 해준다.

#### 코드 서명 (sign)

`$ codesign -s "인증서이름" -f /코드서명할프로그램경로`

#### 인증 (verify)

`$ codesign -vvv /코드서명할프로그램경로`

<img width="617" alt="13" src="https://velog.velcdn.com/images/choi-jiwoo/post/91a88323-5c2c-46b3-a317-4e5d3acce6ba/image.png">

## Result

이제 streamlit 앱을 실행시키면 마지막으로 한번 경고창이 뜨고 그 이후에는 다시 뜨지 않는다!

## Closing Words

아직 배우는 입장에서 개발의 세계는 참 넓다..

## Reference

- [Do you want the application to accept incoming network connections?](https://web.archive.org/web/20140228153242/http://silvanolte.com/blog/2011/01/18/do-you-want-the-application-to-accept-incoming-network-connections)
- ['Do you want the application "main" to accept incoming network connections?' pop up while running Go applications](https://apple.stackexchange.com/questions/393715/do-you-want-the-application-main-to-accept-incoming-network-connections-pop)
- [코드 서명](https://developer.apple.com/kr/support/code-signing/)
- [맥OS 앱 코드사인 및 공증하기](http://cwyang.github.io/2020/12/09/osx-codesign-notarization.html)
