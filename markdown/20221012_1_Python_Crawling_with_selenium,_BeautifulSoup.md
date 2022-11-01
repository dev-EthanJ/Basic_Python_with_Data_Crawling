# Python Crawling 파이썬 크롤링 with selenium, BeautifulSoup

# 크롤링(Crawling)

-   웹에서 원하는 자료를 컴퓨터에게 수집해오도록 하는 기술
-   requests library를 활용한 브라우저 없는 crawling
-   urlib library를 활용한 브라우저 없는 crawling
-   **crawler**의 역할은 원하는 정보를 포함한 자료를 수집해 오는 것까지이며     
실제로 원하는 데이터를 용도에 맞게 처리하는 것은 **BeautifulSoup**가 담당한다

<br>

## selenium 설치

1.  anaconda navigator 좌측 environments 선택
2.  중간에 base(root) 우측 재생버튼 클릭 > open terminal 선택
3.  열리는 cmd창에서 "pip install selenium" 입력

```python
# 크롤링 작업을 위한 library impory

form bs4 impory BeautifulSoup
from selenium import webdriver
import requests
```

<br>

```python
# 코드 진행 지연을 위한 time import

import time
```

<br>

```python
# 2022-07 이후 selenium 업데이트로 인한 XPATH 추적시 사용하는 import

from selenium.webdriver.common.by import By
```

<br>

## chromedriver 다운받기

