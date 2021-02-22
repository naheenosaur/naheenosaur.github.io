---
title:  "python의 middleware"
date:   2021-01-30 21:00:00 +0900
tags: [python]
key: 20210130_04
---

# Middleware

Middleware는 Django의 request, response 처리에 연결되는 framework이다. Django의 input과 output을 전반적으로 변경하기 위한 가볍고, 저수준의 plugin시스템이다.

각각의 middleware component 는 어떤 특정 함수를 실행 할 때 수행합니다. 예를 들어, Django는 `HEAD` request에 대한 모든 response 에  HTTP 헤더에 `"X-View"`를 추가하는 `XViewMiddleware`라는 middleware component 를 포함한다.

이 문서는 middleware 가 어떻게 동작하는지, 어떻게 middleware 를 활성화시킬 수 있는지, 어떻게 자체 middleware 를 작성할 수 있는지에 대해 설명한다. Django는 바로 사용할 수 있는 built-in middleware 를 전달한다.  *[built-in middleware reference](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/middleware.html)* 에서 더 자세한 내용을 다룬다.

## Activating middleware

middleware component를 활성화 시키기 위해 장고세팅에서 [MIDDLEWARE_CLASSES](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/settings.html#std:setting-MIDDLEWARE_CLASSES) 리스트를 추가한다.  [MIDDLEWARE_CLASSES](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/settings.html#std:setting-MIDDLEWARE_CLASSES) 에서 각각의 middleware component 는 middleware 의 클래스 이름의 Python 전체 경로 string으로 나타난다. 예를 들어 [django-admin.py startproject](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/django-admin.html#django-admin-startproject) 에서 생성된 기본 [MIDDLEWARE_CLASSES](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/settings.html#std:setting-MIDDLEWARE_CLASSES) 이다. 

```python
MIDDLEWARE_CLASSES = (
    'django.middleware.common.CommonMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
)
```

[process_request()](https://django-doc-test-kor.readthedocs.io/en/old_master/topics/http/middleware.html#process_request) 와 [process_view()](https://django-doc-test-kor.readthedocs.io/en/old_master/topics/http/middleware.html#process_view) middleware 의 request 구문에서 Django는  [MIDDLEWARE_CLASSES](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/settings.html#std:setting-MIDDLEWARE_CLASSES) 에 선언된 middleware의 순서대로 적용한다. [process_response()](https://django-doc-test-kor.readthedocs.io/en/old_master/topics/http/middleware.html#process_response) and [process_exception()](https://django-doc-test-kor.readthedocs.io/en/old_master/topics/http/middleware.html#process_exception) middleware 의 response 구문에서, class 는 반대로 아래에서부터 위로 적용된다. 각각의 middleware class 가 view 를 layer로 감싸고 있는 양파모양이라고 생각하면 쉽다.

![https://django-doc-test-kor.readthedocs.io/en/old_master/_images/middleware.png](https://django-doc-test-kor.readthedocs.io/en/old_master/_images/middleware.png)

Django 를 설치할 때 middleware 는 필요가 없다. 예를들어 [MIDDLEWARE_CLASSES](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/settings.html#std:setting-MIDDLEWARE_CLASSES) 는 비어있을 수 있지만, 최소한 [CommonMiddleware](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/middleware.html#django.middleware.common.CommonMiddleware)는 사용하길 강력하게 권장한다.

## Writing your own middleware

middleware 를 작성하는것은 어렵지 않다. 각각의 middleware component는 아래 소개하는 method 하나이상을 선언한 Python class이다.

### **process_request**

**`process_request(*self*, *request*)`**

`request`는 [HttpRequest](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpRequest) 객체다. 이 method는 Django가 어느 view 를 실행할 지 결정하기 전에 각각의 request 에서 호출된다.

`process_request()` 는 `None` 혹은 [HttpResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpResponse) 객체를 반환해야 한다. 만약 `None` 을 반환한다면 Django는 이 request 를 계속해서 진행하면서 다른 middleware와 적절한 view를 실행할 것이다. 만약에 [HttpResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpResponse) 객체를 반환한다면 Django는 다른 reuest, view, exception middleware, 적절한 view 를 호출하지 않고  [HttpResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpResponse) 를 반환할 것이다. response middleware는 모든 response 에서 호출된다.

### **process_view**

**`process_view(*self*, *request*, *view_func*, *view_args*, *view_kwargs*)`**

`request`는 [HttpRequest](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpRequest) 객체다. `view_func`는 Django가 사용하려는 Python 함수 이다. ( 함수의 string 이름이 아닌 실제 함수 객체이다. ) `view_args` 는 view에 넘겨지는 인수목록이고, `view_kwarfs` 는 view 에 넘겨지는 keyword 인수 dictionary이다. `view_args` 나 `view_kwargs` 는 첫번째 view 인자인 `request` 를 포함하지 않는다.

`process_view()` 는 Django가 view 를 호출하기 바로 전에 호출된다. `None` 나 [HttpResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpResponse) 객체를 호출한다. 만약 `None` 를 호출하면 Django는 계속해서 이 request 를 진행하고, 다른 `process_view()` middleware와 적절한 view 를 수행한다. 만약 [HttpResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpResponse) 객체를 반한하면 Django는 다른 request, view, excpetion middleware, 적절한 view 를 호출하려 하지 않고 [HttpResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpResponse) 를 호출할 것이다. response middleware 는 모든 response에서 호출된다.

- **Note**

### **process_template_response**

**New in Django 1.3:** *[Please see the release notes](https://django-doc-test-kor.readthedocs.io/en/old_master/releases/1.3.html)*

**`process_template_response(*self*, *request*, *response*)`**

`request` 는 [HttpRequest](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpRequest) 객체다. `response`는[SimpleTemplateResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/template-response.html#django.template.response.SimpleTemplateResponse) (e.g. [TemplateResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/template-response.html#django.template.response.TemplateResponse)) 혹은 render method 를 구현한 response 객체의 하위 클래스이다.

`process_response()` 는 `render` method 를 구현한 response 객체를 반환해야 한다.  `response.template_name` 이나 `response.context_date`를 변경해 주어진  `response` 를 수정하거나 새로운 [SimpleTemplateResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/template-response.html#django.template.response.SimpleTemplateResponse)나 비슷한 것을 생성해서 반환할 수 있다.

`process_template_response()` 는 response instance 가 `render()` 메소드를 갖고 있을 때만 호출된다. [TemplateResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/template-response.html#django.template.response.TemplateResponse) 나 비슷한 것이다.

명확하게 응답값을 render 할 필요는 없다. 응답은 일단 template response middleware 가 호출되면 자동으로 render 된다.

middleware 는 response 구문과 반대로 실행고, process_template_response 를 포함한다.

### **process_response**

**`process_response(*self*, *request*, *response*)`**

`request` 는  [HttpRequest](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpRequest) 객체이다. `respose` 는 Django view 에 의해 반환되는 [HttpRequest](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpRequest) 객체이다.

`process_response()` 는  [HttpRequest](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpRequest) 를 반환해야 한다. 주어진 `response` 를 수정하거나, 새로운  [HttpRequest](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpRequest) 를 생성하고 반환할 수 있다.

`process_request()` 나 `process_view()` method와 달리, `process_response()` method는  이 전의 middleware method가 [HttpResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpResponse) 를 반환해서 같은 middleware 클래스의 `process_request()` 나 `process_view()` method 가 생략되어도 항상 호출된다. 작성한 `process_response()` method 가 `process_request()` 에  설정되어 있는 것을 따를 수 없는 것과 같다. 게다가 response 구문 동안 class 는 역순으로 수행된다. 이 말은 [MIDDLEWARE_CLASSES](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/settings.html#std:setting-MIDDLEWARE_CLASSES)  가 가장 처음 실행된다는 의미이다.

### **process_exception**

**`process_exception(*self*, *request*, *exception*)`**

`request` [HttpRequest](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpRequest) 객체이다. `exception` 은 view 함수에서 생성된 `Excaption` 객체이다.  

Django 는 view가 예외를 발생 시킬 때 `process_exception()` 을 호출한다. `process_exception()` 은 `None` 이나 [HttpResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpResponse) 객체를 반환해야 한다. 만약 [HttpResponse](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/request-response.html#django.http.HttpResponse)  객체를 반환할면, response 는 브라우저로 돌아갈 것이고 아니면 기본 예외 처리로 진행된다.

다시 말하지만 middleware 는 `process_exception` 을 포함하여 response 구문동안 반대로 진행된다. 만약 exception middleware 가 response 를 반환하면, 그 middleware 위의 middleware class 는 아무것도 호출하지 않는다.

### **__init__**

대부분의 middleware class는 middleware class 가 실제로는 `process_*` method의 placeholder 기 때문에 초기화하지 않아도 된다. 만약 전반적인 상태를 변경하고 싶다면 `__init__`을 사용해야 한다. 하지만 몇가지 명심해야 하는 것이 있다.

- Django는 아무 인자도 없이 middleware를 초기화 하기 때문에, 인자를 필요로 하는 `__init__` 을 선언할 수 있다.
- `process_*` method가 request 마다 한번씩 호출되는것과 달리 `__init__` 은 웹서버가 시작될 때 한번만 호출된다.

### **Marking middleware as unused**

가끔 런타임때 middleware가 사용될 것인지 확인하는 것은 유용하다. 이 경우에, middleware 의 `__init__` method 는 `django.core.exceptions.MiddlewareNotUsed`  를 발생시킨다. Django는 이 middleware 과정에서 middleware 를 제거할 것이다.

### **Guidelines**

- middleware class 는 subclass 할 필요 없다.
- middleware class 는 Python 경로 어디에 있어도 상관없다. Django는 [MIDDLEWARE_CLASSES](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/settings.html#std:setting-MIDDLEWARE_CLASSES) 세팅이 포함하는 경로만 신경쓴다.
- 예제는 *[Django’s available middleware](https://django-doc-test-kor.readthedocs.io/en/old_master/ref/middleware.html)* 에서 확인하면 된다.

참조 : https://docs.djangoproject.com/en/3.1/topics/http/middleware/