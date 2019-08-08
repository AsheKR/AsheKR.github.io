---
layout: post
title: "DesignPatterns - Command"
date: 2019-08-08 10:41:46
author: Ashe
categories:
- Python
- DesignPatterns
---

# Command

실행될 기능을 캡슐화함으로 여러 기능을 실행할 수 있는 재사용성 높은 클래스를 설계하는 패턴

<!--more-->

## 구성 요소

- Command
    - 실행될 기능에 대한 인터페이스
- ConcreteCommand
    - 실제로 실행되는 기능을 구현
    - exectue에서 Receiver가 실행해야할 명령어를 호출한다.
- Invoker
    - ConcreteCommand를 가지며 ConcreteCommand.execute()를 실행한다.
- Receiver
    - Invoker에 의해 Receiver가 ConcreteCommand를 호출하게된다.

순서가 참 어렵다. 다음과 같이 호출된다.

```python
Invoker(Command(Reciver))

# 1. Invoker가 Command의 Execute를 실행시킴
# 2. Command가 Receiver의 무언가를 실행시킴
```

## GoF 패턴 범주

Behavior 패턴

- 객체나 클래스 사이 알고리즘이나 책임 분배에 관련된 패턴
- 한 객체가 혼자 수행할 수 없는 작업을 여러개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는 것에 중점을 둔다.

## 장점

1. 요청과 수행을 분리하기때문에 결합도가 낮아지며 각 객체가 수정되어도 영향을 받지 않는다.
2. 호출하는곳을 변경하지 않고 재사용하기 좋다.

## 단점

1. Receiver가 많은 기능을 요구할경우 그 동작에 대한 클래스를 만들어야한다. 복잡도가 올라갈 수 있다.

## 언제 사용해야하나?

1. 빈번한 수정, 추가확장의 여지가 있을 때 사용해야한다.
2. 작업 큐: 커맨드를 받아 스레드는 큐로부터 하나씩 execute()만 실행하면 된다.
3. 작업 취소, 로그, 트랜잭션 시스템 구현에도 활용된다.
    - execute 때 받은 Command를 저장해두면 하나씩 되돌릴 수 있다.

## 구현

인터넷에서 주문을 할 때 장바구니 담기, 구매, 판매같이 여러 기능이 있을 수 있고 추가확장 수정의 여지가 있다.

```python

# Command
class Command:
    def execute(self):
        raise NotImplementedError


# ConcreteCommand
class StoreCommand(Command):

    def __init__(self, order, item):
        self.order = order
        self.item = item

    def execute(self):
        self.order.store(self.item)

# Invoker
class OrderButton:
    def __init__(self, command):
        self.command = command

    def pressed(self):
        self.command.execute()

# Receiver
class Order:
    _basket = []

    def store(self, item):
        self._basket.append(item)

if __name__ == '__main__':
    order1 = Order()

    # 장바구니 넣기
    OrderButton(StoreCommand(order1, item)).pressed()
```

장바구니 넣기 이외에 추가로 기능을 만들고싶다면 호출코드를 변경하지 않고 만들 수 있다.

```python
class BuyCommand(Command):

    def __init__(self, order):
        self.order = order

    def execute(self):
        self.order.buy()

class Order:
    # ... 위에 내용을 포함한다고 하자

    def buy(self):
        print(self._basket, '구매완료!')
        self._basket = []

if __name__ == '__main__':
    order1 = Order()

    # 장바구니 넣기
    OrderButton(StoreCommand(order1, item)).pressed()
    OrderButton(BuyCommand(order1)).pressed()
```

진정한 꽃은 이렇게 명령어를 늘리는게 아니라 호출코드에 공통작업이 있을 때인것같다.

커맨드가 무수히 늘어나더라도 호출코드는 변경되지 않고 호출될때마다 호출자 클래스속성에 넣어두고 Undo나 History도 개발할 수 있다.

---

8월 9일 추가

## References

- https://gmlwjd9405.github.io/2018/07/07/command-pattern.html

- http://ufx.kr/blog/330

