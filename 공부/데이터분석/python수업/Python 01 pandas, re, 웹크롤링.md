# pandas, re, 웹크롤링
## 라이브러리 추가
### pandas
```
pip install pandas
```
### requests
```
pip install requests
```
### beautifulsoup4
```
pip install beautifulsoup4
```
### Workbook
```
pip install openpyxl
```
## 수업 내용
### python 파일 실행 방법
>[!note] cmd
> python [파일이름].py

![[Pasted image 20230925093100.png]]
### csv 파일 처리
#### usecsv.py
>[!note] 함수
> - opencsv : csv 파일을 읽어서 리스트로 리턴
> - switchcsv : list 내부 숫자에 **" , "**  처리 (1,000 => 1000) 

```python
def opencsv(filename):
  f = open(filename, 'r')
  reader = csv.reader(f)
  output = []
  for i in reader :
    output.append(i)
  return output

def switchcsv(listName):
  for i in listName:
    for j in i:
      try:
        i[i.index(j)] = float(re.sub(',','',j))
      except:
        pass
  return listName
```
#### d01_apt.py
##### csv파일 읽기
>[!note] csv파일 읽기
>앞서 만든 csv 파일 처리 py를 임포트해서 사용

```python
import usecsv

ap = usecsv.opencsv("apt_201910.csv")
apt = usecsv.switchcsv(ap)
```
##### 데이터 처리
>[!note] 파이썬 문법
> - 리스트에서 원하는 개수만 읽기 :  리스트이름[:개수]
> - 리스트의 개수 : len(리스트이름)

```python
# apt_201910.csv 파일에서 3줄 출력
print(apt[:3])
# apt_201910.csv 총 개수
print(len(apt))
```
>[!note] 원하는 필드를 출력하고 싶다
```python
# 시군구 단지명 가격 ==> 6개 출력
# 시군구,번지,본번,부번,단지명,면적,계약년월,계약일,가격,층,건축년도,도로명
# 0,1,2,3,4,... 뒤에서 -1,-2,-3
for i in apt[:6]:
  print(i[0],i[4],i[-4])
```
>[!note] 조건이 있다면? if문을 사용하자
```python
import re

# 강원도에 120m^2 이상 3억  이하 검색하기 시군구 단지명 가격 출력
new_list = [['시군구','단지명','가격']]
for i in apt:
  try:
    if i[5] >=120 and i[-4] <= 30000 and re.match('강원',i[0]):
	  #list에 추가하기 위해 append 사용
      new_list.append([i[0],i[4],i[-4]])
  except:
    pass
print(new_list)
```
>[!note] 해당 내용을 csv파일로 만들어보자
```python
import scv

with open("apt11.csv",'w',newline='') as f:
	a = csv.writer(f,delimiter=',')
	a.writerows(new_list)
```

### pandas
>[!note] pandas란?
>데이터 분석에 자주 사용하는 테이블 형태를 다루는 라이브러리
>- 1차원 자료구조 : Series
>- 2차원 자료구조 : DataFrame(많이 쓰게 될 것)
>- 3차원 자료구조 : Panel

>[!info] 컬렉션 표현 방식
>- 리스트 : [ ]
>- 튜플 : ( )
>- 딕셔너리 : { }

#### d01_pandas.py
>[!note] pandas 기본 사용법

```python
import pandas as pd

#data란 dictionary가 있다고 보자
data = {
  'name' : ['Mark', 'Jane', 'aaa', 'rr'],
  'age' : [33 , 44 , 55 , 11],
  'score' : [91.2 , 88.5 , 55.6 , 88.0]
 }
 #pandas로 쓰려면?
 df = pd.DataFrame(data)
```
>[!note] 어떤 형태인지 출력해보자

```python
	print(df)
	print(type(df))
```
![[Pasted image 20230925133931.png]]
>테이블 형태를 확인 할 수 있다

>[!note] 타입은?

![[Pasted image 20230925134144.png]]
> 이러한 타입으로 출력된다

