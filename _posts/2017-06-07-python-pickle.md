---
layout: post
title:  "파이썬객체를 파일로 저장하기"
date:   2017-06-08 16:00:34 +0701
categories: [파이썬, 피클]
---

`피클 모듈`을 사용하면, `파이썬의 객체`를 `파일`로 저장했다가 불러올 수 있다.
저장 가능한 객체는 `primitive`타입에 한정되지 않는다.

{% highlight python %}
import pickle

_ = {'layout': 'post', 'categories': ['파이썬', '피클']}

# _ 객체를 저장하기
pickle.dump(_, open('./cur_data.pickle', 'wb'))

# pickle파일로 부터 객체를 불러오기
pickle.load(open('./cur_data.pickle'))
{% endhighlight %}

open해놓은걸 close해줘야 될거같은데 간략하게 보이기 위해 생략함.
