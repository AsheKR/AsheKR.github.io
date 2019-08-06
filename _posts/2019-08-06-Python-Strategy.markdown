---
layout: post
title: "DesignPatterns - Strategy"
date: 2019-08-05 00:41:46
author: Ashe
categories:
- Python
- DesignPatterns
---

# Strategy

목적은 하나인데 달성할 수 있는 방법이 여러가지일 경우 목적을 이루기위한 동작을 캡슐화하여 전략적으로 목적을 이루는 방식을 교체사용 가능하게하는 디자인 패턴

<!--more-->

## 구성 요소

- Strategy
    - 인터페이스나 추상 클래스로 외부에서 동일한 방식으로 알고리즘을 호출하는 방법을 명시
- ConcreteStrategy
    - Strategy 패턴에서 명시한 알고리즘을 실제 구현한 클래스
- Context
    - ConcreteStrategy를 이용하는 역할을 수행한다.
    - 필요에 따라 동적으로 구체적 전략을 바꿀 수 있도록 setter_method('집약 관계')를 제공한다.

## GoF 패턴 범주

Behavior 패턴

- 객체나 클래스 사이 알고리즘이나 책임 분배에 관련된 패턴
- 한 객체가 혼자 수행할 수 없는 작업을 여러개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는 것에 중점을 둔다.

집약 관계

- 참조값을 인자로 받아 필드를 세팅하는 경우
- 전체 객체의 라이프타임과 부분 객체의 라이프타임은 독립적이다.
- 즉, 전체 객체가 메모리에서 사라진다 해도 부분 객체는 사라지지 않는다.

## 장점

1. Context, Strategy을 서로 독립적으로 분리시켜주기 때문에 이해하기 쉽고, 유지보수, 확장이 쉬우며 동적으로 전략을 변경할 수 있다.

## 단점

1. Startegy 하위 클래스에 대해 서로간의 차이를 알아야하는데, 모두 알려면 부담스러운 일이다.
2. 데이터참조를 위해 Context, Strategy는 연관관계를 가져야하는데 이는 결합도가 올라간다.

## 언제 사용해야하나?

1. 상황에 따라 최적의 알고리즘을 선택하여 사용할 때 유용하다.
2. Client가 알아서는 안되는 데이터를 다룰 때 유용하다. 데이터를 가지고있는 곳(Context) 실제 행동을 주도하는곳(ConcreteStrategy)는 서로 다른 클래스에 정의되어있기 때문에 데이터를 가지고있는 곳(Context)에서 필요한 데이터만 행동을 주도하는곳(ConcreteStrategy)에게 보내줄 수 있다.
3. 프로그램 실행 중 행동을 바꿀 수 있다.

## 구현

게임 캐릭터의 공격을 디자인한다고 가정한다.

```python
class Character:
    def attack(self):
        raise NotImplementedError

class Garen(Character):
    def attack(self):
        print("칼 공격")

class Ashe(Character):
    def attack(self):
        print("활 공격")
```

### 문제점

1. 기존 공격방법을 수정하는 경우
    - Garen이 활을 들고 공격하고싶다면 기존 코드의 내용을 수정해야한다.
    - Ashe와 메서드 내용이 중복된다. 만약 활든 공격이 새 방식이 추가되거나 수정되면 모든 활 공격의 내용을 수정해주어야한다.

### 해결책

문제를 해결하기 위해 무엇이 변화되었는지 찾은 후 이를 클래스로 캡슐화해야한다.

- 문제 발생 요인은 공격방식의 변화이다.
- 캡슐화를하려면 구체적 공격방식을 클래스화해야한다.
    - Character 클래스가 Context가 된다.
    - attack 메서드를 AttackStrategy 클래스를 사용한 인터페이스로 캡슐화한다.
    - 공격방식의 변화를 위해 setter 메서드가 필요하다.

```python
class Character:
    def __init__(self, attack_strategy):
        self._attack_strategy = attack_strategy

    def attack(self):
        self._attack_strategy.attack()

class Garen(Character):
    def __init__(self):
        super().__init__(SwordAttackStrategy)

class Ashe(Character):
    def __init__(self):
        super().__init__(SwordAttackStrategy)

# ---

class AttackStrategy:
    def attack(self):
        raise NotImplementedError

class ArrowAttackStrategy(AttackStrategy):
    def attack(self):
        print("활 공격")

class SwordAttackStrategy(AttackStrategy):
    def attack(self):
        print("검 공격")

if __name__ == '__main__':
    ashe = Ashe()
    ashe.attack()
    # 활 공격

    garen = Garen()
    garen.attack()
    # 검 공격
```

## References

- https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html

- https://kimsunzun.tistory.com/category/1.%20%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4/Behavioral%20%EC%A0%95%EB%A6%AC