1.  크롬창 우측 상단 메뉴 클릭
2.  밑에서 두 번째에 있는 "도움말" 항목에 마우스 갖다대기
3.  Chrome정보 클릭하기
4.  Chrome정보에 나온 버전 확인하기
5.  [https://chromedriver.chromium.org/downloads](https://chromedriver.chromium.org/downloads)  
    위 주소로 접속해서 버전에 맞는 다운로드 링크로 가기
6.  chromedriver zip파일 받아서 unzip > "chromedriver.exe" cd /\[주피터노트북 코드 위치\]  
    (window > win32, 리눅스 맥 등은 맞는버전으로)

```python
# driver라는 변수를 이용해 물리 브라우저를 제어
driver = webdriver.Chrome('chromedriver')

# 딜레이 10초 삽입
# time.sleep(지연시간_sec) 입력시 해당 시간만큼 코드 실행 지연
time.sleep(10)
```

![1012_1_1](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_1.PNG?raw=true)

<br>

```python
# driver.get(url) > 브라우저가 url로 접속
driver.get("https://naver.com")

time.sleep(10)
```

![1012_1_2](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_2.PNG?raw=true)

<br>

```python
# naver.com 로그인 버튼의 xpath를 따오기
# > F12 > Select the element 버튼 클릭 > 영역 선택 > Copy > Copy XPATH

# driver.find_element_by_xpath(XPATH).click() > 현재 사용 불가능한 코드
driver.find_element(By.XPATH, '//*[@id="account"]/a').click() # > 현재 가능 코드

time.sleep(10)
```

![1012_1_3](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_3.PNG?raw=true)

<br>

```python
# 로그인 창 Username 입력 form Copy XPATH 
# > driver.find_element(By.XPATH, 'XPATH')

# string 입력 method > .send_keys("str")

# > username input
driver.find_element(By.XPATH, '//*[@id="id"]').send_keys('username') # > username

# > password input
driver.find_element(By.XPATH, '//*[@id="pw"]').send_keys('password') # > password

time.sleep(10)
```

![1012_1_4](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_4.PNG?raw=true)

<br>

```python
# > click [Sign in] == .send_keys('password\n')
driver.find_element(By.XPATH, '//*[@id="log.login"]').click() # > Click Sign in

# > captcha 활성화 > 수동 로그인
time.sleep(30)
```

![1012_1_5](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_5.PNG?raw=true)

<br>

```python
# > 카페로 접속
driver.get('https://cafe.naver.com/joonggonara')

time.sleep(10)
```

![1012_1_6](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_6.PNG?raw=true)

<br>

```python
# > 글쓰기 버튼 클릭
driver.find_element(By.XPATH, '//*[@id="cafe-info-data"]/div[4]/a').click()

time.sleep(10)
```

![1012_1_7](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_7.PNG?raw=true)

<br>

```python
# 브라우저를 다 쓰면 닫아놓아야 메모리가 절약된다

driver.close()
```

<br>

---

### 교보문고 사이트 접근 실습

1.  네이버 검색창에 "교보문고" 키워드로 검색
2.  검색 결과로 나온 창에서 "교보문고" 이동 링크 클릭

```python
driver = webdriver.Chrome('chromedriver')
time.sleep(10)

driver.get("https://naver.com")
time.sleep(10)


driver.find_element(By.XPATH, '//*[@id="query"]').send_keys("교보문고")
time.sleep(10)
```

![1012_1_8](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_8.PNG?raw=true)

<br>

```python
driver.find_element(By.XPATH, '//*[@id="search_btn"]').click()
time.sleep(10)
```

![1012_1_9](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_9.PNG?raw=true)

<br>

```python
driver.find_element(By.XPATH, '//*[@id="main_pack"]/section[1]/div/div/div[1]/div/div[1]/a/span[2]').click()
time.sleep(10)

driver.close()
```

![1012_1_10](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_10.PNG?raw=true)

<br>

---

## 특정 url로 접근했을 때 바로 원하는 정보를 얻을 수 있는 경우

-   driver.get("url") > 바로 접근 가능
-   그러나 페이스북 등 특정 조건 만족해야 자료 접근 가능한 경우
-   또한 로그인 해야만 자료에 접근할 수 있는 경우도 있다

> _어떻게 접근해야 원하는 자료를 얻어올 수 있는지는 신중하게 고려해야 함_

```python
chrome_driver = webdriver.Chrome()
time.sleep(10)

chrome_driver.get("https://www.kyobobook.co.kr/")

# 베스트셀러 웹페이지로 이동
chrome_driver.find_element(By.XPATH, '//*[@id="welcome_header_wrap"]/div[3]/nav/ul[1]/li[1]/a').click()
```

![1012_1_11](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1012_1_11.PNG?raw=true)

<br>

-   브라우저가 특정 페이지에 접근했을 때, 해당 페이지 소스코드 전체 긁어오기 > selenium의 역할
-   가져온 소스코드는 BeautifulSoup로 정제함
-   `driver.page_source` > 전체 페이지 소스를 string으로 return

```python
source = chrome_driver.page_source

time.sleep(10)
chrome_driver.close()

print(type(source)) # == str
print(source) # == 페이지 소스보기 (F12)
```

```text
<class 'str'>
<html lang="ko" data-view="ink" class="kbb_loaded"><head>
    <meta name="google-site-verification" content="2dlgBOp3K0s6wHjZo_Hkas6yaYPKZIVsmres9vC3F34">



<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=1">





    <meta name="title" content=" 일간 온라인 베스트셀러 | 국내도서 - 교보문고">
.
.
.
```

<br>

---

## BeautifulSoup

-   `BeautifulSoup([소스코드], "html.parser")` > string 소스코드를 html 형식으로 인식해서 반환  
    data type == <class 'bs4.BeasutifulSoup'>
-   **파싱(parsing)**: source code (string data) > html type으로 인식시키는 것
-   책을 소유하고 있다고 그 책에 대해 반드시 이해하고 있는 건 아니다,  
    책 내용을 읽고 지식을 구조화해야 책을 이해할 수 있다.
-   **BeautifulSoup**: source code가 단순 string일 때 > 기능 사용 불가능  
    but, source code parsing 실행하면 > 소스코드 구조 이해 가능 > data정제를 빠르고 쉽게 가능

```python
# parsing
html_parsed_source = BeautifulSoup(source, "html.parser")

print(type(html_parsed_source))
print(type(source))
```

```text
<class 'str'>
<class 'bs4.BeautifulSoup'>
```

<br>

### 책 제목만 가져오기

`[parsed source code].find_all("tag", class_="class" (, id="id"))`  
\> 해당 태그나 클래스에 대한 data만 담은 object return

```python
span_prod_name = html_parsed_source.find_all("span", class_="prod_name")

print(type(span_prod_name))
print(span_prod_name)
```

```text
<class 'bs4.element.ResultSet'>
[<span class="prod_name">부의 치트키</span>, <span class="prod_name">트렌드 코리아 2023</span>, <span class="prod_name">하얼빈</span>, <span class="prod_name">2022 하반기 에듀윌 취업 온라인 SKCT SK그룹 종합역량검사 통합 기본서</span>, <span class="prod_name">반짝이는 하루, 그게 오늘이야</span>, <span class="prod_name">나는 나를 바꾸기로 했다</span>, <span class="prod_name">아버지의 해방일지</span>, <span class="prod_name">잘될 수밖에 없는 너에게</span>, <span class="prod_name">역행자</span>, <span class="prod_name">불편한 편의점 2</span>, <span class="prod_name">마흔에 읽는 니체</span>, <span class="prod_name">세상에서 가장 쉬운 본질육아</span>, <span class="prod_name">2022 하반기 해커스 GSAT 삼성직무적성검사 실전모의고사 8회분(수리논리/추리)</span>, <span class="prod_name">단순한 열정</span>, <span class="prod_name">심리학이 분노에 답하다</span>, <span class="prod_name">너의 이름을 사랑이라 부른다</span>, <span class="prod_name">SQL 자격검정 실전문제</span>, <span class="prod_name">파친코 2</span>, <span class="prod_name">불편한 편의점(40만부 기념 벚꽃 에디션)</span>, <span class="prod_name">파친코 1</span>]
```

<br>

`[parsed source code].find,all()` > list와 유사한 object return  
\> indexing으로 item 지정해서 내부 data handling 가능

```python
# .find_all() return object + [indexing] > object item 하나(tag) 지정
print(span_prod_name[0])

# [parsed code].find_all() object > item 하나(tag)
# tag + ".text" > tag가 제거된 string return
print(span_prod_name[0].text)
```

```text
<span class="prod_name">부의 치트키</span>
부의 치트키
```

<br>

---

### \> 책 제목 출력

-   span\_prod\_name object 내부의 tag item들 > 반복문을 이용해서 전부 태그를 제거하고 출력

```python
for title in span_prod_name:
    print(title.text)
```

```text
부의 치트키
트렌드 코리아 2023
하얼빈
2022 하반기 에듀윌 취업 온라인 SKCT SK그룹 종합역량검사 통합 기본서
반짝이는 하루, 그게 오늘이야
나는 나를 바꾸기로 했다
아버지의 해방일지
잘될 수밖에 없는 너에게
역행자
불편한 편의점 2
마흔에 읽는 니체
세상에서 가장 쉬운 본질육아
2022 하반기 해커스 GSAT 삼성직무적성검사 실전모의고사 8회분(수리논리/추리)
단순한 열정
심리학이 분노에 답하다
너의 이름을 사랑이라 부른다
SQL 자격검정 실전문제
파친코 2
불편한 편의점(40만부 기념 벚꽃 에디션)
파친코 1
```

<br>

---

### \> 가격만 가져와서 출력

```python
span_val = html_parsed_source.find_all("span", class_="val")

for price in span_val:
    print(price.text)
```

```text
15,300
17,100
14,400
20,700
15,120
14,220
13,500
14,400
15,750
12,600
14,400
16,920
16,110
9,000
16,020
8,100
17,820
14,220
12,600
14,220
0
```

<br>

---

### \> 저자만 가져와서 출력

```python
span_prod_author= html_parsed_source.find_all("span", class_="prod_author")

for author in span_prod_author:
    # [string].split("spliter") 
    # > string을 "spliter" 기준으로 자르기 연산된 item들로 구성된 str list 반환 
    # > list indexing 가능
    print(author.text.split(" ·")[0])
```

```text
김성공
김난도 외
김훈
에듀윌 취업연구소
레슬리 마샹 외
우즈훙 외
정지아
최서영
자청
김호연
장재형
지나영
해커스 취업교육연구소
아니 에르노 외
충페이충 외
소강석
한국데이터진흥원
이민진 외
김호연
이민진 외
```

<br>

---

### \> 제목, 가격, 저자를 각각 담은 리스트 3개 출력

```python
title_list = list()
price_list = list()
author_list = list()

for title in span_prod_name:
    title_list.append(title.text)

for price in span_val:
    price_list.append(price.text)

for autho in span_prod_author:
    author_list.append(author.text.split(" ·")[0])

print(title_list)
print(price_list)
print(author_list)
```

```text
['부의 치트키', '트렌드 코리아 2023', '하얼빈', '2022 하반기 에듀윌 취업 온라인 SKCT SK그룹 종합역량검사 통합 기본서', '반짝이는 하루, 그게 오늘이야', '나는 나를 바꾸기로 했다', '아버지의 해방일지', '잘될 수밖에 없는 너에게', '역행자', '불편한 편의점 2', '마흔에 읽는 니체', '세상에서 가장 쉬운 본질육아', '2022 하반기 해커스 GSAT 삼성직무적성검사 실전모의고사 8회분(수리논리/추리)', '단순한 열정', '심리학이 분노에 답하다', '너의 이름을 사랑이라 부른다', 'SQL 자격검정 실전문제', '파친코 2', '불편한 편의점(40만부 기념 벚꽃 에디션)', '파친코 1']
['15,300', '17,100', '14,400', '20,700', '15,120', '14,220', '13,500', '14,400', '15,750', '12,600', '14,400', '16,920', '16,110', '9,000', '16,020', '8,100', '17,820', '14,220', '12,600', '14,220', '0']
['김성공', '김난도 외', '김훈', '에듀윌 취업연구소', '레슬리 마샹 외', '우즈훙 외', '정지아', '최서영', '자청', '김호연', '장재형', '지나영', '해커스 취업교육연구소', '아니 에르노 외', '충페이충 외', '소강석', '한국데이터진흥원', '이민진 외', '김호연', '이민진 외']
```

<br>
