---
layout: post
title:  "파이썬 명령 분배 패턴"
date:   2017-06-08 15:32:34 +0700
categories: [파이썬, 디자인 패턴]
---

### `명령 분배 패턴(Command Dispatch Pattern)`

일반적으로 사용하는 (undo가 있는)커맨드 패턴과는 다르다.

GoF는 명령 패턴을 요청이 객체로 캡슐화되어 있을 경우라고 기술한다. 

예전 1997년에 귀도 반 로섬은 비슷하게 기능을 수행하지만, 파이썬과 펄 과 같은 동적 언어에만 있는 패턴을 식별하였고 거기에 `명령어 분배(Command Dispatch)` 패턴이라고 명명했다.

슬프게도, 파이썬 프로그램에서 일반적으로 이 패턴을 많이 사용하고 있음에도 불구하고 그 이름은 거의 잊혀졌다. 이를 패턴으로 인지하는 것조차도 망각된 듯하다.

바깥의 소스로부터 전송된 수 많은 다양한 명령어들을 실행할 필요가 있는 클래스가 있다고 해보자. 

이를 처리하는 다양한 방법이 있다.

**1. 조건문으로 구분**

{% highlight python %}
if command == 'run':
    run()
elif command == 'fly':
    fly()
elif command == 'eat':
    eat()
{% endhighlight %}

**2. 딕셔너리 활용**
{% highlight python %}
dispatch_table = {
    'run': run,
    'fly': fly,
    'eat': eat,
}

# 명령 분배:
if dispatch_table.has_key(command):
    func = dispatch_table[command]
    func()
else:
    error()
{% endhighlight %}

**3. 명령분배 패턴**

{% highlight python %}
class Dispatcher:
    def do_run(self):
        pass
    
    def do_fly(self):
        pass
    
    def do_eat(self):
        pass
    
    def dispatch(self, command):
        mname = 'do_' + str(command)
        if hasattr(self, mname):
            method = getattr(self, mname)
            method()
        else:
            self.error()

{% endhighlight %}

귀도(Guido)가 언급했듯이: 나는 이 접근법이 아주 우아하다고 생각하며 그 동안 많이 사용했다는 것을 발견하였다.

이 패턴은 BaseHTTPServer, cmd, pydoc, repr, sgmllib, SimpleXMLRPCServer, urllib, distutils와 같은 파이썬 라이브러리 전반에 걸쳐서 사용되었다.


---
>**참조:**
>
> * [파이썬 디자인패턴](https://cryptosan.github.io/pythondocuments/documents/patterns-in-python/#id27)

