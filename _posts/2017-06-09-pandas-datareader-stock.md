---
layout: post
title:  "파이썬으로 주식 데이터 가져오기"
date:   2017-06-09 16:32:34 +0700
categories: [파이썬, pandas, 주식]
---

***

2018년 3 월 10일 encoding 에러를 발생시키는 코드를 수정하였습니다.

# 수작업으로 다운받기


하나의 주식 종목데이터만 필요한 경우라면 그냥 브라우저로 야후 파이낸스 웹 사이트에서 다운 받을 수 있다.


예를들어 아프리카TV의 데이터를 얻고 싶다면,


[[야후 파이낸스]아프키라TV의 historical data](https://finance.yahoo.com/quote/067160.KQ/history?p=067160.KQ)


에서 Time Period를 설정하고 다운로드 버튼을 눌러 직접 다운받을 수 있다.


그러나 하나만 다운받을 것이아니라 대량으로 다운받기 위해서는 자동화 작업이 필요하다.


파이썬 pandas-datareader를 이용해서 자동화 시켜보자.


***

# 파이썬을 이용해 [finance.yahoo.com](finance.yahoo.com)에서 다운받기

필요한 패키지들을 설치한다.
{% highlight bash %}
pip install pandas-datareader
pip install fix_yahoo_finance
{% endhighlight %}

파이썬 코드:
{% highlight python %}
from pandas_datareader import data
import fix_yahoo_finance as yf
yf.pdr_override()

start_date = '1996-05-06' #startdate를 1996년으로 설정해두면 가장 오래된 데이터부터 전부 가져올 수 있다.
tickers = ['067160.KQ', '035420.KS'] #1 아프리카tv와 네이버의 ticker(종목코드)
afreeca = data.get_data_yahoo(tickers[0], start_date)
naver = data.get_data_yahoo(tickers[1], start_date)

{% endhighlight %}
```
#1
주목해야 할 점은 아프리카tv는 코스닥 종목이고, 네이버는 코스피 종목이라는 것.
코스닥은 ticker에 `.KQ`, 코스피는 `.KS`가 붙는다.
#2
data객체의 get_data_yahoo 를 이용해서 얻은 데이터들(아프리카, 네이버)은 DataFrame형의 객체이다.
(정확히는 pandas.core.frame.DataFrame)
data객체가 pandas-datareader에서 가져온 것임을 잊지말자
```

# 가져온 데이터 확인
afreeca.head()의 결과:
{% highlight bash%}
	Open	High	Low	Close	Adj Close	Volume
Date						
2003-12-22	3485.0	3485.0	3485.0	3485.0	2924.239990	18600
2003-12-23	3070.0	3290.0	3070.0	3070.0	2576.016113	1911800
2003-12-24	3065.0	3270.0	2830.0	3000.0	2517.279785	666100
2003-12-26	3000.0	3140.0	2900.0	2900.0	2433.370117	342200
2003-12-29	2720.0	3030.0	2625.0	2800.0	2349.460938	707000
{% endhighlight %}

naver.head()의 결과:
{% highlight bash%}
	Open	High	Low	Close	Adj Close	Volume
Date						
2002-10-29	8988.620117	8988.620117	8988.620117	36673.570313	8903.466797	100349
2002-10-30	10061.099609	10061.099609	9948.769531	41049.285156	9965.787109	4178437
2002-10-31	10214.299805	10459.500000	9325.700195	39007.371094	9470.056641	6465417
2002-11-01	9805.769531	10112.200195	8620.910156	36590.257813	8883.240234	3674734
2002-11-04	8886.480469	8947.769531	8304.259766	34756.539063	8438.057617	3387882
{% endhighlight %}
```
DataFrame객체의 head()함수를 사용하면 가장 위의 5줄을 리턴한다.
반대로 tail()함수를 사용하면 끝부분을 확인 할 수도 있다.
```

# 가져온 데이터 저장

afreeca객체와 naver객체가 제대로 저장되었음을 확인했으면, 이제는 파일로 저장할 차례이다.
afreeca와 naver객체는 `DataFreame`형 객체이다.
`DataFrame`에있는 `to_csv` 메소드를 사용해서 저장한다.

코드:
{% highlight python %}
naver.to_csv('./naver.csv')
{% endhighlight %}

csv가 아닌 [피클](https://gomjellie.github.io/파이썬/피클/2017/06/08/python-pickle.html)이나 엑셀로 저장 할 수도 있다.
{% highlight python %}
afreeca.to_pickle('./afreeca.pickle')
naver.to_excel('./naver.xls')
{% endhighlight %}

# 코스닥과 코스피의 모든 종목을 저장하기

이 작업을 하기위해 코스닥과 코스피의 모든 종목코드를 알아내는 일이 선행되어야 할 것이다. 

다음의 [피클](https://gomjellie.github.io/파이썬/피클/2017/06/08/python-pickle.html) 파일들을 다운받자.

>[kospi.pickle](https://github.com/gomjellie/system-trading/raw/master/app/data/name_ticker/pickle/kospi.pickle)
>
>[kosdaq.pickle](https://github.com/gomjellie/system-trading/raw/master/app/data/name_ticker/pickle/kosdaq.pickle)

kosdaq 1230개, kospi 768개의 종목코드가 담겨있는 [피클](https://gomjellie.github.io/파이썬/피클/2017/06/08/python-pickle.html)파일 이다.

다운 받았으면 python 인터프리터를 실행시킨 디렉토리에 피클을 위치시킨다.(위치가 중요함)

추가로 작성하는 파이썬 코드:
{% highlight python %}
import pandas as pd

kosdaq = pd.read_pickle('./kosdaq.pickle')
kospi = pd.read_pickle('./kospi.pickle')
{% endhighlight %}

### 가져온 피클 확인하기

피클파일에는 `DataFrame`형의 자료가 들어있었다.
따라서 `pd.read_pickle` 를 이용해서 가져온 데이터도 `DataFrame`형의 자료일 것이다.

kosdaq.head()로 안의 데이터를 확인하자.

```
	kor_name	ticker
0	제일홀딩스	003380
1	하나금융9호스팩	261200
2	교보7호스팩	267320
3	보라티알	250000
4	한화수성스팩	265920
```

kosdaq.tail()의 결과도 확인하자

```
	kor_name	ticker
1226	하이록코리아	013030
1227	SBI인베스트먼트	019550
1228	엠벤처투자	019590
1229	제미니투자	019570
1230	모헨즈	006920
```

이제 kosdaq, kospi 객체에서 ticker을 얻은후 루프를 돌리면 kospi, kosdaq 의 전종목 데이터를 얻을 수 있다.

# 반복문 만들기

현재 디렉토리에 kospi, kosdaq 폴더를 생성합니다.
{% highlight bash %}
$ mkdir kospi, kosdaq
{% endhighlight %}

추가로 작성하는 파이썬 코드:
{% highlight python %}
def save_kosdaq():
    for stock in kosdaq.values:
        kor_name = stock[0]
        ticker = stock[1]
        df = data.get_data_yahoo(ticker + '.KQ', '1996-05-06', thread=20)
        df.to_csv('./kosdaq/{}.csv'.format(ticker))
        print('{}.csv is saved'.format(ticker))

def save_kospi():
    for stock in kospi.values:
        kor_name = stock[0]
        ticker = stock[1]
        df = data.get_data_yahoo(ticker + '.KS', '1996-05-06', thread=20)
        df.to_csv('./kospi/{}.csv'.format(ticker))
        print('{}.csv is saved'.format(ticker))

save_kosdaq()
save_kospi()

{% endhighlight %}

야후에 쿼리를 많이보내면 빈파일들이 저장되는 문제가 있다.
분명히 종목코드.csv는 있는데 안에 내용이 아무것도 없다.

이 문제를 해결하기 위해 아래의 코드를 실행시킨다.

{% highlight python %}
import re
import glob
from pandas_datareader import data

def reload_empty(market):
    file_list = glob.glob('./{}/*.csv'.format(market))
    six_digit = re.compile('\d{6}')
    for file_name in file_list:
        file = pd.read_csv(file_name)
        if file.empty:
            print("empty file {} is updated".format(file_name))
            ticker = six_digit.findall(file_name)[0]
            tmp_df = data.get_data_yahoo(ticker+'.KQ', start='1996-05-06', thread=20)
            tmp_df.to_csv(file_name)

reload_empty('kosdaq')
reload_empty('kospi')
{% endhighlight %}

# 최종코드

{% highlight python %}
from pandas_datareader import data
import fix_yahoo_finance as yf
yf.pdr_override()
import pickle
import pandas as pd
import re
import glob

kosdaq = pd.read_pickle('./kosdaq.pickle')
kospi = pd.read_pickle('./kospi.pickle')

def save_kosdaq():
    for stock in kosdaq.values:
        kor_name = stock[0]
        ticker = stock[1]
        df = data.get_data_yahoo(ticker + '.KQ', '1996-05-06', thread=20)
        df.to_csv('./kosdaq/{}.csv'.format(ticker))
        print('{}.csv is saved'.format(ticker))

def save_kospi():
    for stock in kospi.values:
        kor_name = stock[0]
        ticker = stock[1]
        df = data.get_data_yahoo(ticker + '.KS', '1996-05-06', thread=20)
        df.to_csv('./kospi/{}.csv'.format(ticker))
        print('{}.csv is saved'.format(ticker))

save_kosdaq()
save_kospi()

def reload_empty(market):
    file_list = glob.glob('./{}/*.csv'.format(market))
    six_digit = re.compile('\d{6}')
    for file_name in file_list:
        file = pd.read_csv(file_name)
        if file.empty:
            print("empty file {} is updated".format(file_name))
            ticker = six_digit.findall(file_name)[0]
            _ticker = ticker + ['.KQ', '.KS']['market'=='kospi']
            tmp_df = data.get_data_yahoo(_ticker, start='1996-05-06', thread=20)
            tmp_df.to_csv(file_name)

reload_empty('kosdaq')
reload_empty('kospi')

{% endhighlight %}

>참조
>
> * [kospi-kosdaq-csv](https://github.com/gomjellie/kospi-kosdaq-csv)에서 2017-07-07 까지의 데이터를 받을 수 있다.
