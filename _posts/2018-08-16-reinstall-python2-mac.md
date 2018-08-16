---
layout: post
title:  "python2 깔끔하게 다시깔기"
date:   2018-08-16 10:07:34 +0700
categories: [파이썬2, 재설치]
---

뭔짓을 하다가 그런건지 모르겠는데

갑자기 pip2가 안된다......!

날라갔다... python2도 뭐 버전별로 복잡하게 많길래 그냥 다 날려버리기로 했다 하하

또 이럴지 모르니까 미래의 나를 위해서 일단 기록해둬야 겠다. (전에도 한번 이랫고 앞으로도 또 날릴거같으니까)


```sh

brew uninstall python2

brew uninstall --ignore-dependencies python2

brew uninstall --force python@2

sudo rm -rf "/Applications/Python 2.7"

brew install python2

```

깔리다가 에러로 머가 뜨면서 취소된다...

/usr/local/lib/python2.7/site-packages/pip 여기에 뭐 권한이 없다고 했던거같다

```sh
sudo chown -R $USER /usr/local/lib/python2.7/site-packages/pip

brew reinstall python2


```

된다...


```sh

pip install --upgrade pip setuptools

ls -l /usr/local/bin/pip # 링크가 잘 됐는지 확인해준다

```

끝....
