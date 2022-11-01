# Python OOP with Structures and Classes <br> 파이썬 객체 지향 프로그래밍 with 구조체와 클래스

## 1. 구조체

- 클래스(class): C언어의 구조체에서 확장된 개념 > 클래스 이전에 구조체 학습

- 구조체: 배열과 달리 인덱스가 아닌 "변수명"으로 object를 사용

### 1.1. 추상화 (abstaction)

: 일부 관점(기준)으로 바라본 요소를 추출해서 표현하는 것

- 예를 들어, 사람을 프로그래밍적 관점에서 표현하면, 인간의 모든 구성 중에서    
숫자로 표현 가능한 것, bool type으로 표현할 수 있는 것 등으로 추려서 표현하는 것

- 추상화(abstraction)를 통해 programming 개발에 사용 가능!

<br>

- 추상화 예시

1. 고양이의 요소(item): 털 색, 무게, 품종, 나이, 선호 간식, 집 주소, 성별, 점프력, 울음소리 등

2. 동물병원에 등록할 때 > 주인, 이름, 나이, 품종만 골라서 정의 가능

> 개발시, 개발자가 고려할 수 있는(혹은 기능이 요구하는 최소한의) 사항만을 프로그래밍

<br>

### 1.2. 구조체 구현 문법

- 클래스(를 구조체로 사용한) 구현 문법

- class: 설계도 처럼 작용 > 실제 객체(object) 생성 전까지 class 선언 자체로는 기능하지 않는다

```python
class Cat:
    name = ""
    age = 0
    cat_type = ""
    owner = ""
```

- 실제로는 class object 생성을 해줘야 기능한다

- 클래스 생성자를 통해 받은 return값 == 객체(objet) == 인스턴스(instance)

```python
cat1 = Cat()

print(cat1)
print(Cat)
```

```text
<__main__.Cat object at 0x00000214930DBE20>
<class '__main__.Cat'>
```

<br>

```python
cat1.name = "룰루"
cat1.age = 2
cat1.cat_type = "스코티시 폴드"
cat1.owner = "이미영"

print(cat1.name)
print(cat1.age)
print(cat1.cat_type)
print(cat1.owner)
```

```text
룰루
2
스코티시 폴드
이미영
```

<br>

```python
print("이름 : %s, 나이 : %s, 품종 : %s, 주인 : %s" %
      (cat1.name, cat1.age, cat1.cat_type, cat1.owner))
```

```text
이름 : 룰루, 나이 : 2, 품종 : 스코티시 폴드, 주인 : 이미영
```

<br>

### 1.3. 구조체의 요소에 대한 함수

-  class `Cat` instance에 대한 정보를 하나하나 `print()`로 조회해야 해서 불편   
\> class `Cat`이 사용할 수 있는 fuction 사용하면 같은 기능을 함수 단위로 사용 가능

- class `Cat` instance가 사용할 수 있는 function

```python
def show_cat_info(cat):
    print("이름 : %s, 나이 : %s, 품종 : %s, 주인 : %s" %
      (cat.name, cat.age, cat.cat_type, cat.owner))
```

<br>

- class `Cat` instance가 사용할 수 있는지 비교 > class `Dog` instance 선언

```python
class Dog:
    name = ""
    age = 0
    dog_type =  ""
    address = ""
    
dog1 = Dog()
dog1.name = "구슬이"
dog1.age = 6
dog1.dog_type = "시고르자브종"
dog1.address = "별내별가람"
```

```python
show_cat_info(cat1) # 정상 함수 call
show_cat_info(dog1) # AttriuteError
```

```python
이름 : 룰루, 나이 : 2, 품종 : 스코티시 폴드, 주인 : 이미영

---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
Input In [7], in <cell line: 2>()
      1 show_cat_info(cat1) # 정상 함수 call
----> 2 show_cat_info(dog1)

Input In [5], in show_cat_info(cat)
      2 def show_cat_info(cat):
      3     print("이름 : %s, 나이 : %s, 품종 : %s, 주인 : %s" %
----> 4       (cat.name, cat.age, cat.cat_type, cat.owner))

AttributeError: 'Dog' object has no attribute 'cat_type'
```

<br>

### 1.4. Dynamic variable vs. Static variable

\> Programming language를 variable type으로 나눌 때

1. 동적 변수 언어 (Dynamic variable)   
: 변수 자료형을 정하지 않아서, 넣고 싶은 자료형을 자유롭게 넣을 수 있음

