---
layout: post
title: "DesignPatterns - Singleton"
date: 2019-08-05 00:41:46
author: Ashe
categories:
- Python
- DesignPatterns
---

# Singleton

단 하나의 인스턴스를 생성해 사용하는 디자인 패턴

<!--more-->

## GoF 패턴 범주

Creational 패턴

- 객체 생성과 관련된 패턴
- 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 구조에 영향을 크게 받지 않도록 유연성을 제공한다.

## 장점

1. 하나의 인스턴를 사용하기 때문에 메모리 낭비를 방지할 수 있음
2. 전역 인스턴스이기때문에 데이터를 공유하기 쉽다.


## 단점

1. 싱글톤 인스턴스에서 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우 다른 클래스의 인스턴스든간의 결합도가 높아셔 OCP를 위반하게 된다. 따라서 수정과 테스트가 어렵게 된다.
2. 멀티스레드 환경에서 동기화처리가 안되면 인스턴스 한개를 보장할 수 없게된다.

## 언제 사용해야하나?

[영문 문서](https://testing.googleblog.com/2008/08/root-cause-of-singletons.html)

## 구현

[Python에서 Singleton을 구현하는 방법](https://stackoverflow.com/questions/6760685/creating-a-singleton-in-python)

싱글톤을 구현할 때 어떤 방법이 가장 `파이썬스럽게` 구현했는지의 논의를 위한 질문이다.

---

### Method 1: A decorator

```python
def singleton(class_):
    instances = {}
    def getinstance(*args, **kwargs):
        if class_ not in instances:
            instances[class_] = class_(*args, **kwargs)
        return instances[class_]
    return getinstance

@singleton
class MyClass:
    pass
```

#### 장점

데코레이터는 다중상속보다 보다 직관적으로 알 수 있다.

#### 단점

- `MyClass()`를 호출할경우 Sigleton Object를 리턴하지만 `MyClass`를 호출할경우 함수가 호출된다.

```python
MyClass()

# <__main__.MyClass at 0x10829a128>

MyClass

# <function __main__.singleton.<locals>.getinstance(*args, **kwargs)>

```


- 

```python
m = MyClass()
n = MyClass()
o = type(n)()

# m == n
# m != o
# n != o
```

### Method 2: A base class

```python
class Singleton:
    _instance = {}
    def __new__(class_, *args, **kwargs):
        if class_ not in class_._instances:
            class_._instances[class_] = super(Singleton, class_).__new__(class_, *args, **kwargs)
        return class_._instance

class MyClass(Singleton):
    pass
```

#### 장점

- 데코레이터와달리 어떻게 호출해도 클래스이다.

#### 단점

- 다중상속일때 오른쪽 클래스에 의해 덮어씌여질 수 있는 가능성이 있다. 필요 이상으로 생각해야한다.

### Method 3: A metaclass

```python
class Singleton:
    _instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__class__(*args, **kwargs)
        return cls._instances[cls]

class MyClass(metaclass=Singleton):
    pass
```

#### 장점

- 어떻게 호출해도 클래스이다.
- 마법처럼 자동으로 다중상속도 커버한다.
- 적합한 목적으로 `__metaclass__`를 사용한다.

#### 단점

뭔가 있나?

### Method 4: Decorator returning a class with the same name

```python
def singleton(class_):
    class class_w(class_):
        _instance = {}

        def __new__(class_, *args, **kwargs):
            if class_ not in _instance:
                class_w._instance[class_] = super(class_w, class_).__new__(class_, *args, **kwargs):
                class_w._instance._sealed = False
            return class_w._instance
        
        def __init__(self, *args, **kwargs):
            if self._sealed:
                return
            super(class_w, self).__init__(*args, **kwargs)
            self._sealed = True
        
    class_w.__name__ = class_.__name__
    return class_w

@singleton
class MyClass:
    pass
```

#### 장점

- 어떻게 호출해도 클래스이다.
- 자동으로 마법처럼 다중상속도 커버한다.

#### 단점

- Is there not an overhead for creating each new class? Here we are creating two classes for each class we wish to make a singleton. While this is fine in my case, I worry that this might not scale. Of course there is a matter of debate as to whether it aught to be too easy to scale this pattern...

- What is the point of the _sealed attribute

- Can't call methods of the same name on base classes using super() because they will recurse. This means you can't customize __new__ and can't subclass a class that needs you to call up to __init__.

---

### Answer

**Method #2**번과 **metaclass**를 사용하는것을 권장한다.

```python
class Singleton(type):
    _instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__class__(*args, **kwargs)
        return cls._instances[cls]

class Logger(metaclass=Singleton):
    pass
```

`__init__`이 매 클래스 호출에 추가되게 하고싶다면 `Singleton.__call__`의 `if` 문장 뒤에 추가한다.

```python
        else:
            cls._instances[cls].__init__(*args, **kwargs)
```

메타클래스에 대해 몇마디 추가하자면, 메타클래스는 **클래스의 클래스**이고, 클래스는 **메타클래스의 인스턴스**이다.

`type(obj)`를 해보면 metaclass를 찾을 수 있다. 일반적인 새로운 스타일의 클래스 타입은 `type`이다.

위 코드의 `Logger`는 `your_moduel.Singleton` 타입이 될것이다. `Logger`의 인스턴스는 `your_module.Logger` 클래스 타입이 된다.

`Logger()`를 사용하여 Logger를 호출할 때 Python은 먼저 `Logger`의 metaclass에게 요청하여 인스턴스 생성을 선점할 수 있도록한다.

이 과정은 Python이 `myclass.attribute`를 사용하여 속성 중 하나를 참조할 때 `__getattr__`을 호출하여 클래스가 무엇을해야 하는지 묻는 것과 동일하다.

메타클래스는 기본적으로 클래스 정의가 의미하는 바를 결정하고 그 정의를 구현하는 방법을 결정한다.

 하위 클래스가 `__new__` method를 정의하는 Method #2를 사용하면 `Singleton`을 상속받는 하위클래스가 **호출될때마다 실행**된다. 메타클래스의 경우, 인스턴스가 생성될 때 **단 한번만 호출**된다. You want to customize what it means to call the class, which is decided by it's type.

## References

- https://stackoverflow.com/questions/6760685/creating-a-singleton-in-python

- https://testing.googleblog.com/2008/08/root-cause-of-singletons.html

