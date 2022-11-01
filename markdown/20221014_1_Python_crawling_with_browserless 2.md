# Python crawling with browserless

# 파이썬 browserless 크롤링

<br>

## requests library

- requests는 브라우저 없이 Python에서 다이렉트로 server에 http 요청 전송

- request를 받은 webpage server는 해당 page의 source code를 Python 내부로 전송

- 단, 버튼 클릭이나 광고 닫기 등의 web page내에서의 작업은 물리 browser가 없기 때문에 불가능

- 오로지 특정 url로 접속했을 떄 최초에 response되는 webpage code만 가져오기 가능

- 대신 물리 브라우저를 직접 켜지 않고, 코드 내에서 request만 수행 > 속도, resource면에서 우월함

<br>

### 웹페이지와 네트워크

1. user가 브라우저로 특정 서버 주소를 입력해 접속 시도를 한다

2. 서버에 user request 전송

3. 서버가 request에 response하면서 response data 함께 전송

4. brower가 받은 data를 해석해서 PC screen에 반영

- website 접속이면 source code를 받아서 자동으로 화면에 그려줌 (lendering)

- game에 대한 response data면 해당 명령어가 들어가서 게임 상황에 반영

<br>

```python
# library import
import requests
from bs4 import BeautifulSoup
import time
```

<br>

- 특정 url에 대한 요청 == `requests.get("url")`

- request에 대한 server response는 req변수에 저장

```python
req = requests.get("http://www.naver.com")
```

<br>

- html source code 저장 (selenium의 `.text` 와 문법은 같으나 기능이 다름에 주의)

 - (requests) `req.text` == (seleniunm) `driver.page_source`

```python
source = req.text
print(source)
print(type(source))
```

```text
  
<!doctype html>                          <html lang="ko" data-dark="false"> <head> <meta charset="utf-8"> <title>NAVER</title> <meta http-equiv="X-UA-Compatible" content="IE=edge"> <meta name="viewport" content="width=1190"> <meta name="apple-mobile-web-app-title" content="NAVER"/> <meta name="robots" content="index,nofollow"/> <meta name="description" content="네이버 메인에서 다양한 정보와 유용한 컨텐츠를 만나 보세요"/> <meta property="og:title" content="네이버"> <meta property="og:url" content="https://www.naver.com/"> <meta property="og:image" content="https://s.pstatic.net/static/www/mobile/edit/2016/0705/mobile_212852414260.png"> <meta property="og:description" content="네이버 메인에서 다양한 정보와 유용한 컨텐츠를 만나 보세요"/> <meta name="twitter:card" content="summary"> <meta name="twitter:title" content=""> <meta name="twitter:url" content="https://www.naver.com/"> <meta name="twitter:image" content="https://s.pstatic.net/static/www/mobile/edit/2016/0705/mobile_212852414260.png"> <meta name="twitter:description" content="네이버 메인에서
.
.
.
policy/service_group.html" data-clk="policy">네이버 정책</a></li> <li class="corp_item"><a href="https://help.naver.com/" data-clk="helpcenter">고객센터</a></li> </ul> <address class="addr"><a href="https://www.navercorp.com" target="_blank" data-clk="nhn">ⓒ NAVER Corp.</a></address> </div> </div> </div> </div> <div id="adscript" style="display:none"></div> </body> </html>

<class 'str'>
```

<br>

- http 헤더 가져오기  > request에 따른 detail information 보여줌

```python
print(req.headers)
```

```text
{'Server': 'NWS', 'Date': 'Fri, 14 Oct 2022 02:12:26 GMT', 'Content-Type': 'text/html; charset=UTF-8', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'Set-Cookie': 'PM_CK_loc=eeb1b259a03c465783af0c5c528ee7b7f75400c8075582247a7d434d73103936; Expires=Sat, 15 Oct 2022 02:12:26 GMT; Path=/; HttpOnly', 'Cache-Control': 'no-cache, no-store, must-revalidate', 'Pragma': 'no-cache', 'P3P': 'CP="CAO DSP CURa ADMa TAIa PSAa OUR LAW STP PHY ONL UNI PUR FIN COM NAV INT DEM STA PRE"', 'X-Frame-Options': 'DENY', 'X-XSS-Protection': '1; mode=block', 'Content-Encoding': 'gzip', 'Strict-Transport-Security': 'max-age=63072000; includeSubdomains', 'Referrer-Policy': 'unsafe-url'}
```

<br>

- http status code 가져오기 > `code == 200`: 정상 접속

```python
print(req.status_code)
```

```text
200
```

<br>

### > 알라딘 베스트셀러 7페이지 Crwaling 실습

1. `requests` 활용
2. `BeautifulSoup` 구간부터는 `selenium`을 활용한 crawling과 차이 없음.

