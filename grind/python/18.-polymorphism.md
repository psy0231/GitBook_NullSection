# Page

## Polymorphism

* 다형성.
* 그 프로그래밍 언어의\
  자료형 체계의 성질을 나타내는 것으로,\
  프로그램 언어의 각 요소들인\
  상수, 변수, 식, 오브젝트, 함수, 메소드 등이\
  다양한 자료형(type)에 속하는 것이\
  허가되는 성질을 가리킨다.\
  반댓말은 단형성(monomorphism)으로,\
  프로그램 언어의 각 요소가\
  한가지 형태만 가지는 성질을 가리킨다.
* 같은 이름의 메소드를 호출했을 때,\
  다른 동작을 하도록 구현하는 것.

### 다형성의 장점 단점

#### 장점

* 유연성과 확장성\
  한 가지 인터페이스나 메서드를\
  여러 다른 방식으로 구현할 수 있다.\
  이는 코드의 유연성과 확장성을 높여\
  새로운 기능을 추가하거나 변경할 때\
  기존 코드의 수정을 최소화함.
* 코드 재사용:\
  유사한 작업을 수행하는 여러 클래스들이\
  동일한 인터페이스를 공유할 수 있다.\
  이로 인해 코드의 재사용성이 증가하고,\
  개발 시간이 단축될 수 있음.

#### 단점

* 복잡성\
  다양한 클래스와 메소드 간의 관계를\
  이해하고 관리해야 하기 때문에\
  설계 단계에서 많은 고려가 필요하고\
  설계 과정을 복잡하게 만들 수 있음.
* 오버헤드\
  일부 경우 다형성을 구현하기 위해\
  추가적인 추상화나 인터페이스가 필요.\
  이로 인해 코드의 복잡성이 증가하고,\
  실행 속도가 느려질 수 있음.
* 오류 발생 가능성\
  동적으로 메서드나 속성을 선택하므로\
  컴파일 시점에서 오류 감지가 어려워져\
  디버깅을 어렵게 만들 수 있음.

### vs monomorphism

* 단형성을 굳이 자세하게는 안하는데\
  이게 일반적인 상황이기때문.\
  다만 차이점을 보면 좀 더 이해가 쉬울듯.
*   단형성과 다형성의 차이를 보면\
    단형성은 method가 하나만 존재할 수 있고,\
    정해진 type만 받아들이고\
    그에 맞는 동작만을 하는게 주 특징인듯.

    ```python
    #숫자를 문자열로 바꾸는 경우
    string = StringFromNumber(number)

    #날짜를 문자열로 바꾸는 경우
    string = StringFromDate(date)
    ```
*   다형성의 경우는\
    같은 이름의 method가 있을 수 있고\
    parameter 상황에 따라\
    다른 동작을 하도록 구현하는 것으로

    ```python
    #숫자를 문자열로 바꾸는 경우
    string = StringFrom(number)
    #또는 string = number.StringFrom()

    #날짜를 문자열로 바꾸는 경우
    string = StringFrom(date)
    #또는 string = date.StringFrom()
    ```
*   좀 더 익숙한 예를 보면

    ```python
    l1 = len("hello")
    l2 = len([1,2,3,4,5])
    l3 = len((1,2,3,4,5))

    print(l1, l2, l3)
    print("hello")
    print("str1", "str2", "str3", end="\n\n", sep="\t")
    # 5 5 5
    # hello
    # str1	str2	str3
    ```

    각각 다른 타입의 args를 넘겨주었지만\
    같은 이름의 method를 호출했을 때\
    해당 타입에 맞는 동작을 하도록 구현되어 있다.

## python 에서 다형성

* 이미 위에서 예시가 나왔지만,\
  일반적으로 OOP 언어에서는 다형성을 위해\
  override, overload를 사용하는데\
  파이썬에서는 override만 사용한다.\
  overload는 파이썬에서 지원하지 않는다.
  * overload를 지원하지 않는 이유는\
    변수의 타입을 런타임 시점에 결정하는\
    동적 타이핑 언어이기 때문이라고함.
  * 아무튼 안되고, 전혀 방법이 없는건 아니지만\
    여기에 쓰진 않을것같다.
*   python에서는 같은 method를 정의하거나\
    override할 때가 주로 예시로 많이 나오는데\
    둘이 비슷하게 쓰여서\
    간단하게 하고 넘어가도 될듯.

    ```python
    class A:
      def __init__(self):
          self.a = 1

      def printclass(self):
          print("A")

    class B(A):
        def __init__(self):
            super().__init__()
            self.b = 2

        def printclass(self):
            print("B")

    class C(A):
        def __init__(self):
            super().__init__()
            self.c = 3

        def printclass(self):
            print("C")

    class D:
        def __init__(self):
            self.d = 4

        def printclass(self):
            print("D")      

    aa = A()
    bb = B()
    cc = C()
    dd = D()

    for item in (aa, bb, cc, dd):
      item.printclass()
    # A
    # B
    # C
    # D
    ```

    python에서 다형성을 구현하는 방법인\
    method overriding은 A, B, C class에서
*   하나 더.\
    자주 예로 드는`len()`을 다시 보면\
    arg로 넘긴 class의 method인\
    `__len__()`을 호출하는 것이다.\
    같은 이름의 method가 여기저기\
    구현되어있던것.\
    근데 이 경우도 일반적으로\
    Iterable계열에서 쓰이면 상속이긴한데\
    그렇지 않더라도 `__len__()`을 구현하면\
    `len()`을 쓸 수 있음. \
    위에서 D 가 A, B, C 와는 상관없어도\
    같은 이름의 method를 가지고 있기 때문에\
    같은 method를 호출할 수 있는것처럼.

    ```python
    class A:
      def __init__(self):
          self.a = 1

      def __len__(self):
          return self.a

    aa = A()
    print(len(aa))
    # 1
    ```
