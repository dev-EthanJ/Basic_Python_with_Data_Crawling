# 파이썬 함수 Python function

- 반복적으로 사용하는 기능을 묶어서 함수로 정의하면 간편하게 사용할 수 있다.

- 기본 문법

```python
def 함수이름(매개변수):
	<수행할 내용1>
	<수행할 내용2>
	<수행할 내용3>
	...
	<수행할 내용n>

	return <return값>
```

- 매개변수

	- 함수 안에서 사용할 값을 전달하는 변수로, `<수행할 내용>` 문장에서 변수로 사용된다.

	-  함수 정의문에서는 매개변수로서 선언되며 실제로 함수를 호출할 때는 인자값으로 전달된다.

- 용어

	- def : 함수를 정의할 때 사용하는 키워드
	- return: 함수의 결과값을 반환하는 키워드
	- 입력값 == 인수, 입력 인수, 인자
	- 결과값 == 출력값, 리턴값, 돌려주는 값, 반환 값

<br>

```python
# 기본적인 함수의 예제 (return 구문 없음 > 불완전한 함수)
def plus(a):
	print(a + 1)

plus(5)
```

```text
6
```

<br>

- 함수 내부에 선언된 변수는 함수 외부에서 호출할 수 없다.

- 함수나 기타 블록 내부에 선언된 변수 == "지역변수"(local variable)

```python
# plus(5) 실행 > 6 출력
# print(plus(5)) > ?

print(plus(5))
```

```text
6
None
```

- - -

## 0. return

- return 구문은 함수의 종료 지점에 사용할 수 있다.

- return 구문 실행 > 함수 종료, 함수의 결과값(=return값)이 호출한 위치에 할당

<br>

```python
def plus2(b):
	return b + 1

print(plus2(5))
# plus2(5) return값 == 6 > print(6)으로 replaced
```

```text
6
```

<br>

```python
# print() method > 출력 기능 수행, return값 없음(== None )


print(print())
```

```text
None
```

<br>

- - -

## 1. 매개변수와 인수

### 1.1. 매개변수(== parameter, 입력인자, 입력값)

- callee된 함수가 전달된 변수를 저장하는 "**local 변수명**"

- 함수를 **선언**할 때 naming하는 변수 "**identifier**"

<br>

### 1.2. 인수, 호출인자(== argument) 

- 함수를 call할 때 전달하는 caller의 입력 "**value**"

<br>

### > 연습문제

- parameter를 2개 이상 받는 경우

- sum_func 식별자를 가진 함수 선언

- 이 함수는 num1, num2 2개의 변수를 입력받고 둘을 합산한 값을 return

```python
# snake case naming convention 사용
def sum_func(num1, num2):
	sum = num1 + num2

	return sum

print(sum_func(29, 34))
```

```text
63
```

<br>

## 2. 매개변수가 있는 함수

- 인자 값(argument)을 전달할 때 값만 입력하면 순서대로 매개변수(parameter)에 할당

- "parameter identifier"를 지정하여 전달할 수 있다.

<br>

### > 연습문제

- parameter가 3개인 함수

- parameter identifier는 자유

- print()로 parameter 3개를 console에 출력

- result 변수에 3개 합을 저장한 뒤 return

- function identifier는 "test_three"

<br>

```python
def test_three(first, second, third):
	print(first, '+', second, '+', third, ' ', end="")

	result = first + second + third

	print('=', result)

	return result

test_three(12, 23, 34)
```

```text
12 + 23 + 34  = 69
69
```

<br>

- parameter 순서 다르게 전달 > parameter 이름 지목해서 호출

```python
test_three(third = 30, second = 20, first = 10)
```

```text
10 + 20 + 30  = 60
60
```

<br>

### 2.1. 매개변수 초기 값 설정

- 함수 정의문(할당문)에서 매개변수 값을 선언하면 초기값(default value)로 설정 됨

- 함수 호출 시 값을 입력하지 않으면 > default값 사용

- default값을 할당하고 싶은 매개변수들은 뒤쪽에 쓰고 함수 선언

> string % operator에서 index 번호, 변수 name 사용 규칙과 유사 (하나씩 할당)

<br>