```python
# requests > source code 요청
my_req = requests.get("https://www.aladin.co.kr/shop/common/wbest.aspx?BestType=Bestseller&BranchType=1&CID=0&page=%s&cnt=1000&SortOrder=1" %7)

# ip block 방지 지연
time.sleep(10)

# source code > string  type 변환
my_source = my_req.text

# parsing
my_parsed_source = BeautifulSoup(my_source, "html.parser")

# tag와 class로 data extraction
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
    this_price = this_li_book[nIndex + 2].text.split(" →")[0].replace("원","").replace(",", "，")
    
    print(this_rank, this_title, this_author, this_price)
```

```text
301.  세상 쉬운 영문법  윤여홍 19，800
302.  어떻게 말해줘야 할까 (60만 부 기념 한정판 리커버)  오은영 17，500
303.  브라질에 비가 내리면 스타벅스 주식을 사라  피터 나바로 18，000
304.  재수사 2   장강명 16，000
305.  뇌, 욕망의 비밀을 풀다   한스-게오르크 호이젤 18，000
306.  임신 출산 육아 대백과 (2022~2023년 개정판)  삼성출판사 편집부 19，500
307.  부동산 경매로 1년 만에 꼬마빌딩주 되다  김상준 18，000
308.  사로잡는 얼굴들  이사 레슈코 28，000
309.  부부 이상, 연인 미만. 1~2 합본판  카나마루 유키 12，000
310.  그들의 말 혹은 침묵  아니 에르노 14，000
311.  데뷔 못 하면 죽는 병 걸림 1부 초판 한정 굿즈박스 세트 ㅣ 데뷔 못 하면 죽는 병 걸림 1  백덕수 95，000
312.  미움받을 용기 (반양장) ㅣ 미움받을 용기 1  기시미 이치로, 고가 후미타케 14，900
313.  체리새우 : 비밀글입니다 ㅣ 문학동네 청소년 42  황영미 11，500
314.  정의란 무엇인가   마이클 샌델 15，000
315.  나는 당신이 행복했으면 좋겠습니다  박찬위 13，800
316.  어디로 가세요 펀자이씨? ㅣ 펀자이씨툰 1   엄유진 16，000
317.  부모의 말  김종원 16，800
318.  혼자 공부하는 컴퓨터 구조 + 운영체제  강민철 28，000
319.  우리가 빛의 속도로 갈 수 없다면   김초엽 14，000
320.  당신이 옳다   정혜신 15，800
321.  2023 선재국어 기출실록 (해설 통합형) 세트 - 전4권 ㅣ 2023 선재국어   이선재 49，000
322.  이게 다 호르몬 때문이야  마쓰무라 게이코 15，000
323.  지리의 힘 ㅣ 지리의 힘 1   팀 마샬 17，000
324.  마지막 몰입  짐 퀵 16，800
325.  송사무장의 부동산 경매의 기술  송희창 16，000
326.  사랑의 기술   에리히 프롬 12，000
327.  알고 있다는 착각  질리언 테트 17，800
328.  신경 청소 혁명  구도 치아키 14，000
329.  영어 감정 표현 사전  샘 노리스 27，000
330.  슬로우 미러클 영어 그림책 느리게 100권 읽기의 힘  고광윤 29，800
331.  토끼전 : 꾀주머니 배 속에 차고 계수나무에 간 달아 놓고 ㅣ 국어시간에 고전읽기 (휴머니스트) 8  장재화 13，000
332.  리틀 라이프 1  한야 야나기하라 14，800
333.  타자들의 생태학 ㅣ 월딩 시리즈 1  필리프 데스콜라 18，000
334.  2023 문동균 한국사 한 권으로 모든 것을 정리하는 빈칸노트  문동균 12，000
335.  오늘도 고바야시 서점에 갑니다  가와카미 데쓰야 15，000
336.  2023 전한길 한국사 3.0 기출문제집 - 전2권  전한길 38，000
337.  협력의 유전자  니컬라 라이하니 22，000
338.  거꾸로 읽는 세계사  유시민 17，500
339.  스파이 패밀리 2  엔도 타츠야 6，000
340.  스파이 패밀리 8  엔도 타츠야 6，000
341.  포켓몬스터 썬&문 포켓몬 전국대도감  가와시마 준지 9，500
342.  프레임  최인철 17，000
343.  법륜 스님의 행복  법륜 14，000
344.  나의 히어로 아카데미아 35  호리코시 코헤이 6，000
345.  카구야 님은 고백받고 싶어 24  아카사카 아카 6，000
346.  지극히 사적인 네팔  수잔 샤키야, 홍성광 16，300
347.  2023 심우철 문법 풀이 전략서  심우철 15，000
348.  투자에도 순서가 있다  홍춘욱 18，000
349.  전쟁과 약, 기나긴 악연의 역사  백승만 17，000
350.  사회사상의 역사  사카모토 다쓰야 33，000
```


