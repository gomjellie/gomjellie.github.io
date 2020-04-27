---
layout: post
title:  "파이썬 데코레이터"
date:   2017-07-17 00:32:34 +0700
categories: [파이썬, 디자인 패턴]
---

# 데코레이터로 세션 관리하기

매번 함수가 실행되기전에 무언가 체크해야되는경우 데코레이터를 쓰면 편하다

{% highlight python %}
from collections import defaultdict

session = defaultdict()

def check_session(user_key):
    if user_key in session:
        return True
    
    return False

def yes_session():
    print('hello')

if check_session(user_key):
    yes_session()

{% endhighlight %}

다음과 같이 유저키를 통해 세션에 유저키가 있는지 체크하는 코드가 있을때, 세션에 키가 존재하는지 검사하는 코드를 `데코레이터`를 이용해서 만들어보자

{% highlight python %}
from collections import defaultdict

session = defaultdict()

def check_session(user_key):
    if user_key in session:
        return True
    
    return False

def session_deco(func):
    def wrapper(*args, **kwargs):
        if user_key in session:
            func(*args, **kwargs)
    return wrapper

@session_deco
def yes_session():
    print('hello')

yes_session()

{% endhighlight %}

매번 함수의 실행전, 후의 시간을 측정하는 경우도 `데코레이터`를 사용할 수 있다.

{% highlight python %}
import time

def timer_deco(func):
    def wrapper(*args, **kwargs):
        t1 = time.time()
        func(*args, **kwargs)
        t2 = time.time()
        dt = t2 - t1
        print('{} 함수가 실행되는데 걸린 시간: {}'.format(func.__name__,dt))
    return wrapper

@timer_deco
def test_func():
    for i in range(10000):
        print(i)

{% endhighlight %}
