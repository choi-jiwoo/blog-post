# 'python' 응용프로그램이 들어오는 네트워크 연결을 허락하도록 하겠습니까?

<img width="261" alt="1" src="https://user-images.githubusercontent.com/47313851/172755933-a55473b6-8cbc-4610-9bfd-51de99a839be.png">

### Problem
[Streamlit](https://streamlit.io/)을 사용하여 웹앱을 만들고있는데 `streamlit run app.py` 명령어를 칠 때마다 위 사진과 같은 경고창이 뜬다.

매번 '허용' 눌러주기가 귀찮아서 해결방법을 인터넷에 찾아보았다.

그러다 이 [블로그](https://web.archive.org/web/20140228153242/http://silvanolte.com/blog/2011/01/18/do-you-want-the-application-to-accept-incoming-network-connections) 글을 발견하고 문제를 해결할 수 있었다.

Streamlit을 사용할 때 해결법은 아니지만 파이썬도 하나의 프로그램이니까 접근방식은 똑같았다.

### Let's start!

우선 왜 이런 경고창이 계속 뜨는지 알아보자.

맥에서 잘모르는 앱에 대해서 네트워크 접근을 막으려는 보안정책 때문에 streamlit 앱을 실행시킬때 마다 경고창을 날리는것 같다.

그래서 위에 블로그에서 나온 해결방법이 해당 응용프로그램에 [코드서명](https://developer.apple.com/kr/support/code-signing/)을 추가하여 맥이 해당 앱을 안전하다고 판단하게 만다는것이다.

나는 [pyenv](https://github.com/pyenv/pyenv)로 파이썬 버전을 관리하고 있고 사용하고있는 파이썬 버전은 3.10이다.

### Reference

- https://web.archive.org/web/20140228153242/http://silvanolte.com/blog/2011/01/18/do-you-want-the-application-to-accept-incoming-network-connections
- https://apple.stackexchange.com/questions/393715/do-you-want-the-application-main-to-accept-incoming-network-connections-pop
- https://developer.apple.com/kr/support/code-signing/
