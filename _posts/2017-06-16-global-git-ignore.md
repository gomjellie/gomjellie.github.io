---
layout: post
title:  "전역 gitignore 설정하기"
date:   2017-06-16 00:32:34 +0700
categories: [git]
---


홈 디렉토리에 .gitignore를 만든다:
{% highlight bash %}
vim ~/.gitignore
{% endhighlight %}

특별한 설정이 없으면 아래의 설정을 붙여 넣으면 된다

vi에디터로 붙여넣기 하기전에 `:set paste`를 한번 해주면 줄 밀림 없이 잘 들어간다.

{% highlight bash %}
# Folder view configuration files
.DS_Store
Desktop.ini

# Thumbnail cache files
._*
Thumbs.db

# Files that might appear on external disks
.Spotlight-V100
.Trashes

# Compiled Python files
*.pyc

# Compiled C++ files
*.out

# Application specific files
venv
node_modules
.sass-cache
{% endhighlight %}

마지막으로 이렇게 입력해주면 끝난다.
{% highlight bash %}
git config --global core.excludesFile ~/.gitignore
{% endhighlight %}
