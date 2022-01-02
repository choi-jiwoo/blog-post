# pytest 유튜브 영상 추천

### One day...
테스팅에 대해 공부하던 중 파이썬에선 `unittest` 라는 프레임워크을 기본 라이브러리에 제공하고 있었고 [pytest](https://github.com/pytest-dev/pytest/) 라는 프레임워크가 있는 것을 알게 되었다.

여러 블로그, 유튜브를 찾아보다 `pytest`에 대한 좋은 영상이 있어서 다른 사람들과 공유하면 좋을 것 같다는 생각이 들었다.

영상은  2019년 3월에 진행된 [Python Frederick event](https://www.meetup.com/ko-KR/python-frederick/events/) 에서 발표한 영상으로 101과 201 두가지가 있었다. 발표자는 [Matt Layman](https://www.mattlayman.com/) 이라는 분으로 `pytest` 사용법에 대해 차근차근 아주 설명을 잘 해주셔서 발표 내용들이 쏙쏙 이해가 되었다. 영어지만 테스팅이랑 `pytest`가 익숙하지 않은 분들에게 추천👍

### Let's watch!
1. [Python Testing 101 with pytest](https://www.youtube.com/watch?v=etosV2IWBF0&t=3630s)

101 발표에선 주로 `pytest`에 기본적인 테스팅 방법을 소개한다. `assert` 키워드로 테스트 하는 것과  `exception` 을 테스트하는 내용이 나온다.

2. [Python Testing 201 with pytest](https://www.youtube.com/watch?v=fv259R38gqc&t=1659s)

201 발표에선 101 발표에서 했던 내용을 간략하게 설명하고 `pytest`에 특별한 특징들을 주로 다루었다. `parametrize` 기능으로 동일한 테스트를 여러 변수를 대입하면서 테스트 하는 것과 `fixture` 기능을 이용하는 것을 설명해준다. `fixture` 내용은 간단하게 사용법만 설명하고 슉 지나가서 조금은 아쉬운 부분이다. 그 이후에 `pytest` 에 built-in 된 `fixture` 몇가지를 (capsys, monkeypatch, tmpdir) 소개하고 발표가 마무리 된다.
<br>

각 영상 길이는 1시간 정도로 길다고 느낄 수 있지만 `pytest` 로 테스팅을 시작하기에 좋은 출발점이 될 수 있을 것 같다.

