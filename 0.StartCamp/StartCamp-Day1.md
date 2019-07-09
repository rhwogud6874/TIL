# StartCamp-Day1
## 4차산업혁명에 대한 간단한 이야기


### 파이썬 언어 챗봇 제작하기 : https://gj.py.hphk.io/bots

1. 변수지정 `greeting = "안녕하세요"`
2. 북 지정 `A = {""="", ""="", ""=""}`
3. 리스트 지정 (C언어 array) `A = ["","",""]`

4. 언어출력 `print(" ")`
5. 변수출력 `print()`
6. 언어+변수값출력 `print(f"언어 {}")`
7. 북 출력`print(f "{}:{}")`

8. 외장함수 불러오기 `import A` ex) import random



### 랜덤 예시
```python
import random
numbers = list(range(0,46))

pick = random.sample(numbers, 6)
print(sorted(pick))
```
```python
import random

# 원하는 메뉴를 []안에 넣어주세요
menu = ["중식", "한식", "양식"]

choice = random.choice(menu)
print(f"오늘 점심은 {choice} 어떤가요?")
```

### for 문 예시
```python
greeting = "안녕하세요"

for i in range(5):
  print(i)
  print(greeting)
```

### if 문 예시
```python
if 조건
	print("") 
elif 조건
	print("")
else:
	print("")
```

### 웹크롤링 코드 예시
```python
import requests
from bs4 import BeautifulSoup

url = f'http://openapi.airkorea.or.kr/openapi/services/rest/ArpltnInforInqireSvc/getCtprvnRltmMesureDnsty?serviceKey={key}&numOfRows=10&pageSize=10&pageNo=1&startPage=1&sidoName=%EA%B4%91%EC%A3%BC&ver=1.6'
request = requests.get(url).text
soup = BeautifulSoup(request,'xml')
dong = soup('item')[7]
location = dong.stationName.text
time = dong.dataTime.text
dust = int(dong.pm10Value.text)

print(f"{time} 기준 {location}의 미세먼지 농도는 {dust}입니다.")
```