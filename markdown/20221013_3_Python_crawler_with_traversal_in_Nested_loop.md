# Python crawler with taversal in Nested loop

# 이중 반복문을 활용한 파이썬 순회 크롤러

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

## 1. `bs4.element` 내부 item 접근

```python
my_driver = webdriver.Chrome()

# 알라딘 베스트셀러 사이트 접속
my_driver.get("https://www.aladin.co.kr/shop/common/wbest.aspx?BranchType=1&start=we")

# ip block 방지
time.sleep(10)

# 소스 얻기
my_source = my_driver.page_source

# parsing(page 전체)
my_parsed_source = BeautifulSoup(my_source, "html.parser")

my_driver.close()
```

<br>

### 1.1. web page element 조사 결과 > `"li"` tag에 책 정보(가격) 존재 > extract data by `li` tag

```python
li_tag_data = my_parsed_source.find_all("li")

print(li_tag_data)
```

```text
[<li><a href="https://www.aladin.co.kr/home/welcome.aspx" title="HOME"><img alt="HOME" src="//image.aladin.co.kr/img/header/2011/global/global_set1_m01.gif"/></a></li>, <li class="set2se"></li>, <li id="#head_book_layer"><a href="https://www.aladin.co.kr/home/wbookmain.aspx" title="국내도서"><img alt="국내도서" src="//image.aladin.co.kr/img/header/2011/global/global_set2_m01_on.gif"/></a>
<div style="position:relative;z-index:99999;">
<div class="hdr" id="head_book_layer" style="width:836px;">
<div style="position:relative;float:left">
<table border="0" cellpadding="5" cellspacing="4" style="margin:5px 0 0 10px">
<tbody><tr>
<td valign="top" width="114"> <table>
<tbody><tr>
<td><a class="gr03" href="https://www.aladin.co.kr/shop/wbrowse.aspx?CID=13789">유아</a></td>
</tr>
<tr>
<td><a class="gr03" href="https://www.aladin.co.kr/shop/wbrowse.aspx?CID=1108"><strong>어린이</strong></a></td>
.
.
.
li>, <li>대표이사 : 최우경</li>, <li>고객정보보호 책임자 : 최우경</li>, <li>사업자등록 : <a class="footer_blue" href="javascript:fn_ftc_check();">201-81-23094</a></li>, <li>E-mail : privacy@aladin.co.kr</li>, <li>통신판매업신고 : 중구01520호</li>, <li style="clear:left; width:100%">호스팅 제공자 : 알라딘커뮤니케이션</li>, <li style="clear:left; width:100%">(본사) 서울시 중구 서소문로 89-31 <!--a href="http://www.aladin.co.kr/aladdin/waladdin.aspx?pn=contactus" class="footer_blue">약도</a--> ㅣ (중고매장) <a class="footer_blue" href="https://www.aladin.co.kr/usedstore/wgate.aspx">자세히보기</a></li>, <li style="clear:left; width:100%">(고객센터) 서울시 마포구 백범로 71 숨도빌딩 7층, Fax 02-6926-2600</li>]
```

- 정확한 데이터 추출 실패

<br>

### 1.2. 책 정보를 감싸고 있는 `"div"` tag and `"ss_book_box"` class로 data extraction

```python
div_book_box = my_parsed_source.find_all("div", class_="ss_book_box")

print(div_book_box)
print(len(div_book_box))
print(type(div_book_box))
```

