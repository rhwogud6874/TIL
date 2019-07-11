### StartCamp-Day4

# ?
flask.pocco.org

Terminal 명령어
pip install flask 
pip list
FLASK_APP=hello.py flask run

/가 하나만 있다는것은 최상단을 의미한다.
html 작성 후 ! + [Tab]

FIRST APP 폴더 내에 app.py와 templates(폴더)
templates(폴더) 내에는 .html 파일들

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")                #127.0.0.1:5000/
def hello():
    return "Hello World!"

@app.route("/hi")             #127.0.0.1:5000/hi
def hi():
    return "안녕하세요"

@app.route("/html_tag")
def html_tag():
    return "<h1>안녕하세요</h1>"

@app.route("/html_tags")
def html_tags():
    return """               # """ : 여러줄 삽입
    <h1>안녕하세요</h1>
    <h2>반갑습니다</h2>
    ""
   
import datetime
@app.route("/dday")
def dday():                    
    today = datetime.datetime.now()
    endday = datetime.datetime(2019,11,29)
    d = endday-today
    return f"1학기 종료까지 {d.days}"
    
@app.route("/html_file")
def html_file():
    return render_template('index.html')
    
@app.route("/cube/<int:num>")
def cube(num):
    cube_num = num**3     # **은 ^를 의미한다.
    return f"{num}의 세제곱은 {cube_num}입니다."

@app.route("/cube_html/<int:num>")
def cube_html(num):
    cube_num = num**3
    return render_template("cube.html", num_html=num, cube_num_html=cube_num)

@app.route('/greeting_html/<string:name>')
def greeting_html(name):
    return render_template("greeting.html",name=name)

# @app.route("/lunch")
# def lunch():

import random
@app.route("/lunch")
def lunch():
    menu = { 
        "짜장면":"https://upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Jajangmyeon_by_stu_spivack.jpg/240px-Jajangmyeon_by_stu_spivack.jpg",
        "짬뽕":"http://recipe1.ezmember.co.kr/cache/recipe/2015/08/24/835a29917b3a9f074cb8d1636adeefcf1.jpg",
        "스파게티":"http://recipe1.ezmember.co.kr/cache/recipe/2018/06/13/003a9d244b3193f4037cccc4ef59e4221.jpg"
    }
    menu_list = list(menu.keys()) #["짜장면","짬뽕","스파게티"]
    pick = random.choice(menu_list)
    img = menu[pick]
    return render_template("lunch.html", pick=pick, img=img)

@app.route('/movies')
def movies():
    movie_list = ['스파이더맨','토이스토리','알라딘','존윅']
    return render_template("movies.html", movie_list=movie_list)

@app.route("/ping")
def ping():
    return render_template("ping.html")

@app.route("/pong")
def pong():
    user_input = request.args.get("test") #arguments, 매개변수
    return render_template("pong.html", user_input=user_input)

@app.route("/naver")
def naver():
    return render_template("naver.html")

@app.route("/google")
def google():
    return render_template("google.html")

@app.route("/text")
def text():
    return render_template("text.html")

import requests
@app.route("/result")
def result():
    raw_text = request.args.get('raw')
    url="http://artii.herokuapp.com/make?text="
    res = requests.get(url+raw_text).text
    return render_template("result.html", res=res)
    
@app.route('/lotto')
def lotto():
    return render_template("lotto.html")

@app.route('/lotto_result')
def lotto_result():
    #사용자가 입력한 정보 가져오기
    numbers = request.args.get('numbers').split()
    user_numbers = []
    for n in numbers:
        user_numbers.append(int(n))

    # user_numbers = [1,2,3,4,5,6]
    # 로또 홈페이지에서 정보를 가져오기
    url = "https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=866"
    res = requests.get(url)
    lotto_numbers = res.json()

    winning_numbers = []
    for i in range(1,7):
        winning_numbers.append(lotto_numbers[f'drwtNo{i}'])
    bonus_number = lotto_numbers['bnusNo']

    result = "1등"

    matched = len(set(user_numbers) & set(winning_numbers))
    if matched == 6:
        result = "1등"
    elif matched == 5: 
        if bonus_number in user_numbers: 
            result = "2등"
        else : 
            result = "3등"
    elif matched == 5:
        result = "3등"
    elif matched == 4:
        result = "4등"
    elif matched == 3:
        result = "5등"
    else:
        result = "꽝"

    return render_template("lotto_result.html", u=user_numbers, w=winning_numbers, b=bonus_number, r=result)





if __name__ == '__main__':
    app.run(debug=True)



```