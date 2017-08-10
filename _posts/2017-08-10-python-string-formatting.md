---
layout: post
title:  "파이썬에서 문자열을 formatting 하는 4가지 방법"
date:   2017-08-10 00:32:34 +0700
categories: [파이썬]
---

# 1. [%-formatting](https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting)

python docs 에서 printf-style String Formatting 이라고 소개하는 방법이다.

C언어의 sprintf, printf함수의 포맷팅과 비슷하게 사용할 수 있는데,

주의해야 할 점은 인자가 1개일 경우

{% highlight python %}
print('%d' % 10)
{% endhighlight %}
이런식으로 튜플로 감싸지 않고 사용할 수 있지만,

인자가 2개 이상인 경우

{% highlight python %}
print('%d %d' % (10, 20))
{% endhighlight %}

튜플혹은,

{% highlight python %}
print("%(foo)d %(bar)d" % {'foo':10, 'bar':20})
{% endhighlight %}

딕셔너리로 감싸서 인자를 넘겨줘야 한다.

# 2. [str.format()](https://docs.python.org/3/library/string.html#formatstrings)

{% highlight python %}
"{" [field_name] ["!" conversion] [":" format_spec] "}"
{% endhighlight %}

의 형식을 따른다

field_name 이 생략될경우 인자가 앞에서부터 짝지어 순서로 들어간다.

conversion 은 r, s, a 중 하나이며

{% highlight python %}
"Harold's a clever {0!s}"     # Calls str() on the argument first
"Bring out the holy {name!r}" # Calls repr() on the argument first
"More {!a}"                   # Calls ascii() on the argument first
{% endhighlight %}

이걸 보고 이해햐면 된다.

# 3. [str.Template]()

python3 docs에 있는 아래 예제를 보면 이해가 될 것이다.

{% highlight python %}
>>> from string import Template
>>> s = Template('$who likes $what')
>>> s.substitute(who='tim', what='kung pao')
'tim likes kung pao'
>>> d = dict(who='tim')
>>> Template('Give $who $100').substitute(d)
Traceback (most recent call last):
...
ValueError: Invalid placeholder in string: line 1, col 11
>>> Template('$who likes $what').substitute(d)
Traceback (most recent call last):
...
KeyError: 'what'
>>> Template('$who likes $what').safe_substitute(d)
'tim likes $what'
{% endhighlight %}

# 4. [f-string](https://docs.python.org/3/reference/lexical_analysis.html#f-strings)

python 3.6 버전에 추가된 포매팅 방법이다.

python3 docs에 있는 아래 예제를 보면 이해가 될 것이다.

{% highlight python %}
>>> name = "Fred"
>>> f"He said his name is {name!r}."
"He said his name is 'Fred'."
>>> f"He said his name is {repr(name)}."  # repr() is equivalent to !r
"He said his name is 'Fred'."
>>> width = 10
>>> precision = 4
>>> value = decimal.Decimal("12.34567")
>>> f"result: {value:{width}.{precision}}"  # nested fields
'result:      12.35'
{% endhighlight %}

람다함수가 허용된다.
{% highlight python %}
>>> f'{(lambda x: x*2)(3)}'
'6'
{% endhighlight %}

너무 당연한 얘기지만 큰따옴표 안에서 큰따옴표로 인덱싱하는건 안된다.

{% highlight python %}
f"abc {a["x"]} def"  # error
f"abc {a['x']} def"  # works
{% endhighlight %}