>[!note] 기본적인 데이터 가공을 해보자
```python
#열을 기준으로 더한다
print(df.sum())
# age의 평균
print(df['age'].mean())
#df['age']와 df.age는 같은 표현식
print(df.age)
```


d01_apt_pandas
행과 열의 개수를 알려준다
shape
![[Pasted image 20230925105153.png]]
앞, 뒷부분을 출력해서 파일이 어떤식으로 만들어져 있는지 확인 가능하다
head
tail
![[Pasted image 20230925105336.png]]
#### d01_pandas2.py
>[!note] pandas안에 들어가는 데이터를 직접 입력해보자
```python
#index는 행 columns는 열 이름을 나중에 지정할 수 있다.
df2 = pd.DataFrame([[78.5, 92.4, 73.5], [76.3, 56.6, 96.3]],
                   index=['중간고사','기말고사'],
                   columns = ['1반', '2반', '3반'])
print(df2)                   
```
>결과
![[Pasted image 20230925135115.png]]

>[!note] 새로운 column을 만들어보자

```python
df3 = pd.DataFrame([[20201101, 'Kim', 90, 95],[20201102, 'Lee', 80, 85],
                    [20201103, 'Hong', 70, 75], [20201104, 'Park', 60, 95]],
                    columns=['학번','이름','중간고사','기말고사'])
                    
#새로운 column을 만들때는 df3.{columnName} 이런식의 표현은 불가능하다                   
df3['총점'] = df3.중간고사 + df3.기말고사
print(df3)
```
>[!warning] 새로운 column 만들때 주의점
>- df3['총점'] = df3.중간고사 + df3.기말고사 식의 표현은 가능하지만
>- df3.총점 = df3.중간고사 + df3.기말고사 이런 표현은 불가능하다

>[!note] pandas 사용 추가로 알아둘 것

```python
df4 = pd.DataFrame([[20201101, 'Kim', 90, 95],[20201102, 'Lee', 80, 85],
                    [20201103, 'Hong', 70, 75], [20201104, 'Park', 60, 95]
#column이 없다면 0, 1, 2, 3 이렇게 표시된다                    
print(df4)
#중간에 column을 입력 할 수 있다					
df4.columns = ['학번','이름','중간고사','기말고사']
print(df4)
#tail은 끝애서 5개 출력				
print(df4.tail())
```
> 결과
![[Pasted image 20230925172449.png]]

>[!note] csv파일로 만들어 보자
```python
df3.to_csv('pandas2.csv',header='False')
```
>[!note] csv 파일 읽기는?
```python
df5 = pd.read_csv('pandas2.csv',encoding='utf-8')
print(df5)
```



### re 모듈 + String 메서드 연습
>[!note] re 모듈이란?
>Python에서 [[정규표현식]] 처리를 위해 사용하는 표준 라이브러리  
>정규 표현 패턴을 이용한 문자열의 추출이나 치환, 분할 등이 가능하다

>[!note] re 메서드 요약
> - **compiler()** : 정규 표현 패턴의 컴파일
> - **match()** : 문자열이 매치되는가를 체크 (표현식에 해당 안되는 문자열은 등록 안된다)
> - **search()** : 문자열 중에 해당 매치되는 부분을 반환
> - **fullmatch()** : 문자열이 전부 매치되는가 체크
> - **finditer()** : 매치된 부분 모두 이터레이터로 리턴
> - **findall()** : 매치된 부분 모두 리스트로 리턴
> - **sub()** : 매치된 부분 치환
> - **split()** : 정규 표현 패턴으로 문자열을 분할

>[!note] [[String 메서드]]

#### d01_re.py - String 처리 + re 기본
##### String 메서드로 문자열 처리
```python
#주어진 문자열
text = "<title>지금은 문자열 연습입니다.</title>"
```

