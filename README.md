* vue 설치한 적 없다면 아래와 같이 설치
  * -g : global 옵션

```shell
$ npm install -g @vue/cli
```

* cli 구조의 뷰 생성

```shell
$ vue create todo-vue
# (enter) => default
```

* django 서버 설치

  ```shell
  $ mkdir todo-django
  $ cd todo-django
  $ python -m venv venv
  $ activate
  ```

  * `$ activate`: `source venv/Scripts/activate`

    ```shell
    $ vi ~/.bashrc
    # 아래 내용 붙여넣기
    alias venv="source ~/python-virtualenv/3.7.4/Scripts/activate"
    alias activate "source venv/Scripts/activate"
    venv
    # esc + .wq 저장
    $ source ~/.bashrc # 실행
    ```

  ```shell
  $ pip install django
  $ pip install djangorestframework
  # django 프로젝트 시작
  $ django-admin startproject todo_django . # '-' 하이픈 안됨! 언더바쓰기
  ```

* gitignore 설정

  * `settings.py` 에 `INTALLED_APP` 에 `'rest_framework'`추가

  - `todo-vue`에 있는 `.gitignore` 밖으로 빼기

  - `.gitignore`에 추가

    ```.gitignore
    # django
    venv/
    db.sqlite3
    __pycache__/
    ```

* 환경설정 저장하기

  ```shell
  $ cd todo-django/
  $ activate # 가상환경
  $ pip freeze > requirements.txt
  ```

* tip) git add에서 `todo-django/`빼기

  ```shell
  $ git rm --cached -r todo-django/
  ```

  * -r : 폴더는 리컬시브 옵션을 줘야한다.
  * 만약 모든 파일을 하고 싶으면 `todo-django/`대신에 `.`를 써주면 된다.

* `todo-django/` git add

  ```
  # 현재 폴더는 todo-vue-django.
  # git init 했던 최상위 위치
  $ git add todo-django/
  ```

  