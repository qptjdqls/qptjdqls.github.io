---
layout: post
title: python creational design pattern - Factory pattern
date: 2022-09-04 11:59:59 +0900
categories: python
permalink: /26
---

## [Original video](https://www.youtube.com/watch?v=AmwEIt0vhxA&list=PLDV-cCQnUlIYcAmW4j27i8aYPbja9HePm&index=2)


### Factory <br>

object 를 찍어낼 수 있는 공장. <br>

공장은 ```function``` 이나 ```class``` 로 나타낼 수 있다. <br>

클라이언트는 object 의 복잡한 생성과정을 알 필요가 없고, factory 에 요청사항만 넘겨주면 된다. <br>

이러한 개념은 Factory method, Singleton, Builder 등에서 활용된다. <br>

```python
class Animal():
    def speak(self):
        pass

class Cat(Animal):
    def speak(self):
        print("meow")

class Dog(Animal):
    def speak(self):
        print("bark")

# function version
# prefer enum
def FactoryFn(animal: str) -> Animal:
    if animal == "Cat":
        return Cat()
    elif animal == "Dog":
        return Dog()

cat = FactoryFn("Cat")
cat.speak()
dog = FactoryFn("Dog")
dog.speak()

>>> meow
>>> bark

# class version
class AnimalFactory():
    # prefer enum
    def createAnimal(self, animal: str) -> Animal:
        if animal == "Cat":
            return Cat()
        elif animal == "Dog":
            return Dog()

factory = AnimalFactory()
cat = factory.createAnimal("Cat")
dog = factory.createAnimal("Dog")

cat.speak()
dog.speak()

>>> meow
>>> bark

```

```Enum``` 을 사용한 버전 <br>

```python
from enum import Enum
class AnimalEnum(Enum):
    CAT = 1
    DOG = 2
...
def FactoryFn(animal: AnimalEnum):
    if animal == AnimalEnum.CAT:
        return Cat()
    elif animal == AnimalEnum.DOG:
        return Dog()

cat = FactoryFn(AnimalEnum.CAT)
cat.speak()
dog = FactoryFn(AnimalEnum.DOG)
dog.speak()

>>> meow
>>> bark

```