>[!note] 문자열을 원하는 개수로 찍기
```python
#0부터 7문자까지 출력
print(text[0:7])
```
>[!note] 문자열에서 특정한 문자 찾기
```python
#find 사용 - 있으면 위치값 반환 없으면 -1
print(text.find('문')) #11
print(text.find('파')) #-1
#index 있으면 위치값 반환 없으면 error
print(text.index('문')) #11
#print(text.index('파')) #error
```
>[!note] 필요없는 공백을 제거해보자
```python
#strip() => 공백제거 lstrip() 왼쪽 공백 제거 rstrip() 오른쪽 공백 제거
print(text1.strip()+text2)
print(text1.lstrip()+text2)
print(text1.rstrip()+text2)
```
>[!note] 특정 문자열을 교체해보자
```python
#<title>을 <div>로 변경 repace 메서드는 replace(a, b, count)로 count를 적지 않으면 모두 변경
print(text1.replace("<title>","<div>"))
#<title>을 공백으로 변경
print(text1.replace("<title>",""))
```

##### Re를 사용해보자
>[!note] 정규표현식으로 특정 문자열을 찾아 출력해보자
```python
#<head>태그 안에 있는 텍스트만 빼고 싶다
text3 = ('111<head>안녕하세요</head>22')
#문자열 중에 <head.*/head>에 해당하는 부분만 추출
body = re.search('<head.*/head>',text3) 
print(body) # <re.Match object; span=(3, 21), match='<head>안녕하세요</head>'>
body = body.group()
print(body) # <head>안녕하세요</head>
```
>[!note] 표현식에 해당하는 문자열을 공백으로 만들어보자
```python
body = "<title>지금은 문자열 연습</title>"
#<.+?> 표현식에 해당하는 문자열을 공백으로 변경
body = re.sub('<.+?>','',body)
print(body) #지금은 문자열 연습
```
#### d01_re2.py re 이용
##### 대사집 txt파일 문자열 처리
>[!note] 대사집 txt 파일
```python
import codecs

f = codecs.open('friends101.txt', 'r', encoding='utf-8')
script101 = f.read()
```
>[!note] 특정 인물 대사 추출
```python
#추출하고자 하는 특정 패턴을 찾아 추출, Monica:문자 로 추출한 예시
Line = re.findall(r'Monica:.+',script101)
print(Line)
#n개만 추출하고 싶다면?
print(Line[:3])
#특정한 n번째 추출 뒤에서 세는게 빠르다면 -를 사용하면 된다
print(Line[-1])
#추출한 것이 몇개인가?
print(len(Line))
```
>[!note] 추출하고자 하는 정규 표현식을 따로 변수에 뺄 수 있다(재사용성)
```python
# compile : 추출하는 포맷을 지정하는 함수 대문자 이후 소문자~문자열 :로 끝남
char = re.compile(r'[A-Z][a-z]+:') # 모든 대사집에서 이름: 을 추출
# 모든 대사를 하는 사람을 추출하는 예시 (중복이 있다)
print(re.findall(char,script101))
```
>[!note] 리스트에서 중복을 제거하고 싶다
```python
a = [1, 2, 3, 4, 5, 2, 2]
print(a)
#set : 중복을 허용하지 않는 자료구조 list -> set으로 변경해 중복을 없앤다
print(set(a)) 
```
```python
# 위의 예시에서 중복된 set으로 하나로 만든다
y = set(re.findall(char,script101))
# 다시 set 자료구조를 처리를 위해 list로 변경
z = list(y)
# 출연 사람만 뽑아내기 위해 ':'를 제거해서 변수에 저장
character = []
for i in z :
  character += [i[:-1]] #마지막 하나를 제거해서 charter 리스트에 추가
print(character)

#re를 사용해서 같은 결과
character2 = []
for i in z :
  char = re.sub(':','',i) # ':' 를 '' 으로 바꾼다 위와 결과는 같으나 응용하기에 더 좋다
  print(char,end='\t') # for문을 쓸때 옆으로 출력하게 하고 싶으면
  character2.append(char)
print(character2)
```
##### re 추가 연습
>[!note] 특정 문자열에서 원하는 데이터 추출 예시
```python
a = '제 이메일 주소는 greate@naver.com'
a += ' 오늘은 today@naver.com 내일은 apple@gmail.com life@abc.co.kr 라는 메일을 사용합니다.'
print(a)
# 추출할 부분 패턴을 생각하고 정규표현식을 사용하여 추출하자
a1 = re.findall(r'[a-z]+@[a-z.]+',a) # 소문자 영문 + @ + .이 포함된 소문자 영문
print(a1) #[greate@naver.com,today@naver.com,apple@gmail.com life@abc.co.kr]
```
>[!note] 리스트에서 특정 패턴의 문자열만 추출할 때 (findall, search, match의 차이)
```python
#해당 문자열이 주어지고 여기서 a로 시작하는 문자열만 저장하고 싶다
words = ['apples', 'cat', 'brave abc', 'drama', 'asise', 'blow', 'coat', 'above']
#사용할 정규 표현식은 a[a-z]+

#findall을 사용하면 패턴에 해당하는 문자열을 모두 찾아서 반환
mm = []
for i in words:
  mm += re.findall(r'a[a-z]+',i)
print(mm) #['apples', 'at', 'ave','abc', 'ama', 'asise', 'at', 'above']

#search를 사용하면 패턴에 해당하는 문자열을 하나만 찾아서 반환
for i in words:
  m = re.search(r'a[a-z]+',i)
  if m:
    print(m.group()) #'apples', 'at', 'ave', 'ama', 'asise', 'at', 'above' 
					 #brave abc는 ave를 반환하고 abc는 반환되지 않는다

#match를 사용하면 주어진 문자열에서 첫번째 글자부터 패턴이 일치하는 문자열을 반환
for i in words :
  m = re.match(r'a\D+',i) #\d(숫자) \D(숫자 아닌) >> a이후로 숫자가 아닌것 모두 추가
  if m:
    print(m.group()) #apples, asise, above
```
### 정적 웹 크롤링
#### d01_cr01.py - html 처리
>[!note] 연습 해볼 html
```python
html = """
  <html><body>
    <h1>스크레이핑이란?</h1>
    <p>웹 페이지를 분석하는 것</p>
    <p>원하는 부분을 추출하는 것</p>
    </body></html>
"""
```
>[!note] BeautifulSoup을 이용한 html 처리 기본
```python
from bs4 import BeautifulSoup;

soup = BeautifulSoup(html, 'html.parser')
```
>[!note] 특정 태그에 있는 것을 추출해보자
```python
#h1에 있는 것을 출력해보자
h1 = soup.html.h1
print(h1) #<h1>스크레이핑이란?</h1>
p = soup.html.p
print(p) #<p>웹 페이지를 분석하는 것</p>
```
>[!note] p가 두개인데 두번째 p를 추출하고 싶으면?
```python
p1=p.next_sibling.next_sibling
print(p1)
```
>[!note] 태그 없이 문자만 추출하고 싶으면
```python
#h1의 문자만 출력해보자
print("h1 : ", h1.string)
```
#### d02_dr02.py - html 처리
>[!note] html
```python
html = """
    <html><body>
        <div id="meigen">
            <h1>위키북스 도서</h1>
            <ul class="items">
                <li>유니티 게임 이펙트 입문</li>
                <li>스위프트로 시작하는 아이폰 앱 개발 교과서</li>
                <li>모던 웹사이트 디자인의 정석</li>
            </ul>
        </div>
    </body></html>
"""
```
```python
from bs4 import BeautifulSoup;

soup = BeautifulSoup(html, 'html.parser')
```
>[!note] html에서 '위키북스 도서'라는 문자열을 추출할거다
```python
#기본적인 h1태그를 찾아서 추출
h1 = soup.find('h1').string
#select_one을 쓸 수 있다 one은 한개 여러개일대는 select
h1_1 = soup.select_one('h1').string
#div태그 안의 h1태그
h1_2 = soup.select_one('div>h1').string
#meigen id를 가진 div태그 안의 h1태그
h1_3 = soup.select_one('div#meigen > h1').string
```
>[!note] li태그에 있는 내용들을 추출해부자
```python
#li는여러개 있다 select를 사용해서 list로 추출
li_list = soup.select('div#meigen > ul.items > li')
for li in li_list:
  print(li.string)
```
#### d01_cr03.py - 웹 크롤링 01
>[!note] 실제 웹 사이트에서 크롤링 해보자
```python
import requests;
from bs4 import BeautifulSoup;

#requests를 사용해 사이트 정적html을 가져온다
res = requests.get("https://news.daum.net/digital")

#BeautifulSoup 으로 html parser
soup = BeautifulSoup(res.content,'html.parser')

#a 태그에 class가 list_txt 인 태그의 내용을 추출
link_title = soup.find_all('a', class_='link_txt')

#필요한 내용을 가공해서 사용하면 된다
for i in link_title:
  print(i.get_text().strip()) #태그 안의 내용 텍스트만, 공백 제거
```
#### d01_cr04.py - 웹 크롤링 02
>[!note] re를 사용해서 웹 크롤링을 해보자
```python
import requests;
from bs4 import BeautifulSoup;
import re

res = requests.get("https://news.daum.net/economic/")
soup = BeautifulSoup(res.content, "html.parser")
```
>[!note] 추출할 데이터

