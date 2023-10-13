```cmd
pip install django
pip install mysqlclient
```
## Django
### 프로젝트 생성
>프로젝트 할 폴더에서
```cmd
django-admin startproject [프로젝트 명]
```
>프로젝트 이름 폴더 내에서
```cmd
python manage.py startapp myapp01
```
### Setting
#### myapp01.py 사용
```python
	INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp01'
]
```
#### MySQL DB연결
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'djangodb',
        'USER': 'root',
        'PASSWORD': 'root',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```
#### 국가 설정(한국)
```python
LANGUAGE_CODE = 'ko'
TIME_ZONE = 'Asia/Seoul'
USE_I18N = True
USE_L18N = True
USE_TZ = True
```
#### static folder
>static 파일(CSS, JavaScript, Images) 폴더 위치
```python
STATIC_URL = '/myapp01/static/'
```
>autoincrement
```python
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```
#### setting후 연결
```cmd
python manage.py makemigrations

python manage.py migrate
```
#### 실행
```python
python manage.py runserver
```
### view 페이지 연결
>myDjango01/**urls.py**
```python
from django.contrib import admin
from django.urls import path
from myapp01 import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('write_form/',views.write_form) #views.py의 write_form
]
```
>myapp01/**views.py**
```python
def write_form(request):
  return render(request,'board/write.html') #기본적으로 tamplates폴더를 찾아간다
```
> templates폴더 안의 board폴더 안의 write.html 페이지와 연결된다.

### 테이블 생성
> models.py
```python
from django.db import models
from datetime import datetime

# Create your models here.
class Board(models.Model):
  idx = models.AutoField(primary_key=True)
  writer = models.CharField(null=False, max_length=50)
  title = models.CharField(null=False, max_length=200)
  content = models.TextField(null=False)
  hit = models.IntegerField(default=0)
  post_date = models.DateTimeField(default=datetime.now,blank=True)
  filename = models.CharField(null=False,blank=True,default='',max_length=50)
  filesize = models.IntegerField(default=0)
  down = models.IntegerField(default=0)

  def hit_up(self):
    self.hit +=1

  def down_up(self):
    self.down +=1
```
