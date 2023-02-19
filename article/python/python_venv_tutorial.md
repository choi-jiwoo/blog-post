# 파이썬 가상환경 설정 with venv

## 문제의 시작...
처음 파이썬으로 프로그래밍을 시작했을 때 여러 디렉터리에서 작업을 했고 그때 그때 필요한 패키지들을 막 설치하곤 했는데 그러다 보니 프로젝트별 패키지 관리가 엉망 이었다. 특히 특정 프로젝트에 필요한 패키지들을 나열하기 위해  `requirements.txt`를 생성하고 Github에 push 하려는데 참 난감했다. 프로젝트 마다 패키지들을 관리할 수 있는 어떤 좋은 방법이 없을까?

**정답은 '가상환경'!**

**가상환경**을 사용하면 프로젝트마다 별도의 환경에서 작업을 할 수 있게 된다. 파이썬 버전도 따로 설정할 수 있고 패키지도 필요한 패키지들만을 적절하게 설치해서 사용할 수 있다. 이렇게 하면 `requirements.txt`를 생성하는것도 깔끔하게 해결된다.

## Let's Do It

터미널을 열고 가상환경을 생성 해보자. 파이썬3 부터 가상환경 설정을 위해 `venv`라는 라이브러리를 제공하는데 만약 파이썬2 라면 `virtualenv`를 사용하면 된다. (본글은 Mac OS를 기준으로 작성하였음을 알린다)

```shell
$ cd [디렉터리]
$ python -m venv [가상환경이름]
$ source [가상환경이름]/bin/activate
```

위와 같이 해주면 가상환경이 생성되고 이제 마음껏 패키지들을 설치하면 된다. 

가상환경을 비활성화 하려면 아래와 같이 해주면 된다.

```shell
$ deactivate
```

## One More Thing...
VS Code, PyCharm 같은 에디터에서 python interpreter를 생성한 가상환경으로 설정해주면 가상환경에서 작업할 수 있게 된다.


## Reference

- [Installing packages using pip and virtual environments](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)