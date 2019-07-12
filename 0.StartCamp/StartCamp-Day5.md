### StartCamp-Day5

# test.py
```python
import requests
from decouple import config
token = config("TELEGRAM_TOKEN") #Botfather, Newbot에서 얻음
url = f"https://api.telegram.org/bot{token}/" #주소는 변경없음
user_id = config("CHAT_ID")
# 변수가 있을 때는 f 붙여서 변수 인식 해주기
# url + token 마지막에 getUpdates 누르면 고유 "id"값 얻을 수 있음

# 토큰 숨기려면 pip install python-decouple(숨김문자필요할때 터미널에서 명령)
# .env 파일 생성(내용은 대문자와 _로 제작, 띄어쓰기없다)
# .gitignore 생성, 내용은 .env , gitignore사이트에서 파이썬,윈도우,Vcode

# ngrok http 5000 << 외부접속서버 생성
ngrok_url = "https://kjh6874.pythonanywhere.com/" #ngrok Forwarding값
webhook_url = f"{url}setWebhook?url={ngrok_url}/{token}" 
print(webhook_url)

# send_url = f"{url}sendMessage?chat_id={user_id}&text=하이하이"
# requests.get(send_url)
```

# app.py
```python
from flask import Flask, request, render_template  #구글에 flask 검색
from decouple import config
import random
import requests
from bs4 import BeautifulSoup
app = Flask(__name__)

api_url = "https://api.telegram.org"
token = config("TELEGRAM_TOKEN")
chat_id = config("CHAT_ID")
naver_id = config("NAVER_ID")
naver_secret = config("NAVER_SECRET")


#templates 폴더 생성, send와 wirte 제작

@app.route("/write")
def write():
    return render_template("write.html")

@app.route("/send")
def send():
    msg = request.args.get('msg')
    url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={msg}"
    res = requests.get(url)
    return render_template("send.html")

@app.route(f"/{token}", methods=['POST'])
def telegram():
    print(request.get_json())
    data = request.get_json()
    user_id = data.get('message').get('from').get('id')
    user_msg = data.get('message').get('text')

    if data.get('message').get('photo') is None:
        if user_msg == "점심메뉴":
            menu_list = ["삼계탕", "낚지볶음", "물냉면"]
            result = random.choice(menu_list)
        elif user_msg == "로또":
            numbers = list(range(1,46))
            lotto = random.sample(numbers, 6)
            result = sorted(lotto)
        elif user_msg[0:2] == "번역":
            # 번역 안녕하세요 저는 누구입니다.
            raw_text = user_msg[3:]
            papago_url = "https://openapi.naver.com/v1/papago/n2mt"
            data = {
                "source":"ko",
                "target":"en",
                "text":raw_text
            }
            header = {
                'X-Naver-Client-Id':naver_id,
                'X-Naver-Client-Secret':naver_secret
            }
            res = requests.post(papago_url,data=data, headers=header)
            translate_res = res.json()
            translate_result = translate_res.get('message').get('result').get('translatedText')
            result = translate_result
            pass
        elif user_msg == "코스피":
            url = "https://finance.naver.com/sise/"
            html = requests.get(url).text
            soup = BeautifulSoup(html, 'html.parser')
            select = soup.select_one('#KOSPI_now').text
            result = select
        elif user_msg == "환율":
            url = "https://finance.naver.com/marketindex/?tabSel=exchange#tab_section"
            html = requests.get(url).text
            soup = BeautifulSoup(html, 'html.parser')
            select = soup.select_one('#exchangeList > li.on > a.head.usd > div > span.value').text
            result = select

        else:
            result = user_msg
    else:
        # 사용자가 보낸 사진을 찾는 과정
        result="asdf"
        file_id = data.get('message').get('photo')[-1].get('file_id')
        file_url = f"{api_url}/bot{token}/getFile?file_id={file_id}"
        file_res = requests.get(file_url)
        file_path = file_res.json().get('result').get('file_path')
        file = f"{api_url}/file/bot{token}/{file_path}"

        #사용자가 보낸 사진을 클로버로 전송
        res = requests.get(file, stream=True)
        clova_url = "https://openapi.naver.com/v1/vision/celebrity"
        header = {
                'X-Naver-Client-Id':naver_id,
                'X-Naver-Client-Secret':naver_secret
            }

        clova_res = requests.post(clova_url, headers=header, files={'image':res.raw.read()})
        
        if clova_res.json().get('info').get("faceCount"):
            # 누구랑 닮았는지 출력
            celebrity = clova_res.json().get('faces')[0].get('celebrity')
            name = celebrity.get('value')
            confidence = celebrity.get('confidence')
            result = f"{name}일 확률이 {confidence}입니다."
        else:
            # 사람이 없음
            result = "사람이 없습니다."

    res_url = f"{api_url}/bot{token}/sendMessage?chat_id={user_id}&text={result}"
    requests.get(res_url)
    return '', 200


if __name__=="__main__":
    app.run(debug=True)
```

# gitignore
```python
.env

# Created by https://www.gitignore.io/api/python,windows,visualstudiocode
# Edit at https://www.gitignore.io/?templates=python,windows,visualstudiocode

### Python ###
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
pip-wheel-metadata/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
.hypothesis/
.pytest_cache/

# Translations
*.mo
*.pot

# Django stuff:
*.log
local_settings.py
db.sqlite3
db.sqlite3-journal

# Flask stuff:
instance/
.webassets-cache

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
target/

# Jupyter Notebook
.ipynb_checkpoints

# IPython
profile_default/
ipython_config.py

# pyenv
.python-version

# pipenv
#   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
#   However, in case of collaboration, if having platform-specific dependencies or dependencies
#   having no cross-platform support, pipenv may install dependencies that don't work, or not
#   install all needed dependencies.
#Pipfile.lock

# celery beat schedule file
celerybeat-schedule

# SageMath parsed files
*.sage.py

# Environments
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# Spyder project settings
.spyderproject
.spyproject

# Rope project settings
.ropeproject

# mkdocs documentation
/site

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# Pyre type checker
.pyre/

### VisualStudioCode ###
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json

### VisualStudioCode Patch ###
# Ignore all local history of files
.history

### Windows ###
# Windows thumbnail cache files
Thumbs.db
Thumbs.db:encryptable
ehthumbs.db
ehthumbs_vista.db

# Dump file
*.stackdump

# Folder config file
[Dd]esktop.ini

# Recycle Bin used on file shares
$RECYCLE.BIN/

# Windows Installer files
*.cab
*.msi
*.msix
*.msm
*.msp

# Windows shortcuts
*.lnk

# End of https://www.gitignore.io/api/python,windows,visualstudiocode
```

# .env
```python
TELEGRAM_TOKEN=''
CHAT_ID=''
NAVER_ID=''
NAVER_SECRET=''
```

# templates/write.html
``` python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="/send"> <!--templates 내의 받을 폴더-->
        <input type="text" name="msg">
        <input type="submit">
    </form>
    
</body>
</html>
```

# templates/send.html
```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>성공적으로 메세지가 전달되었습니다</h1>
</body>
</html>
```