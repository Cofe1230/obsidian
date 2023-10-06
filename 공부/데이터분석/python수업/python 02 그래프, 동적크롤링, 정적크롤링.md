# Python day02
## 정적 크롤링
>[!note] 내용
> 정적 크롤링 , BeautifuSoup, re, pandas, csv파일 만들기
### BeautifuSoup로 원하는 태그 추출
```python
from bs4 import BeautifulSoup;

html2 = """
  <html><body>
    <h1 id='title'>스크레이핑이란?</h1>
    <p id='body'>웹 페이지를 분석하는 것</p>
    <p>원하는 부분을 추출하는 것</p>
    </body></html>
"""

soup = BeautifulSoup(html2, 'html.parser')
h1 = soup.html.body.h1
print(h1)
title = soup.find(id='title')
print(title)
print('body : ', soup.find(id='body').string)
```
### 여러 개의 li태그에서 원하는 속성값만 추출
>[!note] 목표
>a href 에서 https:// 로 시작하는 것만 추출하기
```python
from bs4 import BeautifulSoup;
import re;

html = """
    <ul>
        <li><a href="hoge.html">hoge</li>
        <li><a href="https://example.com/fuga">fuga*</li>
        <li><a href="https://example.com/foo">foo*</li>
        <li><a href="shttps://example.com/foobbb">foobbb*</li>
        <li><a href="http://example.com/aaa">aaa</li>
    </ul>
"""

soup2 = BeautifulSoup(html,'html.parser')
#https 와 shttps를 다 찾는다
#lis = soup2.find_all(href=re.compile(r'https://'))
#https로 시작하는 것만 찾으려면 정규표현식 ^ : ~로 시작하는 을 사용한다
lis = soup2.find_all(href=re.compile(r'^https://'))
print(lis)
for e in lis :
  print(e.attrs['href'])
```
### 특정 페이지에서 정적 크롤링 (find, select)
>[!note] 목표
>영화이름 평점 예매율 추출 및 출력

```python
from bs4 import BeautifulSoup;
import requests;

#영화이름 평점 예매율

req = requests.get("https://movie.daum.net/ranking/reservation")
html = req.text
soup = BeautifulSoup(html,'html.parser')

#select 사용
ol = soup.select_one('#mainContent > div > div.box_ranking > ol')
items = ol.select('li>div.item_poster>div.thumb_cont')
data = []
for i in items :
  title = i.select_one('strong.tit_item>a').get_text()
  grade = i.select_one('span.txt_append>span.info_txt').get_text()
  reserve = i.select_one('span.txt_append>span.info_txt').findNextSibling().get_text()
  data.append([title,grade,reserve])
print(data)

#find 사용
ols = soup.find('ol',class_='list_movieranking')
lis = ols.find_all('div',class_='thumb_cont')
for i in lis :
  moviename = i.find('a', class_='link_txt').string
  moviegrade = i.find('span','txt_grade').get_text()
  movieReser = i.find('span',{'class':'txt_num'}).get_text()
  print('영화 : ',moviename, end =' / ')
  print('평점 : ', moviegrade, end=' / ')
  print('예매율 : ', movieReser)
```
### 정적 크롤링 (특정 힘든 td), csv 만들기(기본, pandas)
```python
from bs4 import BeautifulSoup;
import requests;
import pandas as pd;

#https://www.weather.go.kr/weather/observation/currentweather.jsp
#지역, 온도 습도
req = requests.get('https://www.weather.go.kr/weather/observation/currentweather.jsp')
soup = BeautifulSoup(req.text,'html.parser')

tbody = soup.select_one('#weather_table > tbody')
data = []
for tr in tbody.select('tr') :
  tds = tr.select('td')
  print('지역 = ',tds[0].text )
  print('온도 = ',tds[5].text )
  print('습도 = ',tds[-4].text )
  print()
  data.append([tds[0].text,tds[5].text,tds[-4].text])
print(data)

#리스트 하나씩 빼먹어 보기
for item in data :
  print(item)

#일반적인 csv파일 만들기
with open('weather33.csv','w') as file :
  file.write('point, temp, hum \n')
  for item in data:
    row = ','.join(item) #scv파일 ,로 연결하기
    file.write(row+'\n')

# csv파일 pandas로 읽기
df = pd.read_csv('weather33.csv',encoding='euc-kr')
print(df)

#pandas를 이용한 csv파일 저장
df1 = pd.DataFrame(data, columns=('point','temp','hum'))
df1.to_csv('weather44.csv', encoding='euc-kr', index=False)

df = pd.read_csv('weather44.csv',encoding='euc-kr')
print(df)
```
## 그래프로 그리기
>[!note] 처음 쓰는 라이브러리

