---
layout: post
title: python property
date: 2022-08-10 11:59:59 +0900
categories: Python
permalink: /3
---

# python property

## 원본 글: [https://blog.naver.com/codeitofficial](https://blog.naver.com/codeitofficial/221684462326)

```python
class Citizen:
    def __init__(self, age):
        self._age = age

    def get_age(self):
        return self.age

    def set_age(self, age):
        self._age = age

citizen = Citizen(20)
print(citizen.get_age())
citizen.set_age(25)
print(citizen.get_age())
```

```python
20
25
```
---
@property 적용
```python
class Citizen:
    def__init_(self, age):
        self._age = age

    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, age):
        self._age = age

citizen = Citizen(20)
print(citizen.age)
citizen.age=25
print(citizen.age)
```

```python
20
25
```

---
유용한 상황

``` python
class Citizen:
    age_thrshold = 19
    def __init__(self, name, age):
```
```
# ...
c1 = Citizen("Tom", 20)
c2 = Citizen("Mike", 10)
c3 = Citizen("Sally", 23)

#...
if c1.age >= Citizen.age_threshold :
	print(True, c1.name, c1.age)
else :
	print(False, c1.name, c1.age) 

if c2.age >= Citizen.age_threshold :
	print(True, c2.name, c2.age)
else :
	print(False, c2.name, c2.age) 

if c3.age >= Citizen.age_threshold :
    print(True, c3.name, c3.age)
else :
	print(False, c3.name, c3.age) 
#...
```
@property 사용
```python
class Citizen:
	age_threshold = 19
    def __init__(self, name, agee):
		self.name = name
        self._age = age

    @property
    def age(self):
        if(self._age >= Citizen.age_threshold):
            print(True)
        else:
            print(False)
        return self._age
# ...
c1 = Citizen("Tom", 20)
c2 = Citizen("Mike", 10)
c3 = Citizen("Sally", 23)
#...
print(c1.name, c1.age)
print(c2.name, c2.age)
print(c3.name, c3.age)
#...
```
---
```@***.deleter``` 도 존재. <br/>
```__get__```, ```__set__```, ```__delete__```, 와 같이 magic method 로 만들어주는 용도? <br/>
getter, setter 함수 없이 더 용이하게 변수 접근에 제한을 둘 수 있다.