2. 정적 변수 언어 (Static variable)   
: 변수 자료형을 정해서 선언하면, 그 이후로는 선언한 자료형으로만 저장 가능

\> Python은 **동적 변수 언어**에 해당한다

<br>

### 1.5. class 외부에서 선언 한 function > class 전용 method로 사용 불가능

- 함수 내부에서 `obj.[variable name]`을 조회 > 변수명 기준 > 사용 가능 함수인지 check

- 같은 variable identifier지만 다른 data type을 가진 object.var   
\> Python은 **Dynamic variable** > 자료형 상관X, 변수명 같아서 함수 사용 가능

```python
class Car:
    name = "붕붕이"
    price = "4000원" # Car obj.price data type = str
    owner_id = "EthanJ" # Car obj.owner_id data type = str
    
class Bag:
    name = "My bag" 
    price = 10000 # Bag obj.price data type = int
    owner_id = 1 # Bag obj.owner_id data type = int
    
my_car = Car()
my_bag = Bag()

def show_obj_info(obj):
    print("%s, %s, %s" %(obj.name, obj.price, obj.owner_id))


show_obj_info(my_car)
show_obj_info(my_bag)
```

```text
붕붕이, 4000원, EthanJ
My bag, 10000, 1
```

- 다른 class의 instance들, 각 instance의 data type이 다른 variable들(== `obj.var`)     

\> 같은 함수 사용 가능 > programming 중 오해의 소지 다분함 > "구조체 전용 함수" 필요

<br>

## 2. class

- 특정 구조체만 사용 할 전용 함수를 외부에 선언할 필요가 있는가?   
\> 특정 구조체에서만 사용할 함수를 외부에 선언 > 혼란 야기

<br>

- 클래스 = 변수 + 함수를 같은 소속(class)으로 선언

- 클래스 내부에 선언된 함수 == `method` == 해당 class만 사용 가능한 전용 함수

<br>

### 2.1. class method

- method는 대부분 `self` keyword를 parameter로 선언 > instance 자신을 의미    
\> 실제 method 사용시에는 `self` 입력 생략

- 클래스 내부의 변수를 지칭 할 때도 `self` 사용 > `self.var` 

```python
class Laptop:
    n_cpu_core = 0
    ram_capacity = 0
    won_price = 0
    
    def get_spec(self):
        print("%s 코어, %sGB RAM, %s원" %(self.n_cpu_core,
                                       self.ram_capacity, self.won_price))

my_laptop = Laptop()

my_laptop.n_cpu_core = 8
my_laptop.ram_capacity = 16
my_laptop.won_price = 900000

my_laptop.get_spec()
```

```text
8 코어, 16GB RAM, 900000원
```

<br>

### 2.2. `self` keyword

- `self`는 클래스로 생성한 객체(== instance, object) 자신의 주소를 나타낸다.

- 자신의 주소를 나타내야 하는 이유: 같은 class의 인스턴스들 >    
양식은 같지만 내용물은 독립적으로 저장되기 때문.

<br>

- Computer 객체 생성, cpu, ram, ssd에 임의의 값 지정, method 호출로 console에 정보 출력

```python
class Computer():
    cpu = "cpu"
    ram = 0
    ssd = 0
    
    def get_info(self):
        print("CPU = {cpu}, RAM = {ram}GB, SSD = {ssd}GB".format(cpu = self.cpu,
                                                                 ram = self.ram, ssd = self.ssd))
    def get_double_info(self, com):
        print("CPU = {cpu}, RAM = {ram}GB, SSD = {ssd}GB".format(cpu = self.cpu,
                                                                 ram = self.ram, ssd = self.ssd)
             ,"\nCPU = {cpu}, RAM = {ram}GB, SSD = {ssd}GB".format(cpu = com.cpu,
                                                                 ram = com.ram, ssd = com.ssd))
    def get_self_info(self):
        print(self, type(self))
    
    
com1 = Computer()
com1.cpu = "Ryzen7 4000series"
com1.ram = 16
com1.ssd = 512

com1.get_info()
```

```text
CPU = Ryzen7 4000series, RAM = 16GB, SSD = 512GB
```

- 함수 선언시 method에 2개의 parameter `def [method](self, obj)`   
\> method 사용: `self.[method](obj)`    
\>`self.var` == 호출 obj의 variable, `obj.var` == param obj의 variable

```python
com2 = Computer()
com2.cpu = "intelCorei7"
com2.ram = 32
com2.ssd = 256

com1.get_double_info(com2)
```

