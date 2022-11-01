# Python OOP Inheritance and Overriding <br> 파이썬 객체 지향 프로그래밍 상속과 오버라이딩

## 1. 상속

: 부모 class의 정보를 활용해 자식 class를 생성하는 것

- 문법

```pythoon
class [class 이름](부모 class):
    <class 내부 정의>
    ...
```

- 상속 시 부모 class의 item(variable, method)을 자식 class에 물려준다.

- 상속은 자식 class 선언 시 부모 class명을 지목해서 실행된다

- 자식 class는 새로운 변수, method를 선언 해 사용할 수 있다.

<br>

```python
class Person:
    name = ""
    age = 0
    height = 0
    
    def get_info(self):
        print("이름 : %s, 나이 : %s살, 키 : %scm" 
              %(self.name, self.age, self.height))
```

```python
p1 = Person()
p1.name = "EthanJ"
p1.age = 25
p1.height = 178

p1.get_info()
```

```text
이름 : EthanJ, 나이 : 25살, 키 : 178cm
```

```python
print(p1)
```

```text
<__main__.Person object at 0x000002A1F43CB670>
```

<br>

- `Person` class를 상속받은 자식class `Student` 선언

```python
class Student(Person):
    major = ""

my_stu = Student()

my_stu.name = "김파이썬"
my_stu.age = 21
my_stu.height = 171
my_stu.major = "Computer Science"

print(f"이름: {my_stu.name}, 나이: {my_stu.age}살, 키: {my_stu.height}, 전공: {my_stu.major}")
```

```text
이름: 김파이썬, 나이: 21살, 키: 171, 전공: Computer Science
```

<br>

- 상속받은 자식class에서 부모class의 method 사용 가능

```python
my_stu.get_info()
```

```text
이름 : 김파이썬, 나이 : 21살, 키 : 171cm
```

<br>

- `Student` class를 상속받은 `SalaryMan` class 선언

```python
class SalaryMan(Person):
    salary = 0

my_salary = SalaryMan()

my_salary.name = "김자바"
my_salary.age = 28
my_salary.height = 169
my_salary.salary = 60000000

print("이름: {}, 나이: {}살, 키: {}cm, 연봉: {}원".format(my_salary.name, my_salary.age,
                                                my_salary.height, my_salary.salary))
```

```text
이름: 김자바, 나이: 28살, 키: 169cm, 연봉: 60000000원
```

<br>

## 2. 오버라이딩 (Overriding)

: 부모class에서 상속된 method를 자식class에서 재정의해서 사용하는 것

- 부모class에 이미 할당된 method    
\> 자식class에서 추가된 사항 반영 필요 등의 이슈 발생    
\> 자식class에서 상속된 method를 현재 class에 맞춰 변형해 사용

- 사용법: 자식class 선언 할 때, 부모class의 이미 존재하는 method와   
같은 `method이름`, `parameter`사용해서 method 정의

<br>

- `Airplane` class 생성

```python
class Airplane:
    velocity = 0
    gas = 0
    flight_number = ""
    
    # 생성자
    def __init__(self, velocity, gas, flight_number):
        self.velocity = velocity
        self.gas = gas
        self.flight_number = flight_number
        
    # fly(self) method
    def fly(self):
        # No gas
        if(self.gas <= 0):
            print("No gas.")
            
            return
        
        # Max v = 1000
        if(self.velocity >= 1000):
            self.velocity = 1000
            
        # up v, down gas
        else :
            self.velocity += 100
            self.gas -= 100
        
    def break_(self):
        if(self.velocity < 300):
            self.velocity = 0
        else:
            self.velocity -= 100
        
    def get_info(self):
        print(f"현재 속도 : {self.velocity}km/h, 현재 연료량 : {self.gas}L, 편명 : {self.flight_number}")
```

```python
airplane_1 = Airplane(0, 3000, "K747")

airplane_1.get_info()
```

```text
현재 속도 : 0km/h, 현재 연료량 : 3000L, 편명 : K747
```

<br>

```python
airplane_1.fly()

airplane_1.get_info()
```

```
현재 속도 : 100km/h, 현재 연료량 : 2900L, 편명 : K747
```

<br>

```python
airplane_1.break_()

airplane_1.get_info()
```

```text
현재 속도 : 0km/h, 현재 연료량 : 2900L, 편명 : K747
```

<br>

- class `Airplane`을 상속받은 class `PrivateJet` 선언

- Overriding method `fly()` > 같은 method명, parameter

```python
class PrivateJet(Airplane):
    
    # Overriding method fly()
    def fly(self):
        # No gas
        if(self.gas <= 0):
            print("No gas.")
            
            return
        
        # Max v = 2000
        if(self.velocity >= 2000):
            self.velocity = 2000
            
        # up v, down gas
        else :
            self.velocity += 300
            self.gas -= 300
```

```python
airplane_2 = Airplane(0, 5000, "KF94")

for i in range(10):
    airplane_2.fly()
    
airplane_2.get_info()
```

```text
현재 속도 : 1000km/h, 현재 연료량 : 4000L, 편명 : KF94
```

```python
my_jet = PrivateJet(0, 5000, "MJ98")

for i in range(10):
    my_jet.fly()
    
my_jet.get_info()
```

```text
현재 속도 : 2000km/h, 현재 연료량 : 2900L, 편명 : MJ98
```