```text
[<div class="ss_book_box" itemid="301692224">
<table border="0" cellpadding="0" cellspacing="0" width="100%">
<tbody><tr>
<td align="left" style="padding-right:5px;" valign="top" width="25">
<table border="0" cellpadding="0" cellspacing="0" width="18">
<tbody><tr><td><div style="text-align: center;">1.</div></td></tr> <tr><td><div style="text-align: center;"><input name="chkCart.8959897094" type="checkbox"/></div></td></tr> </tbody></table>
</td>
<td align="left" valign="top" width="170">
<table border="0" cellpadding="0" cellspacing="0" width="150">
<tbody><tr><td style=""><div style="padding: 0px 0px 5px 0px; text-align: center;"><table align="center" border="0" cellpadding="0" cellspacing="0" style="margin-top:5px;" width="100%"><tbody><tr><td style="font-size:11px;color:#000000;padding:2px 2px 0 2px;line-height:14px !important;text-align:center">
<span style="font-size:11px;color:#000000;padding:0 2px 0 2px">종합</span>Top10<font color="#fb6788" style="margin-left:3px;"><strong>2주</strong></font>
.
.
.
class="mylist Search3_Result_SafeBasketLayer" isbn="K762837520" style="display: none;"><li><a href="javascript:void(0);" onclick="return AddSafeBasket('K762837520');">보관함</a></li><li><a href="javascript:void(0);" onclick="return AddMyList('K762837520');">마이리스트</a></li> </ul>
</div></div>
</td>
</tr>
<tr>
<td colspan="2">
</td>
</tr>
</tbody></table>
</td>
</tr>
</tbody></table>
</div>]
50
<class 'bs4.element.ResultSet'
```

- `div_book_box` data == `bs4.element.ResultSet` object > `find_all()` method 없음

<br>

### 1.3. 내부 item 접근 방법

1. indexing으로 `bs4.element.ResultSet` 접근
2. `bs4.element.ResultSet[i]` > `find_all()` method > tag, class 기준으로 data 걸러내기
3. 걸러 낸 data > for loop 돌면서 item을 str로 형변환: `item.text`

<br>

1. indexing으로 접근

```python
print(div_book_box[0])

print(type(div_book_box[0]))
```

