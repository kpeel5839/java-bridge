# 미션 - 다리 건너기

## 👍 다시 해보는 연습!
- 객체 지향 구조에 대해서 너무 무지하고 있던 자신이 통탄스러워 다시 해본다.
- SOLID 기본 원칙에 의거하여 찐하게 해보자!

## 🤔 SOLID 는 무엇일까?
### 🤡 S
- Single Responsibility Principle (SRP)

단일 책임 원칙이라고 칭한다.
  
핵심은 모든 클래스는 하나의 책임만을 가지며, 클래스는 그 책임을 완전히 캡슐화해야 함을 일컫는다.
  
결과적으로 클래스를 변경하는 이유는 단 한가지여야 한다는 것이다.
  
클래스가 제공하는 모든 기능은 이 책임과 주의 깊게 부합해야 한다.
  
프리코스 중 가장 중시하였던 원칙인 것 같다.
  
아무래도 단칙 책임 원칙을 지키게 되면 유지보수가 편해지고, 즉 기능을 변경하기가 쉬워지는 장점때문인 것 같다. 
  
단일 책임 원칙을 설명할 수 있는 예를 한번 들어보자.
  
자동차가 있다고 가정해보자.
  
자동차에는 Seat, Heater, Handle ... 등등이 있을 것이다.
  
그리고서 이 모든 것들을 합쳐놓으면 Car 가 되는 것이다.
  
만일 이 경우 나머지 Seat, Heater 등을 나누지 않고 Car 로만 해결하게 된다면?
  
Car 는 기능이 너무 방대해지고, 책임이 모호해질 것이다.
  
그렇게 되면 위에서 말했던 유지보수, 기능 변경에 용의하다는 장점을 얻을 수 없는 것이다.
  
그래서 SRP 를 지켜야 하는 것이다.

그냥 예를 들었던 것을 구현을 대충 해보자.

일단 먼저, 규칙에 위반되는 형식대로 구성해보자.

구현해야 할 조건은 그냥 차에 타서, 운전하기 좋은 상태로 초기 세팅을 한다고 생각해보자.

```java
public class Test {
    public class Car {
        private int angleOfSeat;
        private int temperatureOfHeater;
        private int angleOfHandle;
        
        public void initialSetUp() {
            angleOfSeat = 105;
            temperatureOfHeater = 25;
            angleOfHandle = 0;
        }
    }
    
    public void main(String[] args) {
        Car jaeyeonCar = new Car();
        jaeyeonCar.initialSetUp();
    }
}
```

이렇게 하게 되면, 이런 간단한 것들은 훨씬 쉬울지 몰라도, 만일 가져야 하는 정보, 상태등이 많다면 너무 복잡하고 커지게 된다.

그래서 적절하게 책임을 분리시키면 아래와 같이 할 수 있을 것 같다.

```java
public class Test {
    public class Car {
        private Heater heater;
        private Seat seat;
        private Handle handle;
    
        public Car() {
          this.heater = new Heater();
          this.seat = new Seat();
          this.handle = new Handle();
        }
    
        public void initialSetUp() {
          heater.initialSetUp();
          seat.initialSetUp();
          handle.initialSetUp();
        }
    }
  
    public class Seat {
        private int angleOfSeat;
    
        public void initialSetUp() {
          angleOfSeat = 105;
          System.out.println("의자 각도 : " + angleOfSeat);
        }
    }
  
    public class Heater {
        private int temperatureOfHeater;
    
        public void initialSetUp() {
          temperatureOfHeater = 25;
          System.out.println("히터 온도 : " + temperatureOfHeater);
        }
    }
  
  
    public class Handle {
        private int angleOfHandle;
    
        public void initialSetUp() {
          angleOfHandle = 0;
          System.out.println("핸들 각도 : " + angleOfHandle);
        }
    }

    public void main(String[] args) {
        Car jaeyeonCar = new Car();
        jaeyeonCar.initialSetUp();
    }
}
```

이걸로 조금 더 코드는 많아질 수 있겠지만 후에도 설명할 유지보수 측면에서 기능 및 상태를 추가, 수정할 때 굉장히 용이할 것이다.

실제로 구현해보고나니, 확실하게 이점이 보이는 것 같은 느낌이다.
  
### 🤡 O
- Open/Closed Principle (OCP)
  
이것은 개방 폐쇄 원칙이다.
  
개방 폐쇄의 원칙이 의미하는 바는 이름에서부터 알 수 있듯이 확장에는 열려있고, 수정에는 닫혀있는 것이다.
  
이 규칙을 지키게 된다면? 기능 추가 혹은 수정에 굉장히 용이할 것이다.
  
이것도 예를 한번 들어보자.
  
만약 Animal 이라는 클래스가 있고, 거기에 sound() 라는 각 동물들의 울음소리를 내는 메소드가 있다고 가정해보자.
  
만일, if/else 조건문으로 분기하는 경우 이것이 위배될 수 있다.
  
```java
public class Animal {
    public void sound(String animalName) { // 간단히 animalName 이라고 칭하자.
        if (animalName.equals("dog")) {
            System.out.println("멍멍");
        } else if (animalName.equals("cat")) {
            System.out.println("야옹");
        } // 등등 더
    }
}
```
  
이렇게 있다고 했을 때, 만일 동물의 종류가 늘어난다고 하면 계속해서, Animal Class 를 변경해야 하는 일이 생기게 된다.
  
그래서 이런 경우는 오버라이딩을 통해서 해결할 수 있다.

```java
public class Test {
    public abstract class Animal {
          abstract void sound();
    }
    
    public class Dog extends Animal {
        @Override
        public void sound() {
            System.out.println("멍멍");
        }
    }
    
    public class Cat extends Animal {
        @Override
        public void sound() {
            System.out.println("야용");
        }
    }
    
    public static void main(String[] args) {
        Animal dog = new Dog();
        Animal cat = new Cat();
        
        dog.sound();
        cat.sound();
    }
}
```
  
이런식으로 구조를 설계하게 되면, 실제로 동물의 종류가 추가되더라도 기존 코드들은 수정하지 않으면서, 기능 추가, 수정에 대한 확장성은 좋아지는 효과를 얻을 수 있다.

### 🤡 L
- Liskov Substitution Principle (LSP)


### 🤡 I
- Interface Segregation Principle (ISP)


### 🤡 D
- Dependency Inversion Principle (DIP)


## 🤔 OOP 의 4가지 특징은 뭘까?