```python
import matplotlib as mpl;
import matplotlib.pyplot as plt;
```
### 추출한 csv파일을 그래프로 그리기(특정 칼럼만 panda loc)
```python
import pandas as pd;
import matplotlib as mpl;
import matplotlib.pyplot as plt;

df = pd.read_csv('weather44.csv',index_col='point',encoding='euc-kr')
print(df)

#서울, 인천, 대전, 대구, 광주, 부산, 울산
print(df.loc[['서울','인천']])
city_df = df.loc[['서울','인천','대전','대구','광주','부산','울산']]
print(city_df)

# 그래프 그리기
# 한글
font_name = mpl.font_manager.FontProperties(fname='C:/windows/fonts/malgun.ttf').get_name()
mpl.rc('font',family=font_name)

# 차트종류, 제목, 차트크기, 범례, 폰트 크기 설정
ax = city_df.plot(kind='bar',title='날씨',figsize=(12,7),legend=True,fontsize=12)
ax.set_xlabel('도시',fontsize=12)
ax.set_ylabel('기온/습도',fontsize=12)
ax.legend(['기온','습도'])

plt.show()
```
>[!note] 결과

![[Pasted image 20231006131737.png]]
## 동적 크롤링
>[!note] 알아둘 것
> - 크롤링 허용 여부는 사이트마다 다르기 때문에 알아볼 필요가 있다
> - 주소/robots.txt 로 접속하면 어느정도 허용하는지 알 수 있다
### Selenium 기본 사용법
>[!note] selenium 기본적인 사용법
```cmd
pip install selenium
```
> - **크롬 버전 확인** : 도움말 -> Chrome 정보

![[Pasted image 20231006132259.png]]

> - 버전에 맞는 가상 드라이브 [다운로드](https://chromedriver.chromium.org/downloads )
> - 작업 폴더에 chromedrive.exe 파일 추가

### 동적 크롤링 기본 (By 사용)
```python
from selenium import webdriver as wd
from selenium.webdriver.common.by import By

driver = wd.Chrome()
driver.get('https://naver.com')

#홈페이지 소스코드를 보고 원하는 행동을 한다
#파이썬을 입력해서 검색 버튼을 클릭하는 코드
driver.find_element(By.ID, 'query').send_keys('파이썬')
driver.find_element(By.CLASS_NAME, 'btn_search').click()
```
### 동적 크롤링 데이터 추출 (BeautifulSoup 사용)+utf-8 인코딩 csv+re로 필요없는 부분 삭제
```python
from selenium import webdriver as wd
from bs4 import BeautifulSoup
import re;

driver = wd.Chrome()
driver.get('https://www.melon.com/chart/index.htm')

#F12의 소스에는 내용이 있지만
#페이지소스에는 없기 때문에 정적 크롤링을 하면 데이터를 받아 올 수 없다

#f12 눌러서 소스 보는 것 처럼 받아오는 방법
page = driver.page_source
# print(page)

#코드를 받아오는데 성공했기 때문에 이전 했던 방식으로 원하는 값을 추출
soup = BeautifulSoup(page,'html.parser')
tbody = soup.select_one('#frm > div > table > tbody')
trs = tbody.select('tr#lst50')

data = []
for tr in trs :
  rank = tr.select_one('span.rank').get_text()
  name = tr.select_one('div.ellipsis.rank01 > span > a').get_text()
  singer = tr.select_one(' div.ellipsis.rank02 > a').get_text()
  album = tr.select_one('td:nth-child(7) > div > div > div > a').get_text()
  like = tr.select_one('td:nth-child(8) > div > button > span.cnt').get_text()
  like = re.sub('\n총건수\n','',like) #필요없는 부분 삭제
  like = re.sub(',','',like)

  data.append([rank,name,singer,album,like])
print(data)

# melon.csv 순위, 곡명, 가수, 앨범, 좋아요\n
with open('melon.csv','w', encoding='utf-8-sig') as file :
  file.write('순위,곡명,가수,앨범,좋아요\n')
  for item in data:
    row = ','.join(item)
    file.write(row+'\n')
```
## 정적 크롤링 (request get 사용이 안될 때)
>[!note] 개요
>크롤링이 전체 허용이 아닐 경우 단순히 get으로 받아 왔을 때 정적 페이지 소스 코드가 추출이 안 될 수 있다. 일부 허용에 사용이 가능하다면 header를 추가해 정적 페이지 추출이 가능하다
>다음은 멜론의 페이지를 Chrome으로 접근해서 정적 페이지 코드를 받아오는 예시

```python
import requests;
from bs4 import BeautifulSoup;
import re;

header = {'User-Agent' : 'Mozilla/5.0'}
req = requests.get('https://www.melon.com/chart/index.htm', headers=header)
soup = BeautifulSoup(req.text, 'html.parser')
print(soup)
tbody = soup.select_one('#frm > div > table > tbody')
trs = tbody.select('tr#lst50')

data = []
for tr in trs :
  rank = tr.select_one('span.rank').get_text()
  name = tr.select_one('div.ellipsis.rank01 > span > a').get_text()
  singer = tr.select_one(' div.ellipsis.rank02 > a').get_text()
  album = tr.select_one('td:nth-child(7) > div > div > div > a').get_text()
  like = tr.select_one('td:nth-child(8) > div > button > span.cnt').get_text()
  like = re.sub('\n총건수\r\n\t\t\t\t\t\t\t\t\t\t\t\t0\r\n\t\t\t\t\t\t\t\t\t\t\t','',like) #필요없는 부분 삭제
  like = re.sub(',','',like)
  data.append([rank,name,singer,album,like])
print(data)
```
>[!note] 헤더에 넣어야 할 정보

![[Pasted image 20231006133802.png]]

```cmd
pip install lxml

```