```text
<div class="ss_book_box" itemid="301692224">
<table border="0" cellpadding="0" cellspacing="0" width="100%">
<tbody><tr>
<td align="left" style="padding-right:5px;" valign="top" width="25">
<table border="0" cellpadding="0" cellspacing="0" width="18">
<tbody><tr><td><div style="text-align: center;">1.</div></td></tr> <tr><td><div style="text-align: center;"><input name="chkCart.8959897094" type="checkbox"/></div></td></tr> </tbody></table>
</td>
<td align="left" valign="top" width="170">
<table border="0" cellpadding="0" cellspacing="0" width="150">
<tbody><tr><td style=""><div style="padding: 0px 0px 5px 0px; text-align: center;"><table align="center" border="0" cellpadding="0" cellspacing="0" style="margin-top:5px;" width="100%"><tbody><tr><td style="font-size:11px;color:#000000;padding:2px 2px 0 2px;line-height:14px !important;text-align:center">
<span style="font-size:11px;color:#000000;padding:0 2px 0 2px">종합</span>Top10<font color="#fb6788" style="margin-left:3px;"><strong>2주</strong></font>
</td></tr></tbody></table></div><div class="cover_area"><a href="https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=301692224"><div class="flipcover_out" style="height:222px;"><div class="flipcover_in"><img alt="" class="left_cover" src="https://image.aladin.co.kr/product/30169/22/SpineShelf/8959897094_d.jpg" style="height: 222px; left: -10px; transform: rotateY(-90deg) translate3d(-8px, 0px, 0px);"/><img class="front_cover" src="https://image.aladin.co.kr/product/30169/22/cover200/8959897094_3.jpg" style="height: 222px;"/></div></div></a></div></td></tr>
<tr><td><div class="preview_area"><a href="https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=301692224" target="_blank"><img border="0" src="//image.aladin.co.kr/img/search/icon_new2.gif"/></a><a href="javascript:fn_PopUpPriview('/shop/book/wletslookViewer.aspx?ISBN=8959897094')"><img border="0" src="//image.aladin.co.kr/img/search/icon_pv2.gif"/></a></div></td></tr> </tbody></table>
</td>
<td align="left" valign="top" width="*">
<table border="0" cellpadding="0" cellspacing="0" width="100%">
<tbody><tr>
<td valign="top" width="*">
<div class="ss_book_list"><ul>
<li><span class="ss_ht1">[<a href="/events/wevent_redirect.aspx?eventid=240707">토끼 골드머그, 스마트톡(이벤트 대상 도서 포함 국내도서 2만 원 이상)</a>]</span><br/></li><li><span style="font-size: 14px;">[국내도서]</span> <a class="bo3" href="https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=301692224"><b>트렌드 코리아 2023</b></a> </li>
<li><a href="/Search/wSearchResult.aspx?AuthorSearch=김난도@120209&amp;BranchType=1">김난도</a>, <a href="/Search/wSearchResult.aspx?AuthorSearch=전미영@1602425&amp;BranchType=1">전미영</a>, <a href="/Search/wSearchResult.aspx?AuthorSearch=최지혜@2797568&amp;BranchType=1">최지혜</a>, <a href="/Search/wSearchResult.aspx?AuthorSearch=이수진@5816869&amp;BranchType=1">이수진</a>, <a href="/Search/wSearchResult.aspx?AuthorSearch=권정윤@6425552&amp;BranchType=1">권정윤</a>, <a href="/Search/wSearchResult.aspx?AuthorSearch=이준영@1602424&amp;BranchType=1">이준영</a>, <a href="/Search/wSearchResult.aspx?AuthorSearch=이향은@2308579&amp;BranchType=1">이향은</a>, <a href="/Search/wSearchResult.aspx?AuthorSearch=한다혜@8066461&amp;BranchType=1">한다혜</a>, <a href="/Search/wSearchResult.aspx?AuthorSearch=이혜원@8641847&amp;BranchType=1">이혜원</a>, <a href="/Search/wSearchResult.aspx?AuthorSearch=추예린@8775386&amp;BranchType=1">추예린</a> (지은이) | <a href="/search/wsearchresult.aspx?PublisherSearch=%eb%af%b8%eb%9e%98%ec%9d%98%ec%b0%bd@8609&amp;BranchType=1">미래의창</a> | 2022년 10월</li><li><span class="">19,000</span>원 → <span class="ss_p2"><b><span class="">17,100</span>원</b></span> (<span class="ss_p">10%</span>할인),  마일리지 <span class="ss_p">950</span>원 (<span class="ss_p">5%</span> 적립)</li><li><img border="0" src="//image.aladin.co.kr/img/common/star_s7.gif" style="vertical-align: middle;"/> (<a href="https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=301692224#8959897094_CommentReview">12</a>) | 세일즈포인트 :<b> 155,300</b></li> </ul></div>
<div class="ss_book_list"><ul>
<li><a href="/events/wevent.aspx?EventId=211729"><div class="b_ytz_delivery">양탄자배송</div></a> <span class="ss_p">내일 아침 7시 <strong>출근전 배송</strong></span><div>(중구 서소문로 89-31) <img alt="지역변경" onclick="FindZipByList('addInputShop_301692224');" src="//image.aladin.co.kr/img/shop/2012/bu_driveaway_ch.gif" style="cursor:pointer;vertical-align:middle;margin:-3px 0 0 0px;"/><span id="addInputShop_301692224"></span></div></li><li><img src="//image.aladin.co.kr/img/search/icon_arrow.jpg"/>이 책의 전자책 :  13,300원 
                                                <a href="https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=302693935" title="전자책 보기"><img alt="전자책 보기" src="//image.aladin.co.kr/img/shop/btn_ebookview.gif" style="vertical-align:top;"/></a></li></ul></div>
</td>
<td valign="top" width="80">
<div class="book_Rfloat_02"><div class="button_search_cart_new"><a href="/shop/wbasket.aspx?AddBook=8959897094" onclick="return SafeBasket_ListOneAddByAjax('8959897094', document.getElementById('divBasketAddResult_8959897094'), {top: 0, left: -55});">장바구니</a></div><div id="divBasketAddResult_8959897094"></div><div class="button_search_buyitnow_new"><a href="https://www.aladin.co.kr/order/worder_chk_order.aspx?CartType=4&amp;ISBN=8959897094" onclick="return QuickBuyCheck('8959897094');">바로구매</a></div><div class="Search3_Result_SafeBasketArea" isbn="8959897094" style="position: relative;"><div class="button_search_storage"><a href="javascript:void(0);">보관함 <img alt="" src="//image.aladin.co.kr/img/search/btn_bg5_arrow.png"/></a></div> <ul class="mylist Search3_Result_SafeBasketLayer" isbn="8959897094" style="display: none;"><li><a href="javascript:void(0);" onclick="return AddSafeBasket('8959897094');">보관함</a></li><li><a href="javascript:void(0);" onclick="return AddMyList('8959897094');">마이리스트</a></li> </ul>
</div></div>
</td>
</tr>
<tr>
<td colspan="2">
</td>
</tr>
</tbody></table>
</td>
</tr>
</tbody></table>
</div>
<class 'bs4.element.Tag'>
```

