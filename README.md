

# 시작하기

## 이슈

### CORS

> Django의 localhost : 8000
>
> Vue localhost : 8080
>
> Vue -> Django로 axios 요청을 보냈는데 브라우저에서 요청을 거부했다.
>
> 기본 정책 때문

* CORS 정책 

  * django가 설정해야함. 

    ```python
    # settings.py
    # CORS
    CORS_ORIGIN_ALLOW_ALL = True # 편의상 CORS 모든 도메인에서 허용
    CORS_ORIGIN_WHITELIST = [
        # 추후에 배포시 vue에서만 요청 보낼 수 있도록 정의!!
    ]
    ```

  * 헤더에 특정 정보를 담음

### 로그인

> Vue는 Django에게 로그인정보-JWT(username, password) 요청
>
> Django는 Vue에게 JWT (user 정보가 들어있음)를 보냄

* 웹 토큰이 한번 털리면 만료 전까지 계속 활용할 수 있다.
* 그렇기 때문에 세션 만료기간을 짧게 한다.
* 또한 토큰 길이가 길면 토큰을 주고 받을 때 네크워크가 좋지않은 환경에서는 안좋은 성능 초래할 수 있음

## 기본 설치 및 GITHUB 커밋

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

* 장고 커밋

  * `todo-django/` 

  ```shell
  # 현재 폴더는 todo-vue-django.
  # git init 했던 최상위 위치
  $ git add todo-django/
  $ git add .gitignore
  $ git commit -m 'Django | Init project'
  ```

* Vue 커밋

  ```shell
  $ git add todo-vue
  $ git add README.md
  $ git commit -m 'Vue | init Vue'
  ```

# vue

* 설치

  ```
  $ npm i vue-router
  ```

  i(install)

  ```
  $ vue add router
  ```

  

# Vue - DRF

## 1. 기본 설정

1. Django

   1. 가상환경 설정

      ```shell
      $ python -V
      3.7.4
      $ python -m venv venv
      $ source venv/Scripts/activate
      (venv) $
      ```

   2. 패키지 설치

      ```shell
      (venv) $ pip install django djangorestframework
      (venv) $ pip freeze > requirements.txt
      ```

   3. django 프로젝트 생성

      ```shell
      $ django-amdin startproject {프로젝트명} .
      ```

   4. Vue

      1. VueCLI 설치

         ```shell
         $ npm install -g @vue/cli
         ```

      2. Vue 프로젝트 생성

         ```shell
         $ vue create {프로젝트 이름}
         ```

      3. 프로젝트 폴더 구조

         ```
         todo-django-vue/
         	.git/
         	todo-django/
         		venv/
         		manage.py
         		todo_django/
         	todo_vue/
         	node_modules/
         	public/
         	src/
         	package.json
         ```

## 2. DRF 모델링

## 3. Vue

### Vue-router

```shell
$ npm i vue-router
$ vue add router
> Still proceed? y
> Use history mode for router? (Requires proper server setup for index fallback in production) y
```

```shell
$ npm i axios
```

* CORS(Cross-Origin Resource Sharing) policy 알아보기
  1. 브라우저에서 다른 도메인으로 요청 보낼 수 없게 만들어놓았다.
  2. Policy(CORS) - 서버가!

### Django server에서 CORS 설정하기

```
$ pip install django-cors-headers
$ pip freeze > requirements.txt
```

