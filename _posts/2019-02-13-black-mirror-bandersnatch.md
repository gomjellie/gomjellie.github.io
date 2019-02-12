---
layout: post
title:  "블랙미러 밴더스내치 웹사이트의 비밀"
date:   2019-02-13 02:07:34 +0700
categories: [웹]
---

# bandersnatch

블랙미러 밴더스내치를 보고 한동안 여운이 남아서 이것저것 이스터에그를 찾아보다가

발견하게된 사이트!

[https://tuckersoft.net/ealing20541/history/](https://tuckersoft.net/ealing20541/history/)

터커소프트회사 사이트가 실제로 있었다.

![Image Alt 밴더스내치-홈페이지](/static/img/_posts/black-mirror/tuckersoft-history.png)

친구랑 이것저것 얘기하다가 알게된 사실은 친구랑 나랑 뜨는 웹페이지 모습이 다르다는 것이였다.

밴더스내치 드라마가 그러하듯이 웹페이지도 여러가지 버전을 준비한것 같았다.

프라이빗 모드로 새로 들어갈때마다 웹페이지가 랜덤하게 바뀌는것같아서

쿠키를 까서보니 쿠키에 어떤 버전을 띄울지 정보가 저장되어 있었다.

쿠키가 뭔지 모르는 사람은 [꺼무위키](https://namu.wiki/)를 참고하길 바란다.

![Image](/static/img/_posts/black-mirror/cookie-1.png)

_ga, _gid 는 구글 애널리스틱스와 관련된 값을 담고있는 항목이고

지금 주목해서 봐야할 부분은 tl 값이다.

테스트 해본결과 tl 값은 0, 1, 2 를 갖을 수 있다.

현재 1인 tl의 값을 0으로 대입해서 바꿔보도록 하겠다.

![Image Alt 2대입](/static/img/_posts/black-mirror/assign-tl-2.png)

직접 해보고 싶다면 페이지 우클릭후 개발자 도구를 열고 console에서 위의 라인을 따라치면 된다. (_ga, _gid는 본인의 값으로 복붙해서 써야한다)

tl=2 가 되었다면 이제 페이지 새로고침을 해서 변화된 페이지를 확인해보자!

![Image Alt 2일때](/static/img/_posts/black-mirror/when-tl-2.png)

tl = 2 일때는 밴더스내치가 캔슬되고, 노즈다이브는 출시된 모습이다.

![Image Alt 0일때](/static/img/_posts/black-mirror/when-tl-0.png)

tl = 0 일때는 밴더스태치가 출시되고, 노즈다이브는 출시되지 않았다.

![Image Alt 1일때](/static/img/_posts/black-mirror/tuckersoft-history.png)

tl = 1 일때 밴더스내치와 노즈다이브가 둘다 출시되었다.