- `bs4.element.Tag[0]`: 1위 책에 대한 정보 담겨있음

- type == `bs4.element.Tag` > `find_all()` method 사용 가능

<br>

2. `find_all()` method > "li" tag로 data 걸러내기

3. extracted data `item` > for loop `item.text` > string으로 형 변환

```python
first_li_book = div_book_box[0].find_all("li")

for item in first_li_book:
    print(item.text)
```

```text
[토끼 골드머그, 스마트톡(이벤트 대상 도서 포함 국내도서 2만 원 이상)]
[국내도서] 트렌드 코리아 2023 
김난도, 전미영, 최지혜, 이수진, 권정윤, 이준영, 이향은, 한다혜, 이혜원, 추예린 (지은이) | 미래의창 | 2022년 10월
19,000원 → 17,100원 (10%할인),  마일리지 950원 (5% 적립)
 (12) | 세일즈포인트 : 155,300
양탄자배송 밤 11시 잠들기전 배송(중구 서소문로 89-31) 
이 책의 전자책 :  13,300원 
                                                
보관함
마이리스트
```

<br>

- `first_li_book[indexing].text` > index 1 == title, index 2 == author, index 3 == price 추측됨 > `print()`해서 확인 
```python
print(first_li_book[1].text)
print(first_li_book[2].text)
print(first_li_book[3].text)
```

```text
[국내도서] 트렌드 코리아 2023 
김난도, 전미영, 최지혜, 이수진, 권정윤, 이준영, 이향은, 한다혜, 이혜원, 추예린 (지은이) | 미래의창 | 2022년 10월
19,000원 → 17,100원 (10%할인),  마일리지 950원 (5% 적립)
```

<br>

- `div_book_box[index N == N+1순위 책].find_all("li")` > for loop `bs4.element.Tag` \[index 1 ~ 3\]:  item.text > 책 정보 출력 가능으로 추측됨    
\>index N에 다른 숫자 넣어서 확인

```python
for i in range(10, 14): # N = 10 ~ 13 > 10위 ~ 13위 책
    this_li_book = div_book_box[i].find_all("li")
    
    for j in range(1, 4): #index 1, 2, 3
        print(this_li_book[j].text)
        
    print()
```

```text
[국내도서] 총 균 쇠 (리커버 특별판) 
재레드 다이아몬드 (지은이), 김진준 (옮긴이) | 문학사상 | 2022년 10월
28,000원 → 25,200원 (10%할인),  마일리지 1,400원 (5% 적립)

[국내도서] 올리버쌤의 미국식 아이 영어 습관 365 
올리버 샨 그랜트 (지은이), 정다운 (그림) | 브라이트(다산북스) | 2022년 10월
23,000원 → 20,700원 (10%할인),  마일리지 1,150원 (5% 적립)

[국내도서] 가녀장의 시대  
이슬아 (지은이) | 이야기장수 | 2022년 10월
15,000원 → 13,500원 (10%할인),  마일리지 750원 (5% 적립)

[국내도서] 불편한 편의점 (40만부 기념 벚꽃 에디션) 
김호연 (지은이) | 나무옆의자 | 2021년 4월
14,000원 → 12,600원 (10%할인),  마일리지 700원 (5% 적립)
```

- **규칙성 확인 완료** > _data crawler_ 사용 가능!

<br>

- - -

## 2. parsed_source를 for loop로 한 권씩 책 정보 출력

- text.split() > 원하는 정보 골라서 출력 가능

<br>

### 2.1. 한 webpage의 parsed_source 

- source = `parsed_source.find_all()` > `for loop` source\[indexing\] (== item),   
`item.find_all()` > 원하는 data indexing

```python
div_book_box = my_parsed_source.find_all("div", class_="ss_book_box")

for this_div_book in div_book_box:
    this_li_book = this_div_book.find_all("li")
    
    this_title = this_li_book[1].text.split("]")[1]
    this_author = this_li_book[2].text.split(" (")[0]
    this_price = this_li_book[3].text.split("원 →")[0].replace(",", "，")
    
    print(this_title, this_author, this_price)
```

