---
layout: post
title: Effective python - Chapter 1 (Better way 3)
date: 2022-08-22 11:59:59 +0900
categories: python
permalink: /15
---

# 원본 글: 파이썬 코딩의 기술 개정2판

# Effective python - Chapter 1 파이썬 답게 생각하기 

## Better way 3 ```bytes``` 와 ```str``` 의 차이를 알아두라

파이썬에는 문자열 데이터의 시퀀스를 표현하는 두 가지 타입이 있다. 바로 ```bytes```  와 ```str``` 이다. <br> 
```bytes``` 타입의 인스턴스에는 부호가 없는 8바이트 데이터가 그대로 들어간다(종종 아스키 인코딩을 사용해 내부 문자를 표시한다). <br>
```python
a = b'h\x6511o'
print(list(a))
print(a)

>>>
[104, 101, 108, 108, 111]
b'hello'
```

```str``` 인스턴스에는 사람이 사용하는 언어의 문자를 표현하는 유니코드 **코드 포인트**(code point) 가 들어 있다. <br>
```python
a = 'a\u0300 propos'
print(list(a))
print(a)

>>>
['a', '`', ' ', 'p', 'r', 'o' ,'p', 'o', 's']
à propos
```

중요한 사실은 ```str``` 인스턴스에는 직접 대응하는 이진 인코딩이 없고 ```bytes``` 에 는 직접 대응하는 텍스트 인코딩이 없다는 점이다. <br>
유니코드 데이터를 이진 데이터로 변환하려면 ```str``` 의 ```encode``` 메서드를 호출해야하고, 이진 데이터를 유니코드 데이터로 변환하려면 ```bytes``` 의 ```decode``` 메서드를 호출해야 한다. <br>
두 메서드를 호출할 때 여러분이 원하는 인코딩 방식을 명시적으로 지정할 수도 있고, 시스템 디폴트 인코딩을 받아들일 수도 있다. <br>
일반적으로는 UTF-8 이 시스템 디폴트 인코딩 방식이다(항상UTF-8 이 디폴트인 것은 아니다) <br>

파이썬 프로그램을 작성할 때 유니코드 데이터를 인코딩하거나 디코딩하는 부분을 인터페이스의 가장 먼 경계 지점에 위치시켜라 <br>
이런 방식을 **유니코드 샌드위치** 라고 부른다. <br>
프로그램의 핵심 부분은 유니코드 데이터가 들어 있는 ```str``` 을 사용해야 하고, 문자 인코딩에 대해 어떠한 가정도 해서는 안 된다. <br>
이런 접근 방식을 사용하면 다양한 텍스트 인코딩으로 입력 데이터를 받아들일 수 있고, 출력 테스트 인코딩은 한 가지로(UTF-8이 이상적이다) 엄격히 제한할 수 있다. <br>

![](/public/img/2022-08-22-Effectivepython-Chapter1(Betterway3)/1.png){: width="50%" height="50%"}{: .center} <br>
## [사진 출처](https://nedbatchelder.com/text/unipain.html) <br>

문자를 표현하는 타입이 둘로 나뉘어 있기 때문에 파이썬 코드에서는 다음과 같은 두 가지 상황이 자주 발생한다. <br>
-UTF-8(또는 다른 인코딩 방식)로 인코딩된 8비트 시퀀스를 그대로 사용하고 싶다. <br>
-특정 인코딩을 지정하지 않은 유니코드 문자열을 사용하고 싶다. <br>

두 경우를 변환해주고 입력 값이 코드가 원하는 값과 일치하는지 확신하기 위해 종종 두 가지 도우미 함수가 필요하다. <br>
첫 번째 함수는 ```bytes``` 나 ```str``` 인스턴스를 받아서 항상 ```str``` 을 반환한다. <br>
```python
def to_str(bytes_or_str):
    if isinstance(bytes_or_str, bytes):
        value = bytes_or_str.decode('utf-8')
    else:
        value = bytes_or_str
    return value    # str 인스턴스

print(repr(to_str(b'foo')))
print(repr(to_str('bar')))
print(repr(to_str(b'xed\x95\x9c')))

>>>
'foo'
'bar'
'한'
```

두 번째 함수는 ```bytes``` 나 ```str``` 인스턴스를 받아서 항상 ```bytes``` 를 반환한다. <br>
```python
def to_bytes(bytes_or_str):
    if isinstance(bytes_or_str, str):
        value = bytes_or_str.encode('utf-8')
    else:
        value = bytes_or_str
    return value    # bytes 인스턴스

print(repr(to_str(b'foo')))
print(repr(to_str('bar')))
print(repr(to_str('한글')))

>>>
b'foo'
b'bar'
b'xed\x95\x9c\xea\xb8\x80'
```

이진 8비트 값과 유니코드 문자열을 파이썬에서 다룰 때 꼭 기억해야 할 두 가지 문제점이 있다. <br>

첫 번째 문제점은 ```bytes```와 ```str```이 똑같이 동작하는 것처럼 보이지만 각각의 인스턴스는 서로 호환되지 않기 때문에 전달 중인 문자 시퀀스가 어떤 타입인지를 항상 잘 알고 있어야 한다는 점이다.

```python
print(b'foo' == 'foo')

>>>
False
```
```python
print('red %s' % b'blue')
print(b'red %s' % 'blue')

>>>
red b'blue'
Traceback ...
TypeError: ...
```

두 번째 문제점은 (내장 함수인 ```open```을 호출해 얻은) 파일 핸들과 관련한 연산들이 디폴트로 유니코드 문자열을 요구하고 이진 바이트 문자열을 요구하지 않는다는 것이다. <br>
이로 인해 코드가 실행되지 않아 놀랄 수도 있다 <br>

```python
with open('data,bin', 'w') as f:
    f.write(b'\xf1\xf2\xf3\xf4\xf5')

>>>
Traceback ...
TypeError: ...
```

예외가 발생한 이유는 파일을 열 때 이진 쓰기 모드```('wb')``` 가 아닌 텍스트 쓰기 모드```('w')```로 열었기 때문이다. <br>
파일이 텍스트 모드인 경우 ```write``` 연산은 이진 데이터가 들어 있는 ```bytse``` 인스턴스가 아니라 유니코드 데이터가 들어있는 ```str``` 인스턴스를 요구한다. <br>
이 문제는 ```'wb'``` 모드를 사용해 파일을 열면 해결할 수 있다. <br>

파일에서 데이터를 읽을 때도 비슷한 문제가 발생할 수 있다. <br>

```python
with open('data.bin', 'r') as f:
    data = f.read()

>>>
Traceback ...
UnicodeDecodeError: ...
```

```python
with open('data.bin', 'rb') as f:
    data = f.read()

assert data == b'\xf1\xf2\xf3\xf4\xf5'
```

다른 방법으로 , ```open``` 함수의 ```encoding``` 파라미터를 명시하면 플랫폼에 따라 동작이 달라져서 놀라는 일을 막을 수 있다. <br>
예를 들어 다음 코드에는 파일에 들어있는 이진 데이터가 실제로 ```cp1252```로 돼 있다고 가정한다. <br>

```python
with open('data.bin', 'r', encoding='cp1252') as f:
    data = f.read()
```

여러분이 예상하는 것과 시스템의 디폴트 인코딩이 어떻게 다른지 이해하기 위해 시스템 인코딩을 항상 검사해야 한다는 것이다. <br>
> ```python3 -c 'import locale; print(locale.getpreferredencoding())'```

