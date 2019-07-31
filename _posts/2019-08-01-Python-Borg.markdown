---
layout: post
title: "DesignPatterns - Borg"
date: 2019-08-01 00:00:46
author: Ashe
categories:
- Python
- DesignPatterns
img: post05.jpg
---

# Borg

클래스로부터 생성되는 모든 인스턴스를 공유하고, 공유 상태에 중점을 두는 패턴이다.

<!--more-->

```python
class Borg:
    _shared_state = {}

    def __init__(self):
        self.__dict__ = self._shared_state
        self.state = 'Init'

    def __str__(self):
        return self.state

class OtherBorg(Borg):
    pass

if __name__ == '__main__':
    rm1 = Borg()
    rm2 = Borg()

    rm1.state = 'Idle'
    rm2.state = Running

    print('rm1: {0}'.format(rm1))
    print('rm2: {0}'.format(rm2))

    rm2.state = 'Zombie'

    print('rm1: {0}'.format(rm1))
    print('rm2: {0}'.format(rm2))

    rm3 = YourBorg()

    print('rm1: {0}'.format(rm1))
    print('rm2: {0}'.format(rm2))
    print('rm3: {0}'.format(rm3))
    
    rm1.other_variable = 'another one!'

    print('rm1.other_variable: {0}'.format(rm1))
    print('rm2.other_variable: {0}'.format(rm2))
    print('rm3.other_variable: {0}'.format(rm3))

### OUTPUT ###
# rm1: Running
# rm2: Running
# rm1: Zombie
# rm2: Zombie
# rm1: Init
# rm2: Init
# rm3: Init
# rm1.other_variable: another one!
# rm2.other_variable: another one!
# rm3.other_variable: another one!
```


## 장점

1. 인스턴스가 상태 생성 및 동작을 공유하도록 보장되어 쉽고 유연하게 사용할 수 있다.

## 단점

## 언제 사용해야할까?

여러 과목에서 사용되어야하는 사용자가 있다고 가정한다.

```python
class Subject(Borg):
    def __init__(self, name=None):
        Borg.__init__(self)
        if name is not None: self.name = name

    def __str__(self): 
        return self.__class__.__name__ + '과목을 ' + self.name + '이(가) 수강중입니다.'

class Math(Subject):
    pass

class Science(Subject):
    pass

if __name__ == '__main__':
    math = Math('Ashe')
    science = Science()

    print(math)
    print(science)

### OUTPUT ###
# Math과목을 Ahse이(가) 수강중입니다.
# Science과목을 Ahse이(가) 수강중입니다.
```

더 실용적인 예를 들자면 Database Connection처럼 같이 공유되고 생성되고 수정되야하는 곳에서 사용되어야한다.