```text
1
 트렌드 코리아 2023  김난도, 전미영, 최지혜, 이수진, 권정윤, 이준영, 이향은, 한다혜, 이혜원, 추예린 19，000
2
 아버지의 해방일지   정지아 15，000
3
 이토록 평범한 미래   김연수 14，000
4
 단순한 열정 (무선) ㅣ 문학동네 세계문학전집 99  아니 에르노 10，000
5
 세상에서 가장 쉬운 본질육아  지나영 18，800
6
 헤어질 결심 스토리보드북   이윤호, 박찬욱 33，000
7
 불편한 편의점 2  김호연 14，000
8
 역행자  자청 17，500
9
 물고기는 존재하지 않는다 (리커버 특별판, 양장)  룰루 밀러 17，000
10
 하얼빈   김훈 16，000
11
 총 균 쇠 (리커버 특별판)  재레드 다이아몬드 28，000
12
 올리버쌤의 미국식 아이 영어 습관 365  올리버 샨 그랜트 23，000
13
 가녀장의 시대   이슬아 15，000
14
 불편한 편의점 (40만부 기념 벚꽃 에디션)  김호연 14，000
15
 작은 땅의 야수들  김주혜 18，000
16
 세월 ㅣ 아니 에르노 컬렉션   아니 에르노 15，500
17
 아름다운 초저녁달 4  야마모리 미카 6，000
18
 파친코 1  이민진 15，800
19
 흔한남매의 흔한 호기심 7 ㅣ 흔한남매   안치현 14，000
20
 파친코 2  이민진 15，800
21
 나는 오래된 거리처럼 너를 사랑하고 ㅣ 문학과지성 시인선 572   진은영 12，000
22
 2023 써니 행정법총론 기출문제집 - 전2권  박준철 42，000
23
 쇼타 형아 2  나카야마 미유키 6，500
24
 이 책은 돈 버는 법에 관한 이야기  고명환 16，800
25
 사람을 얻는 지혜  발타자르 그라시안 13，800
26
 마지막 이야기 전달자   도나 바르바 이게라 18，000
27
 잘될 수밖에 없는 너에게  최서영 16，000
28
 2022 김승옥문학상 수상작품집   편혜영, 김연수, 김애란, 정한아, 문지혁, 백수린 10，000
29
 그 길로 갈 바엔 ㅣ 젊은 만화가 테마단편집 2   재활용, 약국, 서글, 각종모에화, 하양지 15，000
30
 그리고 행복하다는 소식을 들었습니다  이병률 15，800
31
 마음세탁소  황웅근 15，000
32
 부자 아빠 가난한 아빠 1 (20주년 특별 기념판)   로버트 기요사키 15，800
33
 ETS 토익 정기시험 기출문제집 1000 Vol. 3 Reading (리딩) ㅣ ETS 토익 정기시험 기출문제집   ETS 17，800
34
 2022 큰별쌤 최태성의 별★별한국사 기출 500제 한국사능력검정시험 심화 (1.2.3급) ㅣ 큰별쌤 최태성의 별★별한국사 한국사능력검정시험 시리즈   최태성 19，500
35
 빠르게 실패하기  존 크럼볼츠, 라이언 바비노 16，500
36
 헤어질 결심 각본  박찬욱, 정서경 15，000
37
 ETS 토익 정기시험 기출문제집 1000 Vol. 3 Listening (리스닝) ㅣ ETS 토익 정기시험 기출문제집   ETS 17，800
38
 도쿄 에일리언즈 4 (초판 한정 홀로그램 양면 일러스트 카드 + 선착순 한정 아크릴 일러스트 카드 포함 특장판)  NAOE 5，000
39
 원씽 The One Thing (리커버 특별판)   게리 켈러, 제이 파파산 14，000
40
 10배의 법칙   그랜트 카돈 16，800
41
 빈 옷장 ㅣ 아니 에르노 컬렉션   아니 에르노 14，500
42
 퀀트 투자 무작정 따라하기 ㅣ 무작정 따라하기 경제경영/재테크   강환국 25，000
43
 책과 우연들  김초엽 16，000
44
 마흔에 읽는 니체  장재형 16，000
45
 과학이 필요한 시간  궤도 16，000
46
 해커스 토익 기출 보카 TOEIC VOCA 단어장  David Cho 12，900
47
 길티 이노센스 5  윤한 6，000
48
 그릿 GRIT (100쇄 기념 리커버 에디션)   앤절라 더크워스 16，000
49
 사랑은 그렇게 하는 것이 아니다  김달 16，500
50
```