* [django github **django-cors-headers** ](https://github.com/adamchainz/django-cors-headers)

* settings에 corsheaders 추가

  ```python
  INSTALLED_APPS = [
      ...
      'corsheaders',
      ...
  ]
  ```

* 미들웨어 순서 중요!

  ```python
  MIDDLEWARE = [ 
      ...
      'corsheaders.middleware.CorsMiddleware',
      'django.middleware.common.CommonMiddleware',# CommomMiddleware 위에 반드시 추가
      ...
  ]
  ```

  ```python
  # CORS
  CORS_ORIGIN_ALLOW_ALL = True # 편의상 CORS 모든 도메인에서 허용
  
  CORS_ORIGIN_WHITELIST = [
      # 추후에 배포시 vue에서만 요청 보낼 수 있도록 정의!!
  ]
  ```


## 4. Todos axios 요청

1. getTodos 메소드 정의

   ```js
   // Home.vue
   getTodos() {
     // axios 요청
   axios.get('http://127.0.0.1:8000/api/v1/todos/')
     .then(response => {
       console.log(response) // 만약, 오류가 발생하게 되면 ESLint 설정을 package.json에 추가
       this.todos = response.data
     })
     .catch(error => {
       console.log(error)
     })
   }
   ```

2. mounted에서 호출

   ```js
   // Home.vue
   mounted() {
     this.getTodos()
   }
   ```

3. CORS 오류 발생

   * 해결하기 위해서는 django 서버에서 설정이 필요

4. `django-cors-headers` 패키지 활용

   * [Github 참고](https://github.com/adamchainz/django-cors-headers)

   ```shell
   $ pip install django-cors-headers
   ```

   * `INSTALLED_APPS`, `MIDDLEWARE` 설정
   * `CORS_ORIGIN_ALLOW_ALL` : True시 모든 도메인에서 요청 가능
   * `CORS_ORIGIN_WHITELIST` : 위의 옵션을 False로 하고, 화이트리스트에 직접 도메인 등록
   * 기타 옵션들도 확인해볼 것

5. Vue에서 다시 요청 보내보기

## 5. TodoForm component를 통해 투두 등록하기

## 6. 로그인 기능

> JWT(JSON Web Token, 토큰 기반 로그인 인증)
>
> 1. 클라이언트(Vue) 로그인 정보(username, password)를 서버(Django)로 전송
> 2. 서버는 해당 정보를 바탕으로 Token을 발급 및 암호화
> 3. 클라이언트는 Token을 받아서 매 요청때마다 헤더에 해당 정보를 추가해서 보냄
> 4. 서버에서는 매번 Token이 유효한지 확인
> 5. 클라이언트는 전송된 값을 디코딩하여 사용자 정보 활용
>
> JWT는 기본적으로 헤더, Payload, Verify signature로 구성된다.
>
> http://127.0.0.1:8000/api-token-auth/ 에서 발급받은 토큰을
>
> https://jwt.io/에서 직접 디코딩을 해볼 수 있다.

[JWT 참고](https://jpadilla.github.io/django-rest-framework-jwt/)



### 1) Django

```bash
$ pip install djangorestframework-jwt
$ pip freeze > requirements.txt
```



* vue-session

```bash
$ npm i vue-session
```

[Vue Session github 참고](https://github.com/victorsferreira/vue-session)

* `main.js` 에서 뷰 세션 넣어주기

  ```js
  import VueSession from 'vue-session'
  Vue.use(VueSession)
  ```

  

# 발생 이슈

* django request.POST

  * Form 으로 받아오는것

  * 만약에 뷰(자바 스크립트)에서 객체 형태로 데이터를 넘기면 무조건 request.data로 들어온다.

  * 따라서 request.POST로 받아오려면 자바스크립트에서 FormData Object를 생성해서 넘겨줘야한다.

    ```js
    const formData = new FormData()
    formData.append('title', title) // 변수 - 값
    axios.post('http://127.0.0.1:8000/api/v1/todos/', formData)
      .then(response => {
        console.log(response)
      })
      .catch(error => {
        console.log(error)
      })
    ```

    * formData로 넘겨주면 request.POST와 request.data 두 곳 모두 들어있다.
    * request.data가 더 범용적

  * request.POST vs request.data

    * request.POST : FormData로 POST 전송이 되었을 때
    * request.data : FormData로 POST 전송 및 data로 전송 모두

