# Python File IO with codecs and Encoding

# codecs 라이브러리를 활용한 파이썬 파일 입출력 and Encoding

## 1. codecs 라이브러리

- Python ver.3.5. > 내장 라이브러리로 바뀜 (예전에는` pip install codecs` 명령어 수행해야 했음)

- 파이썬으로 (텍스트)파일을 제어할 수 있도록 (읽어오기, 쓰기) 도와줌

- console에 출력된 내용을 txt로 옮겨서 출력, 읽어올 때 사용

- 특이사항

	- ~~개행은 "\\r\\n"으로 처리~~ 현재는 "\\n"으로 개행 처리 가능

	- mode

		- w: 기존에 있던 자료 없에고 새 파일 입력

		- a: 기존에 있던 자료에 이어서 계속 입력

		- r: 기존 파일에 있던 내용 읽어오기

<br>

```python
# import codecs library

import codecs
```

<br>

### 1.1. mode="w"

- `codecs.open()` return값 > txt file 그 자체처럼 사용함

- `codecs.open()` parameter: ("txt file 절대경로" > 없으면 file 생성, (mode="mode"), (encoding="인코딩 방식",))

- mode parameter default == `r`

- option paramter default == `utf-8`

<br>

- file path > **must replace '\\' with '/'

```python
f = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/File_IO/txt_file_test.txt", mode="w")
```

<br>

```python
print(type(f))
```

```text
<class '_io.TextIOWrapper'>
```

<br>

- `f.write("things to write")` > txt file 내부에 작성

```python
# "Hello world[n]" 10번 작성

for i in range(10):
    f.write("Hello world[%s]\n" %(i+1))
```

<br>

- `f.close()` 종료 필수 > 미종료시 temp처리됨 > txt file 변경사항 적용X

```python
f.close()
```


<br>