```python
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
Input In [46], in <cell line: 4>()
      5 this_li_book = this_div_book.find_all("li")
      7 print(i)
----> 9 this_title = this_li_book[1].text.split("]")[1]
     10 this_author = this_li_book[2].text.split(" (")[0]
     11 this_price = this_li_book[3].text.split("원 →")[0].replace(",", "，")

IndexError: list index out of range
```

- `IndexError: list index out of range` 원인

\> `[50번].find_all('li')[1]` != title, `50번.find_all('li')[0]` == title

\> 다른 책에는 index 0에 증정품 item 있음

\> 50번 째 책에는 증정품 없이 바로 title item > index 0에 title item 존재

<br>

### 2.2. indexing 결과와 원하는 item이 다를 때 처리 방법

```python
# 구글웹드라이버 사용
my_driver = webdriver.Chrome()

# 알라딘 베스트셀러 사이트 접속
my_driver.get("https://www.aladin.co.kr/shop/common/wbest.aspx?BestType=Bestseller&BranchType=1&CID=0&page=1&cnt=1000&SortOrder=1")

# ip block 방지
time.sleep(10)

# 소스 얻기
my_source = my_driver.page_source

# parsing(page 전체)
my_parsed_source = BeautifulSoup(my_source, "html.parser")

my_driver.close()
```

<br>

- str로 형변환 된 source code > `[N번].find_all('li')[0]`: 첫 번째 index item    
\> 만약 ']'로 끝나면 > title 아니고 증정품 > `index += 1` 수행

```python
div_book_box = my_parsed_source.find_all("div", class_="ss_book_box")

for this_div_book in div_book_box:
    this_li_book = this_div_book.find_all("li")
    this_td_book = this_div_book.find_all("td")

    first_line = this_li_book[0].text
    # 첫째 line이 ']'로 끝나면 (증정품) > index 밀기
    nIndex = 0
    if (first_line[len(first_line) - 1] == ']'):
        nIndex = 1
    
    this_rank = this_td_book[1].text
    this_title= this_li_book[nIndex].text.split("]")[1]
    this_author = this_li_book[nIndex + 1].text.split(" (")[0]
    this_price = this_li_book[nIndex + 2].text.split("원 →")[0].replace(",", "，")
    
    print(this_rank, this_title, this_author, this_price)
```

