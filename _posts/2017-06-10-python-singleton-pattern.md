---
layout: post
title:  "파이썬 싱글턴 패턴"
date:   2017-06-10 00:32:34 +0700
categories: [파이썬, 디자인 패턴]
---

### `싱글턴 패턴(Singleton Pattern)`

싱글턴 패턴은 두개 이상의 객체를 허용하지 않는다.

클래스에 오직 하나의 객체만을 허용한다.

객체를 여러개 생성하려고 하면 거부하고 첫번째 객체를 돌려준다.

{% highlight python %}

class Singleton(type):
    _instance = None

    def __call__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instance

{% endhighlight %}

싱글턴 패턴을 사용하려는 클래스는

다음과 같이 상속 받으면 된다:

{% highlight python %}

class MessageManager(metaclass=Singleton):
    pass

{% endhighlight %}

