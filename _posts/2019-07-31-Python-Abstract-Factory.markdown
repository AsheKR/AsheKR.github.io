---
layout: post
title: "DesignPatterns - Abstract Factory"
date: 2019-07-31 01:52:46
author: Ashe
categories:
- Python
- DesignPatterns
img: post04.jpg
---

[Python-patterns](https://github.com/faif/python-patterns)

# Abstract Factory

추상화(Abstract) + 팩토리(Factory)로 구성된 단어이다. 추상화는 **주체와 구체적인 행동 나중에 결정하고 목적을 달성할 수 있는 추상적인 동작만을 구현하는것이다.** 추상적인 동작을 예로들자면 `쉰다.`가 있다. 누구나(다양한 주체)가 쉬는 방법(다양한 행동)은 다르겠지만 `쉰다`라는 목적은 같을것이다.

<!--more-->

내가 `쉰다.` 라는 동작을 세부적으로 나타내면 다음과 같다.
1. 눕는다.
2. 핸드폰을 켠다.
3. 유튜브를 본다.

누군가는 `쉰다.`라는 동작을 세부적으로 나타내면 다음과 같다.
1. 앉는다.
2. 명상을 한다.

```python

class Ashe:

    def rest():
        print("1. 눕는다.")
        print("2. 핸드폰을 켠다.")
        print("3. 유튜브를 본다.")


class Song:

    def rest():
        print("1. 앉는다.")
        print("1. 명상을 한다.")


def rest(person_type):
    if isinstance(person_type, Ashe):
        person = Ashe()
    elif isinstance(person_type, Song):
        person = Song()

    person.rest()


# 실행

if __name__ == "__main__":

    print("ashe의 결과")
    rest(Ashe)

    print("song의 결과")
    song.rest()

## OUTPUT ##
# me의 결과
# 1. 눕는다.
# 2. 핸드폰을 켠다.
# 3. 유튜브를 본다.
# ================
# other의 결과
# 1. 앉는다.
# 2. 명상을 한다.
```

이를 추상팩토리를 통한 코드로 나타내보자.

```python

class Person:

    def __init__(self, person):
        # 이 때 들어오는 person은 Class이다.
        self.person_factory = person

    def rest(self):
        some_person = self.person_factory()
        print("공통 동작")
        some_person.rest()


class Ashe:

    def rest():
        print("1. 눕는다.")
        print("2. 핸드폰을 켠다.")
        print("3. 유튜브를 본다.")


class Song:

    def rest():
        print("1. 앉는다.")
        print("1. 명상을 한다.")


# 실행

if __name__ == "__main__":

    ahse = Person(Ashe)
    print("ahse의 결과")
    ashe.rest()

    song = Person(Song)
    print("song의 결과")
    song.rest()

## OUTPUT ##
# ashe의 결과
# 1. 눕는다.
# 2. 핸드폰을 켠다.
# 3. 유튜브를 본다.
# ================
# song의 결과
# 1. 앉는다.
# 2. 명상을 한다.
```

## 장점

1. 추상 팩터리는 코드를 더 유연하게 사용할 수 있게 해준다. 만약 추상팩터리를 사용하지 않고 사람이 많아진다면 모든 사람에 대한 타입체크를 해주어야한다.
2. 인터페이스에 맞게 클래스를 구현해야하므로 일관성있는 클래스가 작성 가능하다.
3. 내부 구현을 알 필요가 없다. 인터페이스에 맞게 코드만 작성하면된다.
4. 제품군의 대체가 쉬워진다. 

`ashe = Person(Ashe)` 가 아니라 다른 사람으로 대체해야하는경우 쉽게 대체할 수 있다. 단순히 클래스만 다르게 주면되니까!

## 단점

1. 새로운 목적이 있을경우 확장하기 힘들다.

위의 예에서 `쉰다.` 이외에 `공부를 한다.`를 추가하려는경우 추상팩토리에 사용되는 모든 클래스에 `공부를 한다.` 메서드를 추가해야한다.

## 언제 사용해야할까?

같은 목적을 가진 서브클래스가 많을 때 유용한 패턴이다!
