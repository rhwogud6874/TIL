# StartCamp-Day3

### 파일작성
```python
f = open("student",'w') 
## open=파일을 열기(더블클릭), w=작성쓰기(덮어쓰기), a=글자내용 추가할때, r=
f.write("안녕하세요") ## 파일 안에 내용 작성
f.close() ##파일 닫기
```
### CSV 파일 작성
```python

import csv  #csv 모듈 불러오기

lunch = {  # lunch는 딕셔너리
    "BBQ":"123123",
    "중국집":"789789",
    "한식":"456456"
}

with open("lunch.csv",'w', encoding="utf-8", newline="") as f:
# encoding="utf-8" : 한글 호환성을 위해 인코딩함
	csv_writer = csv.writer(f)

    for item in lunch.items():
        csv_writer.writerow(item)
```

### 크롤링 후 데이터 csv파일작성
```python
import csv
import requests
from bs4 import BeautifulSoup
url = "https://finance.naver.com/marketindex/exchangeList.nhn"
res = requests.get(url).text
soup = BeautifulSoup(res, "html.parser")

tr = soup.select('tbody>tr')
with open("naver_exchange.csv", "w", encoding="utf-8", newline="") as f:
    csv_writer = csv.writer(f)
    for r in tr:
        print(r.select_one('.tit').text.strip())
        print(r.select_one('.sale').text)
        row = [r.select_one('.tit').text.strip(), r.select_one('.sale').text]
        csv_writer.writerow(row)
```

```python
import requests
import csv
from bs4 import BeautifulSoup
url = "https://www.bithumb.com/"
res = requests.get(url).text
soup = BeautifulSoup(res, "html.parser")


    
tr = soup.select('tbody>tr')
with open("coin_exchange.csv", "w", encoding="utf-8", newline="") as f:
    csv_writer = csv.writer(f)
    for r in tr:
        print(r.select_one('.sort_coin').text.strip())
        print(r.select_one('.sort_real').text)
        row = [r.select_one('.sort_coin').text.strip(), r.select_one('.sort_real').text]
        csv_writer.writerow(row)
```

# HTML
```html
<h1></h1> <!--html의 시작-->
<h1 style="background-color:red;color:blue">HTML</h1>
<!-- style="bacground-color:red" 는 배경색상-->
<!-- style="color:blue" 는 글자색상-->

<h2>HyperText Markup language</h2> <!--만 작성 후 tab키-->
<a href="">네이버</a> 
<!--a(anker)만 작성 후 tab키, herf=""안에 넣을 내용은 원하는 주소.-->
<!-- <태그이름 속성명="속성값" 속성명2="속성값2">내용</태그이름> -->

<h3>우리가 공부한 것</h3>
<ol>
    <li><strong><i>파이썬</i></strong></li>
    <!-- 스트롱은 글자 진하게, i는 글자 흘리게 -->
    <li>HTML</li>
    <li>Git</li>
</ol>
<!-- ol:ordered list, 리스트 목록 -->
```

```html
<!DOCTYPE html>
<html>
    <head>
    
    </head>
    <body>
    
    </body>
</html>
<!-- html 기본 코드 -->
```


```html
<!DOCTYPE html>
<html>
    <head>
        <style>  <!-- 글 전체의 설정값 정하기 -->
            h1 {
                background-color:red;
            }
            a {
                color: brown
            }
            .blue {
                color: blue
            }
        </style>
    </head>



​```html
<!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="./intro.css">
        <!-- head 설정값을 다른 css에 저장해놓고 불러오기 -->
    </head>
    
    <body>
        <h1>HTML</h1>
        <h1 class="blue">CSS</h1>
        <h2>HyperText Markup Language</h2>
        <a href="https://naver.com">네이버</a>
        
        <!-- <태그이름 속성명="속성값" 속성명2="속성값2">내용</태그이름> -->
        
            <h3>우리가 공부한 것</h3>
        <ol>
            <li><strong><i>파이썬</i></strong></li>
            <li>HTML</li>
            <li class="blue">HTML</li>
            <li id="git" class="blue">Git</li>
        </ol>

    </body>
</html>
```