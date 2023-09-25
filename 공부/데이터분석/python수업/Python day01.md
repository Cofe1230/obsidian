
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



### re 모듈
>[!note] re 모듈이란?
>Python에서 [[정규표현식]] 처리를 위해 사용하는 표준 라이브러리  
>정규 표현 패턴을 이용한 문자열의 추출이나 치환, 분할 등이 가능하다

>[!note] 메서드 요약
> - **compiler()** : 정규 표현 패턴의 컴파일
> - **match()** : 문자열이 매치되는가를 체크 (표현식에 해당 안되는 문자열은 등록 안된다)
> - **search()** : 
> - **fullmatch()** : 문자열이 전부 매치되는가 체크
> - **finditer()** : 매치된 부분 모두 이터레이터로 리턴
> - **findall** : 매치된 부분 모두 리스트로 리턴
> - **sub()** : 매치된 부분 치환
> - **split()** : 정규 표현 패턴으로 문자열을 분할

#### d01_re.py


![[Pasted image 20230925151351.png]]
![[Pasted image 20230925151417.png]]