# VS Code pylint unable to import 문제 해결

### 문제의 시작...

VS Code에서 python에 사용할 linter로 pylint를 설정 해주었는데 아래 사진과 같은 import error 가 발생했다. 


![1](https://velog.velcdn.com/images/choi-jiwoo/post/53801d00-e50e-4400-a874-ab55f21960ff/image.png)


구글링을 해보니 pylint에 path와 관련된 문제였다. pylint를 가상환경에서 설치하였기 때문에 pylint가 다른 디렉터리에 있는 모듈을 인식할 수 없는것이었다.

그럼 문제를 알았으니 해결해보자.

### Let's do it

pylint에 `init-hook`을 설정해주면 되는데 이게 뭘까 찾아보았다. 구글링을 해보는데 Import error에 대한 해결 방법으로 `init-hook`을 설정해주면 된다라는 내용은 많이 있었는데 구체적인 내용은 찾기가 힘들었다. 

계속 구글링을 해보다 보니 `init-hook`은 initialization hooks를 의미하고 pylint가 초기실행(initialize)될 때 파이썬 코드를 실행시켜주는 옵션이다. 주로 `sys.path`를 설정하는데 사용된다. 이번 문제도 path와 관련된 문제로 pylint에게 모듈이 있는 path를 알려주면 pylint가 모듈을 찾을 수 있게 되고 `import`를 정상적으로 할 수 있게 되는 것이다.

`init-hook` 설정은 .vscode 폴더 안에 settings.json 에 다음과 같은 설정을 추가해 주면 된다.
```{JSON}
"python.linting.pylintArgs": [
    "--init-hook",
    "import sys; sys.path.append('path to folder your module is in')"
]
```

![2](https://velog.velcdn.com/images/choi-jiwoo/post/cbbe994a-874e-4ce1-9e8b-38b78dcff4ca/image.png)

![3](https://velog.velcdn.com/images/choi-jiwoo/post/82a517a7-6c72-43b6-9a16-236b9edb2b59/image.png)

`append()` 안에 자신의 모듈이 있는 디렉터리를 적어주면 끝!

![4](https://velog.velcdn.com/images/choi-jiwoo/post/4eca2a23-78d7-4267-8fe2-541e1ca2efd9/image.png)

이제 import error가 보이지 않는다!

### One more thing...

PEP 328에 따르면 모듈은 절대경로를 사용하여 import를 해야 한다고 명시하고 있다. 그 이유로 파이썬의 라이브러리가 확장되고 여러 패키지들이 소개되면서 같은 이름의 모듈이 생겨나게 되었고 그냥 `import foo`를 하게 되면 이게 어떤 foo를 말하는 것인지 알기 어려워지는 상황이 되었기 때문이다. 

> As Python's library expands, more and more existing package internal modules suddenly shadow standard library modules by accident. It's a particularly difficult problem inside packages because there's no way to specify which module is meant.

구글에서도 오픈소스 프로젝트를 위한 코드 스타일 가이드를 제시하고 있는데 Python Style Guide 에서도 마찬가지로 절대경로를 사용하여 `import`하라고 명시하고 있다.

> Do not use relative names in imports. Even if the module is in the same package, use the full package name. This helps prevent unintentionally importing a package twice.

이러한 사실들을 적용하면 pylint에 import error를 다른 방법으로도 해결할 수 있다.

바로 import 하려는 모듈들을 절대경로를 사용하여 import 하면 pylint 에서도 다른 설정 없이 모듈을 읽을 수 있게 된다. 

```python
from 패키지 import 모듈
```

하지만 이렇게 하면 `python 패키지/모듈.py` 방식으로 파이썬을 실행하면 `ModuleNotFoundError: No module named '모듈'` 이라는 에러 메세지를 받게 될것이다. python 실행시 패키지 디렉터리를 sys.path 제일 첫번째에 (`sys.path[0]`) 지정하게 되기 때문에 이미 그 패키지 디렉터리 안에 들어와 있는 상황이니 해당 패키지를 찾을 수 없는 것이다. 따라서 파이썬 실행을 `python -m 패키지.모듈` 방식으로 해주어야 한다.

### Reference

- [PEP 328 – Imports: Multi-Line and Absolute/Relative](https://www.python.org/dev/peps/pep-0328/)
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html#s2.2-imports)
- [Absolute imports in python not working, relative imports work](https://stackoverflow.com/questions/45448182/absolute-imports-in-python-not-working-relative-imports-work)

- [pylint false positive E0401 import errors in vscode while using venv](https://stackoverflow.com/questions/51095449/pylint-false-positive-e0401-import-errors-in-vscode-while-using-venv)
- [Pylint configuration](https://www.getcodeflow.com/pylint-configuration.html)
- [Call Python script from pylint init-hook](https://sam.hooke.me/note/2019/01/call-python-script-from-pylint-init-hook/)