![f_write](https://github.com/insung-ethan-j/Python_basic/blob/c741e1fa60c8fb454e673b693ec20f78db472a59/img_source/1013_1.PNG?raw=true)

<br>

### 1.2. mode="r"

```python
# 기존 text file 읽기 > mode="r"
f = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/File_IO/txt_file_test.txt", mode="r")
```

#### 1.2.1.  `f.readline()` > 한 줄만 읽기 > io stream header 한 줄 아래로 이동


```python
read_line1 = f.readline()
read_line2 = f.readline()

# return type == str
print(type(read_line1))

print(read_line1)
print(read_line2)

# .readline() > file text line수보다 많이 실행하면 >
# > 이미 io stream header가 EOF로 이동 > empty string return
for line in range(15):
    my_line = f.readline()
    print(type(my_line), len(my_line))

f.close()
```

```text
<class 'str'>
Hello world[1]

Hello world[2]

<class 'str'> 15
<class 'str'> 15
<class 'str'> 15
<class 'str'> 15
<class 'str'> 15
<class 'str'> 15
<class 'str'> 15
<class 'str'> 16
<class 'str'> 0
<class 'str'> 0
<class 'str'> 0
<class 'str'> 0
<class 'str'> 0
<class 'str'> 0
<class 'str'> 0
```

<br>

#### 1.2.2. `f.readlines()` > 줄 단위, 각 줄을 string item으로 가진 list type으로 전부 읽어오기 > EOF로 io stream header 이동

```python
f = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/File_IO/txt_file_test.txt", mode="r")

read_lines = f.readlines()

print(type(read_lines)) # == list
print(type(read_lines[0])) # == str

print(read_lines)

# 한 번 실행 시 io stream header가 EOF로 이동 > 두 번 이상 실행시 이미 EOF에 위치 > empty list return
print(f.readlines())

f.close()
```

```text
<class 'list'>
<class 'str'>
['Hello world[1]\n', 'Hello world[2]\n', 'Hello world[3]\n', 'Hello world[4]\n', 'Hello world[5]\n', 'Hello world[6]\n', 'Hello world[7]\n', 'Hello world[8]\n', 'Hello world[9]\n', 'Hello world[10]\n']
[]
```

<br>

#### 1.2.3. `f.read()` > txt file 전체 문자열을 단일 string으로 읽어오기 > io stream header가 EOF로 이동

```python
f = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/File_IO/txt_file_test.txt", mode="r")

f_read = f.read()

print(type(f_read))
print(f_read)

# 한 번 실행 시 io stream header가 EOF로 이동 > 두 번 이상 실행시 이미 EOF에 위치 > empty str return
print(f.read())


f.close()
```

```text
<class 'str'>
Hello world[1]
Hello world[2]
Hello world[3]
Hello world[4]
Hello world[5]
Hello world[6]
Hello world[7]
Hello world[8]
Hello world[9]
Hello world[10]
```

<br>

- - -

## 1.3. csv 파일 다루기

`data.csv`: str > ,(comma)로 구분 된 형태의 파일 > excel 형식으로 읽기 가능!

<br>

#### 1.3.1. txt파일 > csv로 변환

```python
# 저장된 result.txt 읽어오기
r = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/File_IO/txt_file_test.txt", mode="r")

# txt > csv로 변환 > "utf-8" endcoding은 excel 실행 시 한글 깨짐 > "utf-8-sig" encoding 사용
w = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/File_IO/txt_to_csv.csv", encoding="utf-8-sig", mode="w")

w.write(r.read())

r.close()
w.close()
```


![csv](https://github.com/insung-ethan-j/Python_basic/blob/c741e1fa60c8fb454e673b693ec20f78db472a59/img_source/1013_1.PNG?raw=true)

<br>

#### 1.3.2. csv파일 작성

```python
w = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/File_IO/person_csv.csv", 'w', "utf-8-sig")

w.write("순번, 이름, 성별, 나이\n")

n_list = [1, 2, 3, 4]
str_name = ["김가람", "이나람","박다람", "최라람"]
sex_m_f = ["M", "F", "F", "M"]
n_age = ["24", "23", "35", "21"]

for i in range(4):
    this_line = "%s,%s,%s,%s\n" %(n_list[i], str_name[i], sex_m_f[i], n_age[i])
    w.write(this_line)

w.close()
```

![csv](https://github.com/insung-ethan-j/Python_basic/blob/c741e1fa60c8fb454e673b693ec20f78db472a59/img_source/1013_3.PNG?raw=true)


<br>

- - -

## 2. Is Python default encoding type "utf-8" ?

<br>

### 2.1. Checking with `sys` library
```python
import sys

print(sys.getdefaultencoding())
```

```text
utf-8
```

- sys > Python default encoding == "utf-8"

<br>

### 2.2. `id("str")` > str의 메모리 주소 check


```python
en_py = "Python"

print(id(en_py), id("Python"))
print(id("Python"), id("Python"))
print(id("Python"), id(en_py))
# 어느 줄에서 어떻게 선언하든 "Python"의 id는 같다
```

```text
3187690226032 3187690226032
3187690226032 3187690226032
3187690226032 3187690226032
````

<br>

```python
kr_py = "파이썬"

print(id(kr_py), id("파이썬"))
print(id("파이썬"), id("파이썬"))
print(id("파이썬"), id(kr_py))
# "파이썬"을 넣은 변수명, 같은 줄에서 선언된 "파이썬"만 id가 같다
```

```text
3187799468432 3187799467856
3187799467952 3187799467952
3187799388208 3187799468432
```

<br>
### 2.3. Checking with `chardet` library


- encoding type check library: `chardet`

```python
import chardet

encoded_en_py = en_py.encode()
print(encoded_en_py, type(encoded_en_py))

en_detect = chardet.detect(encoded_en_py)
print(en_detect, type(en_detect))

encoded_kr_py = kr_py.encode()
print(encoded_kr_py, type(encoded_kr_py))

kr_detect = chardet.detect(encoded_kr_py)
print(kr_detect)
```

```text
b'Python' <class 'bytes'>
{'encoding': 'ascii', 'confidence': 1.0, 'language': ''} <class 'dict'>
b'\xed\x8c\x8c\xec\x9d\xb4\xec\x8d\xac' <class 'bytes'>
{'encoding': 'utf-8', 'confidence': 0.87625, 'language': ''}
```

<br>

- [Python documentation: class str](https://docs.python.org/3/library/stdtypes.html#str?)

- [Python documentation: str.encode() method](https://docs.python.org/3/library/stdtypes.html?highlight=encode#str.encode)

- Python doucmentation에서 str type의 기본 encoding은 "utf-8"이라고 명시 돼 있으나,    
`chardet` library > `chardet.detect(str.encode())` method로 확인 해 본 결과

영어 str 인코딩 == '`ascii`'
한글 str 인코딩 == '`utf-8`'

<br>

### 2.4. Checking with `codecs` library

```python
# codecs library를 활용한 file IO
import codecs

f_write = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/encoding_test.csv", "w", "utf-8")
# "utf-8" 인코딩으로 파일 입력

f_write.write(en_py)
f_write.write(',')
f_write.write(kr_py)

f_write.close()   
```

<br>

```python
f_read = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/encoding_test.csv", "r")

print(f_read.read()) # UnicodeDecodeError

f_read.close()
```

```python
---------------------------------------------------------------------------
UnicodeDecodeError                        Traceback (most recent call last)
Input In [18], in <cell line: 3>()
      1 f_read = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/encoding_test.csv", "r")
----> 3 print(f_read.read()) # UnicodeDecodeError
      5 f_read.close()

UnicodeDecodeError: 'cp949' codec can't decode byte 0xed in position 7: illegal multibyte sequence
```


- [codecs.open() method documentation](https://docs.python.org/3/library/codecs.html?highlight=codecs%20open#codecs.open)
- `codecs.open("file path", "w", encoding="utf-8")` > utf-8 인코딩으로 입력 > 이상 없음

<br>

- `codecs.open("file path", "r"(, encoding=None))` > Python 자체 인코딩으로 읽기 > `UnicodeDecodeError: cp949 codecs` decoding중 Error 발생 

- **한글 문서 특정 encoding 지정하지 않을 시 > `cp949` encoding으로 파일 인식**

<br>

### 2.4. Conclusion

: `codecs.open()` method parameter > `encoding="encoding"` 지정 필수

```python
# codecs.open() encoding="utf-8"
f_read = codecs.open("C:/Users/EthanJ/develop/PLAYDATA/Python_basic/encoding_test.csv", "r", encoding="utf-8")

print(f_read.read())
# encoding 지정 후 실행 가능

f_read.close()
```

```text
Python,파이썬
```

<br>


