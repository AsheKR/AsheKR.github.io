---
layout: post
title: "DesignPatterns - Builder"
date: 2019-08-02 00:00:46
author: Ashe
categories:
- Python
- DesignPatterns
img: post06.jpg
---

# Builder

Builder 패턴은 복잡한 인스턴스를 조립하듯이 만들어주고, 복잡한 객체 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현 결과를 만들 수 있게 해주는 패턴이다.

<!--more-->

이 패턴은 다음과 같은 구성요소로 이루어져있다.

- Builder : 객체를 만들기위한 부품들을 구분하여 어떻게 만들어져야겠다는 목적만 명시한 추상 인터페이스

- ConcreteBuilder : Builder의 인터페이스를 구현한다. 즉, 각 부품을 실제로 제조할 수 있는 **동작**을 구현한다.

- Director : Builder로 만들어진 ConcreteBuilder를 받아 각 부품을 **제조 동작을 호출하고 조립하여 생성한다**. (동일한 생성 절차를 가짐)

- Product : Director가 Builder로 생성한 **결과물**을 이야기한다. (각 ConcreteBuilder마다 다른 결과물을 가짐)


햄버거를 만드는 예시를 사용한다.

```python

# Builder, 햄버거의 부품들을 명시함
class HamburgerBuilder:

    def __init__(self):
        self.burger = []

    # 빵
    def make_bread(self):
        self.burger.append('빵')

    # 패티
    def make_patty(self):
        raise NotImplementedError

    # 소스
    def make_source(self):
        raise NotImplementedError

    # 내부 재료
    def make_ingredient(self):
        raise NotImplementedError
    
    def get_complete_burger(self):
        return ', '.join(self.burger)


class BulgogiBurgerBuilder(HamburgerBuilder):

    # 패티
    def make_patty(self):
        # 불고기 패티를 만드는 과정 ~
        self.burger.append('불고기');

    # 소스
    def make_source(self):
        # 소스를 만드는 과정 ~
        self.burger.append('데리야끼 소스');

    # 내부 재료
    def make_ingredient(self):
        pass


class ThighBurgerBuilder(HamburgerBuilder):

    # 패티
    def make_patty(self):
        # 닭 넓적다리 패티를 만드는 과정 ~
        self.burger.append('닭넓적다리');

    # 소스
    def make_source(self):
        # 소스를 만드는 과정 ~
        self.burger.append('레몬마요네즈');

    # 내부 재료
    def make_ingredient(self):
        # 재료를 가져오고 알맞게 만드는 과정 ~
        self.burger.append('피클');


class Director:

    def __init__(self, builder):
        self.builder = builder

    def make_burger(self):
        self.builder.make_bread()
        self.builder.make_patty()
        self.builder.make_source()
        self.builder.make_ingredient()
        self.builder.make_bread()

    def get_burger(self):
        return self.builder.get_complete_burger()

# 실행

if __name__ == "__main__":

    director = Director(BulgogiBurgerBuilder())
    print("불고기의 결과")
    director.make_burger()
    result = director.get_burger()
    print(result)

    director = Director(ThighBurgerBuilder())
    print("싸이버거의 결과")
    director.make_burger()
    result = director.get_burger()
    print(result)

## OUTPUT ##
# 불고기의 결과
# 빵, 불고기, 데리야끼 소스, 빵
# ================
# 싸이버거의 결과
# 빵, 닭넓적다리, 레몬마요네즈, 피클, 빵
```


## 장점

1. Director 코드는 작성이 쉽고, 가독성이 좋다.
2. 각 부품을 다양하게 변화할 수 있다.
3. 생성(부품생성동작 호출, 조립)과 표현(각 부품)을 분리함으로서 생성시 내부 동작은 몰라도 만들어야하는 부품만 알아도 된다.
4. 복잡한 객체를 생성하는 절차를 인터페이스를 통한 조립식으로 구성하여 객체 생성 과정을 상대적으로 쉽게 알 수 있다. 
   

## 단점

1. 부품이 추가된다면 모든 Builder에 추가해야한다. (확장이 힘듬) 

## 언제 사용해야할까?

1. 복잡한 객체를 생성해야하는데 그 객체의 구성요소가 부품과 조합으로 생성이 가능한 경우
2. 생성방식은 같지만 표현(부품)이 다른 경우

