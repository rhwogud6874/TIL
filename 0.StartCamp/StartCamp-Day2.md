###  StartCamp-Day2

# Git bash 명령어
`ls` (현재 작업디렉토리 내 파일 확인)
`pwd` (현재 작업디렉토리 확인)
`cd` (change directory),			 ex)`cd TIL`
`cd ..` (디렉토리 뒤로가기)
`cd ~` (홈폴더 가기)
`cd + [Tab key]` (파일 찾기)
`mkdir` (make directory) 			 ex)`mkdir 0.startCamp`
`touch` (파일 만들기) 				ex)`touch day1.md`
`rm` (파일 지우기)					ex) `rm day1.md`
`rm -r` (지우기)					ex) `rm -r 0.EndCamp`
위방향 화살표 -> 최근에 사용한 명령어로 이동

`code .` (현재 폴더에서 비쥬얼스트디오 열어줘, code=비쥬얼스튜디오 실행, .=현재폴더)




# 비쥬얼 스튜디오

STARTCAMP(폴더) 마우스 올리고 New file, browser.py
`Ctrl + shift + p` => `> Terminal: Select default shell` => `Git bash`
터미널 내용지우기 : `ctrl + L`



### 인터넷 창 키기


```python
import webbrowser

webbrowser.open("naver.com")
webbrowser.open_new_tab("google.com")
```
```python
import webbrowser

url = "https://search.naver.com/search.naver?sm=top_hty&fbm=0&ie=utf8&query="

webbrowser.open(url)

my_keyword = "iu"
webbrowser.open(url+my_keyword)

my_keywords = ["samsung", "lg", "sk", "lotte"]
for i in my_keywords:
webbrowser.open(url+i)
```



### 코스피 크롤링

STARTCAMP(폴더) 마우스 올리고 New file, kospi.py
터미널에서 `pip install requests`, `pip install `

```python
import requests

res = requests.get("https://naver.com")
print(res.text)
```

```python
import requests
from bs4 import BeautifulSoup

response = requests.get("https://finance.naver.com/sise/").text 
## text 안붙히면 [200] 으로 나옴
soup = BeautifulSoup(response, 'html.parser')
kospi = soup.select('#KOSPI_now')  
##F12후에 찾기로 원하는값 찾고 그 값 명령줄에 우클릭->copy->copy selector
##one 안붙히면 리스트 형태로 나옴
print(kospi)
```


### 네이버 검색어 1위 크롤링

STARTCAMP(폴더) 마우스 올리고 New file, naver.py

```python
import requests
from bs4 import BeautifulSoup

response = requests.get("https://naver.com").text
soup = BeautifulSoup(response, "html.parser")
naver = soup.select_one("#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_list.PM_CL_realtimeKeyword_list_base > ul:nth-child(5) > li:nth-child(1) > a.ah_a > span.ah_k")

print(naver)
```



### 임의의 텍스트 만들기

STARTCAMP(폴더) 마우스 올리고, New folder, New file, files,py

```python
from faker import Faker
import os

f = Faker('ko_KR')
for i in range(100):
    filename = f"{i}_{f.name()}.txt"
    print(filename)
    cmd = f"touch {filename}"
    os.system(cmd)
```

`mv files.py ..` => 파일을 상위폴더로 옮기기
`mv ABC.txt DEF.txt` =>파일이름 바꾸기
`print(os.listdir("."))` =>현재폴더 파일 보여주기
`os.listdir(".")` => 디렉토리 내의 파일 리스트

### 텍스트에 이름 붙이기

```python
import os

os.chdir(r"C:\Users\student\startcamp\students")
for filename in os.listdir("."):
    os.rename(filename, "SAMSUNG_" + filename)
```
### 텍스트에 이름 대체하기

```python
import os

os.chdir(r"C:\Users\student\startcamp\students")
for filename in os.listdir("."):
    os.rename(filename, filename.replace("SAMSUNG_SSAFY_","SSAFY_"))
```



위아래 같은 식

```python
tbody = soup.select_one('tbody')
tr = tbody.select('tr')
print(tr)

tr = soup.select('tbody > tr') ##tbody 안에 있는 tr값

```



### 환율 크롤링

```python
import requests
from bs4 import BeautifulSoup
url = "https://finance.naver.com/marketindex/exchangeList.nhn"
res = requests.get(url).text
soup = BeautifulSoup(res, "html.parser")

tr = soup.select('tbody>tr')
for r in tr:
    r.select_one('.sale').text
    print(r.select_one('.sale').text)
    
   
```

```python
import requests
from bs4 import BeautifulSoup
url = "https://finance.naver.com/marketindex/exchangeList.nhn"
res = requests.get(url).text
soup = BeautifulSoup(res, "html.parser")

tr = soup.select('tbody>tr')
for r in tr:
    print(r.select_one('.tit').text.strip()) 
    ##strip 은 여백을 날리는 명령
    print(r.select_one('.sale').text)

```

### github 올리기

1. github 회원가입
2. Git GUI에서 
	git init
	ls -a
	touch .gitignore
3. https://gitignore.io 들어가서 python, window, visualstudiocode 누른 후 복사
4. visualstudiocode 들어가서 .gitignore 파일에 내용 붙혀넣기
5. Git GUI에서
	git add.
	git commit -m "first commit"
	git config --global user.email "rhwogud6874@gmail.com"
	git config --global user.name "rhwogud6874"
	git add
	git commit -m "first commit"
6. 깃헙에서 우측상단 +New repository
	git remote add origin https://github.com/rhwogud6874/startcamp.git
7. git push origin master