```text
1.  트렌드 코리아 2023  김난도, 전미영, 최지혜, 이수진, 권정윤, 이준영, 이향은, 한다혜, 이혜원, 추예린 19，000
2.  아버지의 해방일지   정지아 15，000
3.  이토록 평범한 미래   김연수 14，000
4.  단순한 열정 (무선) ㅣ 문학동네 세계문학전집 99  아니 에르노 10，000
5.  세상에서 가장 쉬운 본질육아  지나영 18，800
6.  헤어질 결심 스토리보드북   이윤호, 박찬욱 33，000
7.  불편한 편의점 2  김호연 14，000
8.  역행자  자청 17，500
9.  물고기는 존재하지 않는다 (리커버 특별판, 양장)  룰루 밀러 17，000
10.  하얼빈   김훈 16，000
11.  총 균 쇠 (리커버 특별판)  재레드 다이아몬드 28，000
12.  올리버쌤의 미국식 아이 영어 습관 365  올리버 샨 그랜트 23，000
13.  가녀장의 시대   이슬아 15，000
14.  불편한 편의점 (40만부 기념 벚꽃 에디션)  김호연 14，000
15.  작은 땅의 야수들  김주혜 18，000
16.  세월 ㅣ 아니 에르노 컬렉션   아니 에르노 15，500
17.  아름다운 초저녁달 4  야마모리 미카 6，000
18.  파친코 1  이민진 15，800
19.  흔한남매의 흔한 호기심 7 ㅣ 흔한남매   안치현 14，000
20.  파친코 2  이민진 15，800
21.  나는 오래된 거리처럼 너를 사랑하고 ㅣ 문학과지성 시인선 572   진은영 12，000
22.  2023 써니 행정법총론 기출문제집 - 전2권  박준철 42，000
23.  쇼타 형아 2  나카야마 미유키 6，500
24.  이 책은 돈 버는 법에 관한 이야기  고명환 16，800
25.  사람을 얻는 지혜  발타자르 그라시안 13，800
26.  마지막 이야기 전달자   도나 바르바 이게라 18，000
27.  잘될 수밖에 없는 너에게  최서영 16，000
28.  2022 김승옥문학상 수상작품집   편혜영, 김연수, 김애란, 정한아, 문지혁, 백수린 10，000
29.  그 길로 갈 바엔 ㅣ 젊은 만화가 테마단편집 2   재활용, 약국, 서글, 각종모에화, 하양지 15，000
30.  그리고 행복하다는 소식을 들었습니다  이병률 15，800
31.  마음세탁소  황웅근 15，000
32.  부자 아빠 가난한 아빠 1 (20주년 특별 기념판)   로버트 기요사키 15，800
33.  ETS 토익 정기시험 기출문제집 1000 Vol. 3 Reading (리딩) ㅣ ETS 토익 정기시험 기출문제집   ETS 17，800
34.  2022 큰별쌤 최태성의 별★별한국사 기출 500제 한국사능력검정시험 심화 (1.2.3급) ㅣ 큰별쌤 최태성의 별★별한국사 한국사능력검정시험 시리즈   최태성 19，500
35.  빠르게 실패하기  존 크럼볼츠, 라이언 바비노 16，500
36.  헤어질 결심 각본  박찬욱, 정서경 15，000
37.  ETS 토익 정기시험 기출문제집 1000 Vol. 3 Listening (리스닝) ㅣ ETS 토익 정기시험 기출문제집   ETS 17，800
38.  도쿄 에일리언즈 4 (초판 한정 홀로그램 양면 일러스트 카드 + 선착순 한정 아크릴 일러스트 카드 포함 특장판)  NAOE 5，000
39.  원씽 The One Thing (리커버 특별판)   게리 켈러, 제이 파파산 14，000
40.  10배의 법칙   그랜트 카돈 16，800
41.  빈 옷장 ㅣ 아니 에르노 컬렉션   아니 에르노 14，500
42.  퀀트 투자 무작정 따라하기 ㅣ 무작정 따라하기 경제경영/재테크   강환국 25，000
43.  책과 우연들  김초엽 16，000
44.  마흔에 읽는 니체  장재형 16，000
45.  과학이 필요한 시간  궤도 16，000
46.  해커스 토익 기출 보카 TOEIC VOCA 단어장  David Cho 12，900
47.  길티 이노센스 5  윤한 6，000
48.  그릿 GRIT (100쇄 기념 리커버 에디션)   앤절라 더크워스 16，000
49.  사랑은 그렇게 하는 것이 아니다  김달 16，500
50.  세상의 마지막 기차역  무라세 다케시 14，000
```

<br>

## 3. 순회 크롤링 in 2중 for loop (crawler with traversal in Nested loop)

```python
# 구글웹드라이버 사용
my_driver = webdriver.Chrome()

# 리스트에 item append
rank_list = list()
title_list = list()
author_list = list()
price_list = list()

# 첫 번째 for loop: 사이트 traversal
for i in range(1, 21):
    # 알라딘 베스트셀러 사이트 n페이지 접속
    my_driver.get("https://www.aladin.co.kr/shop/common/wbest.aspx?BestType=Bestseller&BranchType=1&CID=0&page=%s&cnt=1000&SortOrder=1" %i)

    # ip block 방지
    time.sleep(10)

    # 소스 얻기
    my_source = my_driver.page_source

    # parsing(page 전체)
    my_parsed_source = BeautifulSoup(my_source, "html.parser")
    
    div_book_box = my_parsed_source.find_all("div", class_="ss_book_box")
    
    # 두 번째 for loop: 페이지 내의 class ss_book_box item traversal 
    for this_div_book in div_book_box:
        this_li_book = this_div_book.find_all("li")
        
        # rank item tag "td"
        this_td_book = this_div_book.find_all("td")
        
        first_line = this_li_book[0].text
        # 첫째 line이 ']'로 끝나면 (증정품) > index 밀기
        nIndex = 0
        if (first_line[len(first_line) - 1] == ']'):
            nIndex = 1
    
        this_rank = this_td_book[1].text
        
        this_title= this_li_book[nIndex].text.split("]")[1]
        this_author = this_li_book[nIndex + 1].text.split(" (")[0]
        this_price = this_li_book[nIndex + 2].text.split(" →")[0].replace("원","").replace(",", "，")
    
        rank_list.append(this_rank)
        title_list.append(this_title)
        author_list.append(this_author)
        price_list.append(this_price)
    
        print(this_rank, this_title, this_author, this_price)

my_driver.close()
```