```python
# default값 선언 예시

def test2_func(a=1, b=2, c=3):
	result = a + b + c

	return result

print(test2_func())
```

```text
6
```

<br>

```python
# 사용자가 입력한 값이 우선 사용

print(test2_func(7, 5, 6)
```

```text
18
```

<br>

- 함수 선언시 할당한 parameter 개수보다 적은 개수로 함수 calling시     
\> 앞에 위치한 parameter부터 할당, 나머지는 default값 적용

```python
print(test2_func(10))
# print(test2_func(10, default(==2), default(==3)))

print(test2_func(10, 20))
# print(test2_func(10, 20, defualt(==3)))
```

```text
15
33
```

<br>

- default값 매개변수는 반드시 오른쪽 parameter부터 지정!
```python
def test3_func(a=1, b=2, c):
    result = a + b + c
    
    return result
```

```python
  Input In [15]
    def test3_func(a=1, b=2, c):
                              ^
SyntaxError: non-default argument follows default argument
```

<br>

- default값 할당된 매개변수 > 값을 전달해도 실행 가능, 전달하지 않아도 가능

- default값 할당되지 않은 매개변수 > 반드시 전달값 입력해서 함수 call

```python
def test4_func(a, b, c=10):
	result = a + b + c

	return result

print(test4_func(1, 2))
print(test5_func(1))
```

```python
13

---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Input In [19], in <cell line: 7>()
      4     return result
      6 print(test4_func(1, 2))
----> 7 print(test4_func(1))

TypeError: test4_func() missing 1 required positional argument: 'b'
```

<br>

### 2.2. 가변인자 리스트: \*param

- 입력할 parameter의 개수가 몇 개인지 모를 때(정해지지 않았을 때) 사용

- parameter identifier 왼쪽에 \*(asterisk)를 붙인다.

- 함수를 call할 때 parameter를 ,(comma)로 연결해서 call한다.

<br>

- 대표적인 가변인자 사용 함수 > print()
```python
print("EthanJ", 1000, 50, "qwerty")
```

```text
EthanJ 1000 50 qwerty
```

<br>

- parameter 개수가 정해진 함수들 > 인자 개수 고정   
\> parameter 개수가 정해지지 않았을 때 사용할 수 없음

```python
def my_print4(p1, p2, p3, p4):
    print("%s%s%s%s" %(p1, p2, p3, p4))

def get_member_list1(mem1):
    print("출석 학생은 %s님 입니다." %mem1)
    
def get_member_list2(mem1, mem2):
    print("출석 학생은 %s님, %s님 입니다." %(mem1, mem2))

my_print4("가나다", 30, "abc", 50)
    
get_member_list1("김철수")

get_member_list2("홍길동", "김영희")
# get_member_listN() 함수를 매번 할당하는 건 비효율적 
```

```text
가나다30abc50
출석 학생은 김철수님 입니다.
출석 학생은 홍길동님, 김영희님 입니다.
```

<br>

- 가변인자 list 사용한 함수 > \*(asterisk) + parameter name 으로 함수 할당

```python
def get_members(*mem):
    print("출석 학생은 ", end="")
    
    for i in range(len(mem)):
        print("%s님, " %mem[i], end="")
        
    print("입니다.")
    
    
get_members("뽀로로")

get_members("뽀로로", "루피")

get_members("뽀로로", "루피", "애옹", "갱쥐")
```

```text
출석 학생은 뽀로로님, 입니다.
출석 학생은 뽀로로님, 루피님, 입니다.
출석 학생은 뽀로로님, 루피님, 애옹님, 갱쥐님, 입니다
```


<br>

- 가변인자 list data type == tuple

```python
def parameter_test(*param):
	print(type(param))

parameter_test(10, 20, 30)
```

```text
<class 'tuple'>
```

<br>

### > 연습문제

- 가변인자를 이용해서 학생들의 점수를 입력받을 때마다 저장 후    
총점을 구해 평균값을 리턴하는 코드를 작성하시오

```python
def get_avg(*scores):
    total_score = 0
    
    # 기존 코딩 스타일
    """
    for i in range(len(scores)):
        total_score += scores[i]
    """
    
    # Python 코드 > tuple을 반복문 scope로 설정 > tuple item수만큼 반복
    for score in scores:
        total_score += score
    
    total_score /= len(scores)
    
    return total_score

get_avg(70, 80, 90)
```