```text
CPU = Ryzen7 4000series, RAM = 16GB, SSD = 512GB 
CPU = intelCorei7, RAM = 32GB, SSD = 256GB
```

<br>

```python
com1.get_self_info()
```

```text
<__main__.Computer object at 0x000002149431AC10> <class '__main__.Computer'>
```

| stack               | heap                                                                                                                                      |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| Computer<br>(== 50) | (addr == 50)<br>blueprint                                                                                                                  |
| com1<br>(== 100)    | (addr == 100)<br>cpu = "Ryzen7 4000series"<br>ram = 16<br>ssd = 500<br>get_info(self)<br>get_double_info(self, com)<br>get_self_info(self) |
| com2<br>(== 150)    | (addr == 150)<br>cpu = "IntelCorei7"<br>ram = 32<br>ssd = 256<br>get_info(self)<br>get_double_info(self, com)<br>get_self_info(self)      |

<br>

- 멤버를 편하게 초기화시킬 수 있도록 새로운 클래스 정의.

```python
class Teacher:
    name = ""
    subject = ""
    age = 0
    
    def save_tchr_info(self, name, subject, age):
        self.name = name
        self.subject = subject
        self.age = age
        
    def print_info(self):
        print("{}'s {} class. {} is {}".format(self.name, self.subject, self.name, self.age))

tchr1 = Teacher()
tchr1.print_info()

tchr1.save_tchr_info("EthanJ", "CS", 25)
tchr1.print_info()
```

```text
's  class.  is 0
EthanJ's CS class. EthanJ is 25
```

| stack                                                                                                           | heap                                                                                                                                                                                  |
|-----------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| class<br>Teacher<br>(addr == 100)                                                                               | (addr == 100)<br>class Teacher component blueprint<br>:member variable, method                                                                                                        |
| tchr1<br>(addr == 200)                                                                                          | (addr == 200)<br>name = ""<br>subject = ""<br>age = 0<br>save_tchr_info(self, name, subject, age)<br>print_info(self)                                                                 |
| tchr1.save_tchr_info(<br>(self = 200),<br>name = "Ethanj",<br>subject = "CS",<br>age = 25)<br><br>(addr == 200) | (addr == 200)<br>name = ""<br>subject = ""<br>age = 0<br>method save_tchr_info(...) execute<br>: self.name = "Ethanj"<br>  self.subject = "CS"<br>  self.age = 25<br>print_info(self) |
| tchr1<br>(addr == 200)                                                                                           | (addr == 200)<br>name = "EthanJ"<br>subject = "CS"<br>age = 25                                                                                                                        |

<br>

### 2.3. 생성자 함수

- 클래스 내부에 `__init__` 키워드로 선언한 함수: 생성자 함수

- 생성자 함수 정의 > user가 class instance를 생성할 때 입력해야 하는 요소를 강제화

- 생성자 함수로 instance 생성 시: must be 자동으로, 1번만 호출됨!

```python
class BlackCat:
    name = " "
    weight = 0
    color = " "
    gender = " "
    
    def __init__(self, name, weight, color, gender):
        self.name = name
        self.weight = weight
        self.color = color
        self.gender = gender
```

```python
b_cat1 = BlackCat("meow", 4.5, "black", "male")
b_cat2 = BlackCat() # TypeError
```

```python
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Input In [15], in <cell line: 2>()
      1 b_cat1 = BlackCat("meow", 4.5, "black", "male")
----> 2 b_cat2 = BlackCat()

TypeError: __init__() missing 4 required positional arguments: 'name', 'weight', 'color', and 'gender'
```

- 생성자 함수 정의할 때 parameter 할당하면     
\> instance 생성 시 해당 **argument value** 반드시 넘겨줘야 함!

```python
class MyCat:
    name = " "
    weight = 0
    color = " "
    gender = " "
    
    def __init__(self, name, weight, color, gender):
        self.name = name
        self.weight = weight
        self.color = color
        self.gender = gender
        
    def get_info(self):
        print(f"name = {self.name}, weight = {self.weight}kg, color = {self.color}, gender = {self.gender}")
        
    def meow(self):
        print("야옹야옹")
        
my_cat = MyCat("조림", 4.5, "고등어", "없음")

my_cat.get_info()

my_cat.meow()
```

```text
name = 조림, weight = 4.5kg, color = 고등어, gender = 없음
야옹야옹
```