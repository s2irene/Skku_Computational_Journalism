# Skku_Computational_Journalism

# 특정 패키지가 있는지 확인하고, 없으면 설치하는 명령문 (실행하면 자동으로 패키지 설치, 한번만 실행하면 된다)

!pip show konlpy || pip install konlpy
!pip show requests || pip install requests
!pip show beautifulsoup4 || pip install beautifulsoup4

# 간단히 예제 사이트 하나를 탐색해 보자

import requests

url = "https://example.com"

# 요청 보내기
res = requests.get(url)

# 만약, 웹사이트가 특정 추가정보(브라우저 정보, 쿠키 등)를 요구한다면 다음과 같이 추가할 수 있다
# res = requests.get(url, data=..., headers=...)

# 요청이 제대로 도착/반환됐나 확인(200일 경우 제대로 된 것)
print(res.status_code)

# 반환된 내용 출력

#print(res.content) # 답변이 멀티미디어 (오디오, 음악, 이미지 등)를 포함할 경우
print(res.text) # html문서 내용만 확인할 경우

import pandas as pd

url = "https://ko.wikipedia.org/wiki/%EB%8C%80%ED%95%9C%EB%AF%BC%EA%B5%AD%EC%9D%98_%EC%98%81%ED%99%94_%ED%9D%A5%ED%96%89_%EA%B8%B0%EB%A1%9D"
df = pd.read_html(url)

# 전체 몇 개의 Table이 List에 들어갔는지 확인해 보자
print(len(df))

# 두번째 테이블이 아마도 우리가 원하는 내용일 것이다
df[1]

# Table을 사용하는 여러 사이트를 탐색해 보자

url = "https://finance.naver.com/sise/sise_market_sum.nhn?sosok=0"

# 아래 #표시를 지우고 코드를 완성 & 전처리를 수행해 보자

#
#
#
#
#

from bs4 import BeautifulSoup
import requests

url = "https://news.daum.net/"


# 웹사이트 정보 받아오기
res = requests.get(url)

# 요청이 제대로 도착/반환됐나 확인(200일 경우 제대로 된 것)
print(res.status_code) #


# 반환된 내용 출력

# print(res.content) # 답변이 멀티미디어 (오디오, 음악, 이미지 등)를 포함할 경우
# print(res.text) # html문서 내용만 확인할 경우

soup = BeautifulSoup(res.content) # 결과를 BeautifulSoup 형식으로 변환한다

# 웹사이트 검사기로 찾은 태그와 특징을 입력하면 모든 값을 찾아준다
# find_all(태그, 태그특징)

soup_filter = soup.find_all("div",class_="cont_thumb")

len(soup_filter) # 몇 개의 객체가 찾아졌는지 확인

for line in soup_filter:
  print(line.text) # 찾아진 객체에서 텍스트만 추출
  print("-"*30) # 구분점을 두고 확인해 보자

  for line in soup_filter:
  for line in soup_filter:

    # 텍스트 말고 다른 요소를 추출할 때는 Dictionary처럼 조회
    print(line.find("img",class_="thumb_g")['alt'],end="\t")

    # Text 추출 결과는 문자열 -> lower()나 strip() 적용 가능
    print(line.find("a").text.strip()) # Strip = 앞뒤 공백제거

    # 데이터 추출 과정 or 탐색 과정에서 오류가 나도 그대로 진행하게 하려면 if대신 try, except를 쓰자
# try, except를 쓰면 오류가 나더라도 except 파트를 실행하고 반복이 계속된다

for line in soup_filter:
  try:
    print(line.find("img",class_="thumb_g")['alt'],end="\t")
    print(line.find("a").text.strip())
  except:
    print("없음",end="\t")
    print(line.find("a").text.strip())

    from google.colab import userdata
accessKey = userdata.get('api_key')

######### 패키지 정리
import requests
import json
from pprint import pprint # 출력을 깔끔하게 정리해주는 패키지

######### 1. 보낼 정보를 정리한다

# 1.1. 모델을 선택한다 - 구어 (입말) or 문어
openApiURL = "http://aiopen.etri.re.kr:8000/WiseNLU_spoken" # 구어
# openApiURL = "http://aiopen.etri.re.kr:8000/WiseNLU" # 문어

# 1.2. API 키를 입력한다
# accessKey = "" # Colab을 사용하지 않을 경우 직접 넣어주거나, 환경변수를 설정한다

# 1.3. 분석 내용을 정리한다 (형태소 분석기능)
analysisCode = "morp"

# 1.4. 분석할 텍스트를 설정한다
text = "아니 여기서 끊는게 어딨어!!!!! 와 진짜!! 아우 이 여우들 얘네 뭐야뭐야 김수현 눈빛 진짜 미쳤나봐 끄악 어머어머 둘이 진짜 능글거리는거봐ㅋㅋㅋㅋㅋㅋㅋㅋ "

######### 2. 입력 정보를 통합한다

# 2.1. 요청자 정보 (요청 세부내역, API 키)
headers = {
    "Content-Type": "application/json; charset=UTF-8",
    "Authorization": accessKey
}

# 2.2. 요청내용 (본문내용, 분석 유형)
requestJson = {
    "argument": {
        "text": text,
        "analysis_code": analysisCode
    }
}

######### 3. 요청 정보를 전송하고 값을 받아온다
# 내 정보를 전달하고, 결과를 반환해서 저장한다
res = requests.post(
    openApiURL,
    headers=headers, # 요청자 정보
    json=requestJson # 요청 내용
)

######### 4. 전송 결과 확인 - 결과가 잘 나왔는지 확인하자 (200번 = 별 문제 없음)
print("[responseCode] " + str(response.status_code))

# 결과값은 Json이기 때문에 json형태로 변환해서 확인하자
import json # json 해석 도구 로드

res_to_text = res.text # 결과를 text로 변환한다
res_json = json.loads(res_to_text) # 변환된 결과를 json으로 다시 불러온다

# res_json = json.loads(res.text) # 이렇게 줄이면 편하다


# print대신 pprint를 쓰면 결과가 깔끔하게 정리된다
pprint(res_json)

# 결과값 -> 문장 -> 문장분석 중 첫번째 문장 -> 결과 확인 (+ 디테일한 형태소 분석)
# 아래 코드에서 0은 첫번째 문장을 의미한다

for line in res_json['return_object']['sentence'][0]['morp_eval']:
    # 결과만 보기
    print(line['result'],end="\t\t")

    # 결과 + 해석
    print(line)