![[Pasted image 20230925151351.png]]

> [!note] 특정 조건에 맞는 데이터를 추출해보자
```python
#href속성이 있는 a태그를 모두 추출
links = soup.select('a[href]')

#a[href=https://v....]로 시작하는 것에 들어있는 것 텍스트를 출력하고 싶다
for i in links:
  #a태그에서 re를 사용해 https://v가 있을 경우만 안의 내용을 출력
  if re.search('https://v.\w+', i['href']):  # .: 임의의 문자 1개, \w : 숫자와 문자
    print(i.get_text().strip())
```
>[!note] 결과

![[Pasted image 20230925151417.png]]
#### d01_cr05.py - 웹 크롤링 03
>[!note] 로또 사이트에서 결과를 추출해보자
```python
import requests;
from bs4 import BeautifulSoup

res = requests.get("https://m.dhlottery.co.kr/gameResult.do?method=byWin")
soup = BeautifulSoup(res.content,"html.parser")
```
>[!note] 추출할 데이터

![[Pasted image 20230926120542.png]]

>[!note] 필요한 데이터 추출
```python
#find_all , select 둘다 가능하다
lottery_num = soup.find_all('span', class_ = 'ball')
lottery_num2 = soup.select('span.ball')
for i in lottery_num:
  print(i.get_text(), end='\t')
```
>[!note] 추출할때 팁
```python
#개발자 도구에서 추출할 값 우클릭 => Copy => Copyselector
#container > div > div.bx_lotto_winnum > span:nth-child(1)
nums = soup.select('#container > div > div.bx_lotto_winnum > span.ball')
for i in nums:
  print(i.string, end='\t')
```
#### d01_cr06.py - 웹 크롤링 04 +엑셀로 출력
>[!note] naver 주식에서 인기 검색 종목을 크롤링 해보자
```python
import requests
from bs4 import BeautifulSoup
from openpyxl import Workbook

res = requests.get("https://finance.naver.com/")
soup = BeautifulSoup(res.content, "html.parser")
```
>[!note] 크롤링 할 데이터

![[Pasted image 20230926121725.png]]

>[!note] 데이터 추출
```python
tbody = soup.select_one('#container > div.aside > div > div.aside_area.aside_popular > table > tbody')
trs = tbody.select('tr')
data = []
for i in trs:
  name = i.select_one('th>a').get_text()
  price = i.select_one('td').get_text()
  ch_direction = i.select_one('td>img')['alt'] #alt="상승""하락" 속성의 값 가져오는 방법
  #ch_direction = i['class'][0] # up down
  ch_price = i.select_one('td>span').get_text().strip()
  data.append([name,price,ch_direction,ch_price]) #리스트에 추가
print(data)
```
>[!note] 엑셀 파일로 만들기
```python
write_wb = Workbook()
#결과라는 시트가 생성된다
write_ws = write_wb.create_sheet('결과')
for d in data:
  write_ws.append(d)
write_wb.save(r'testfinance.xlsx')
```
>[!note] 결과

![[Pasted image 20230926122601.png]]