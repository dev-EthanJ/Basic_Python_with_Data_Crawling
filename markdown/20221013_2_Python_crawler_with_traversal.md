
## Python crawler with traversal 파이썬 순회 크롤러

- 같은 양식의 페이지를 순회하면서 자료를 수집해오는 크롤러

- 원 페이지 크롤러 제작 후 > 완성된 크롤러를 반복문에 넣어서 만든다

> 반복을 어디부터 돌릴지에 대한 파악이 제일 중요!

```python
# crwaling library import
from bs4 import BeautifulSoup
from selenium import webdriver
import requests

# 코드 진행 지연을 위한 time 임포트
import time

# 2022-07 이후 selenium 업데이트로 인한 XPATH 추적 시 사용하는 임포트
from selenium.webdriver.common.by import By

# file io
import codecs
```

<br>

- 순서

1. approach N page

2. source code crawling

3. parsing

4. data extraction

5. saving in txt file

6. move to number 1.

> 다음페이지 버튼 XPATH 클릭으로 페이지 넘기기

<br>

- 리스트 형식 페이지: \[F12\] + \[Network menu click\] > 리스트 다음 페이지 클릭    
\>  url 바뀌지 않아도, Network 변경사항을 \[Headers\], \[Payload\] tab에서 확인 가능     
\> XPATH 구하기 가능!

```python
chrome_driver = webdriver.Chrome('chromedriver')

# approach first page
chrome_driver.get("https://product.kyobobook.co.kr/bestseller/online?period=001")

# 첫번째 제목 저장 리스트 > 반복문 중지 조건으로 필요
check_name_list = list()

rank_list = list()
title_list = list()
price_list = list()
author_list = list()

time.sleep(6)

# 반복문
while True:
    
    # 끝까지 스크롤 다운 (광고로 페이지 가리기 방지)
    chrome_driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    
    # source code crawling
    source = chrome_driver.page_source
    
    # parsing
    html_parsed_source = BeautifulSoup(source, "html.parser")

    # extract data(span_prod_name) & saving in list
    
    ####### Title
    span_prod_name = html_parsed_source.find_all("span", class_="prod_name")   
    
    # while문 중지 조건 -> 같은 title이 list에 존재 할 때
    if (span_prod_name[0].text in check_name_list):
        chrome_driver.close()
        break
    check_name_list.append(span_prod_name[0].text)
    
    for title in span_prod_name:
        title_list.append(title.text)     
    
    ######## Rank
    div_prod_rank = html_parsed_source.find_all("div", class_="prod_rank")

    for rank in div_prod_rank:
        rank_list.append(rank.text)

    ######## Price        
    span_val = html_parsed_source.find_all("span", class_="val")
    
    for price in span_val:
        if(price.text == "0"):
            None
        else:
            price_list.append(price.text)
    
    ######### Author
    span_prod_author = html_parsed_source.find_all("span", class_="prod_author")

    for author in span_prod_author:
        author_list.append(author.text.split(" ·")[0])
    
    # 다음 페이지 버튼 XPATH로 이동
    chrome_driver.find_element(By.XPATH, '//*[@id="tabRoot"]/div[4]/div[2]/button[2]').click()
    
    time.sleep(6)

# extracted data item 개수 일치하는 지 확인
book_list = [ title_list, rank_list, price_list, author_list ]

for book in book_list:
    print(len(book))
```

```text
916
916
916
916
```

<br>

```python
# csv로 출력
w_csv = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/crawling/crawler_with_traversal.csv", 'w', "utf-8-sig")

for i in range(len(title_list)):
    this_line = "%s,%s,%s,%s\n" %(rank_list[i].replace(',', '，'), title_list[i].replace(',', '，'),
                                  author_list[i].replace(',', '，'), price_list[i].replace(',', '，'))
    w_csv.write(this_line)

w_csv.close()
```

![1013_2_1](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1013_2_1.PNG?raw=true)


![1013_2_2](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1013_2_2.PNG?raw=true)