```text
1.  트렌드 코리아 2023  김난도, 전미영, 최지혜, 이수진, 권정윤, 이준영, 이향은, 한다혜, 이혜원, 추예린 19，000
2.  아버지의 해방일지   정지아 15，000
3.  이토록 평범한 미래   김연수 14，000
4.  단순한 열정 (무선) ㅣ 문학동네 세계문학전집 99  아니 에르노 10，000
5.  세상에서 가장 쉬운 본질육아  지나영 18，800
6.  헤어질 결심 스토리보드북   이윤호, 박찬욱 33，000
7.  불편한 편의점 2  김호연 14，000
8.  역행자  자청 17，500
9.  물고기는 존재하지 않는다 (리커버 특별판, 양장)  룰루 밀러 17，000
10.  하얼빈   김훈 16，000
11.  총 균 쇠 (리커버 특별판)  재레드 다이아몬드 28，000
12.  올리버쌤의 미국식 아이 영어 습관 365  올리버 샨 그랜트 23，000
13.  가녀장의 시대   이슬아 15，000
14.  불편한 편의점 (40만부 기념 벚꽃 에디션)  김호연 14，000
15.  작은 땅의 야수들  김주혜 18，000
16.  세월 ㅣ 아니 에르노 컬렉션   아니 에르노 15，500
.
.
.
993.  나의 아저씨 1~2 세트 - 전2권 ㅣ 인생드라마 작품집 시리즈   박해영 49，600
994.  이파라파냐무냐무 ㅣ 사계절 그림책    이지은 15，000
995.  아내를 모자로 착각한 남자   올리버 색스 18，500
996.  20일 만에 끝내는 해커스 토익 750+ RC (리딩) ㅣ 20일 만에 끝내는 해커스 토익   David Cho 14，900
997.  잘 그리기 금지   사이토 나오키 16，500
998.  우울할 땐 뇌 과학  앨릭스 코브 17，000
999.  우리는 언제나 다시 만나 ㅣ 스콜라 창작 그림책 7   윤여림 12，000
1000.  해커스 토플 리딩 (Hackers TOEFL Reading) ㅣ 해커스 토플   David Cho 23，900
```

<br>

```python
info_list = [rank_list, title_list, author_list, price_list]
for info in info_list:
    print(len(info))
```

```text
999
999
999
999
```

-  1000위까지 존재, but len(list) == 999 ? > 중간에 **빠진 index check**

```python

rank_check = []
    
for rank in rank_list:
    rank_check.append(int(rank.split(".")[0]))

for i in range(1, 1001):
    if not i in rank_check:
        print(i)
```

```text
63
```

- 빠진 index == 63 > 실제 webpage에서 63th rank 없는 것 확인 완료 > data 출력

<br>


```python
# .csv file로 출력
w = codecs.open("C:/Users/EthanJ/develop/Basic_Python_with_Data/crawling/crawler_with_traversal_in_Nested_loop.csv", encoding="utf-8-sig", mode="w")

this_line = str()

for i in range(len(rank_list)):
    this_line = str("%s,%s,%s,%s\n" %(rank_list[i].replace(",", "，"), title_list[i].replace(',', '，'),
                                      price_list[i].replace(',', '，'), author_list[i].replace(',', '，')))
    
    w.write(this_line)
    
w.close()
```

![1013_3_1](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1013_3_1.PNG?raw=true)

![1013_3_2](https://github.com/insung-ethan-j/Basic_Python_with_Data/blob/13744218c87d839e99c05dcc2036ba96fbaa6f12/img_source/1013_3_2.PNG?raw=true)