```text
80.0
```

<br>

#### 2.3. 키워드 파라미터: \*\*kwargs (keyword arguments)

- 함수 선언시, 길이가 정해지지 않은 dict형태로 parameter 할당

- **\*\*kwargs**: dict 가변인자

- 함수 caller의 매개변수: (k1=v1, ... , kn=vn) 형식으로 함수 call (tuple처럼)

<br>

#### 2.3.1. callee function parameter == \*\*kwargs
- type(kwargs) == dict 
- print(kwargs) == print(dict)
- print(kwargs.keys()) == print(dict.keys()) > dict_keys list
- print(kwargs.values()) == print(dict.values()) > dict_values list

<br>
- argument 문법: (k1=v1, k2=v2, k3=v3, ..., kn=vn)
- kn == str type except for " ", vn == any data type

```python
def kwargs_check_func(**kwargs):
    print(type(kwargs)) # dict
    print(kwargs) # print(dict)
    print(kwargs.keys()) # print(dict.keys()) > dict_keys list
    print(kwargs.values()) # print(dic.values()) > dict_values list

kwargs_check_func(fir=7, sec='k', thr="str", fou=['l', 'i', 's', 't'],
            fif={"k1":"y1"}, six=('t', 'u'))
```

```text
<class 'dict'>
{'fir': 7, 'sec': 'k', 'thr': 'str', 'fou': ['l', 'i', 's', 't'], 'fif': {'k1': 'y1'}, 'six': ('t', 'u')}
dict_keys(['fir', 'sec', 'thr', 'fou', 'fif', 'six'])
dict_values([7, 'k', 'str', ['l', 'i', 's', 't'], {'k1': 'y1'}, ('t', 'u')])
```

<br>

#### 2.3.2. for loop data로 kwargs 가변인자 사용하기

- for item in data(== kwargs) > return kwargs.keys() 

- for item in kwargs.keys()) > return kwargs.keys()

- for item in kwarga.values() > return kwargs.values()

```python
def kwargs_func(**kwargs):
    
    # for item in data(==kwargs) > return kwargs.keys()
    for item in kwargs:
        print(f"{item} ", end="")
    print()
    
    # for item in kwargs.keys()) > return kwargs.keys()
    for item_key in kwargs.keys():
        print(f"{item_key} ", end="")
    print()
    
    # for item in kwarga.values() > return kwargs.values()
    for item_value in kwargs.values():
        print(f"{item_value} ", end="")
    print()
        

kwargs_func(fir=7, sec='k', thr="str", fou=['l', 'i', 's', 't']
            , fif={"k1":"y1"}, six = ('t', 'u'))
```

```text
fir sec thr fou fif six 
fir sec thr fou fif six 
7 k str ['l', 'i', 's', 't'] {'k1': 'y1'} ('t', 'u')
```

<br>

---

## 3. 함수의 결과값 (== return값)

- 함수의 return값은 항상 1개
- 여러 개의 값을 ,(comma)로 연결해서 return > return tuple

<br>

```python
def plus_multi_func(a, b):
    plus = a + b
    multi = a * b
    
    return plus, multi

double_values = plus_multi_func(3, 4)

print(double_values)
```

```text
(7, 12)
```

<br>

- 결과를 개별 변수에 나눠서 가져오기 == unpacking

```python
p, m = plus_multi_func(5, 6)

print(f'p = {p}, m = {m}')
```

```text
p = 11, m = 30
```

<br>

### > 연습문제

- 리스트로 입력받은 수들에 대해 양수만 필터링 해 반환하는 함수를 작성하시오.

1. 입력값은 parameter로 가변인자 list를 전달
2. return값은 양수로만 구성된 새로운 리스트

<br>

```python
def only_pos_list(*vargs):
    pos_list = list()
    
    for value in vargs:
        if value > 0:
            pos_list.append(value)
            
    return pos_list

print(only_pos_list(1, -7, 200, -45, 0))
# 가변인자 리스트 = tuple type으로 parameter 전달
```

```text
[1, 200]
```

