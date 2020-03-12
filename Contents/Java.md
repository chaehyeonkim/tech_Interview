# java 면접 대비 공부
---

### 객체지향 언어의 특징
1. 캡슐화(Encapsulation)
   * 객체를 캡슐로 싸서 외부로 부터 보호 (정보 은닉)
2. 상속(Inheritance)- 일반화 관계
    * 하위 개체가 상위 개체의 속성을 모두 가짐
3. 다형성(Polymorphism)
    - 같은 이름의 메소드가 클래스나 객체에 따라 다르게 동작하도록 하는것
    - 오버라이딩, 오버로딩
4. 추상화(Abstraction)
    - 공통적인 특성을 파악하여 하나의 개념으로 다루는 것
 
### 오버라이딩
- 슈퍼클래스에 구현된 메소드를 서브 클래스에서 동일한 이름으로 자신의 특징에 맞게 다시 구현
- 같은 이름, 같은 리턴 타입, 같은 매개변수 리스트
- static, final, private 으로 선언된 메소드는 서브 클래스에서 오버라이딩 할 수 없다.
    - 동적 바인딩 -오버라이딩된 메소드가 항상 실행되도록 보장
```java
class Weapon {
    protected int fire() {
        return 1;
    }
}

class Cannon extends Weapon {
    @Override
    protected int fire() {
        return 10;
    }
    public static void main(String[] args) {
        Weapon weapon;
        weapon=new Cannon();
        System.out.println(weapon.fire());//동적 바인딩되어서 10이 출력
    }
}
```
### 오버로딩 (overloading)
- 클래스 내에서 이름이 같지만 서로 다르게 동작하는 메소드
- 메소드 이름이 동일하거나 매개변수의 개수나 타입이 달라야 한다.
```java
class MethodOverloading { //정상적인 오버로딩 사례
    public int getSum(int i,int j) {
        return i+j;
    }
    public int getSum(int i,int j,int k) {
        return i+j+k;
    }
}
```
```java
class MethodOverloadingFail {
    public int getSum(int i,int j) {
        return i+j;
    }
    public double getSum(int i,int j) {//위의 getSum과 매개변수 타입과 개수 같아서 오버로딩 실패
        return (double) i+j; //리턴 타입은 메소드를 구분하는 기준으로 사용하지 않는다.
    }
}
```
### 추상 클래스
- 추상 메소드
    - 선언은 되어있으나 구현되지 않은 껍데기만 있는 메소드
    - abstract키워드와 함께 원형만 선언하고 코드는 작성하지 않는다.
    ```java
    public abstract String getName();
    public abstract void setName(String s);
    ```
- 추상 클래스
    - 추상 클래스가 되는 경우
        - 추상 메소드를 포함
        ```java
        abstract class Shape {// 추상 클래스 선언
            public Shape() { }
            abstract public void draw();//추상 메소드 선언
        }
        ```
        - 추상 메소드가 없지만 abstract로 선언한 클래스
        ```java
        abstract class MyComponent {
            String name;
            public void load(String name) {
                this.name=name;
            }
        }
        ```
    - 추상클래스 구현
        - 슈퍼클래스에 선언된 모든 추상 메소드를 서브 클래스에서 오버라이딩하여 실행 가능한 코드로 구현하는 것(다형성 실현)
        - 서브 클래스에서 추상 클래스에 선언된 추상 메소드를 모두 구현해야 함.
        - 추상 클래스의 추상 메소드를 서브 클래스에서 구체화하여 그 기능을 확장하는데 목적이 있다.
        ```java
        class Circle extends Shape {
            public void draw() { // 추상 메서드 (오버라이딩)
                System.out.println("Circle"); 
             } 
        }
        ```
        
### 인터페이스
   - 인터페이스는 상수와 추상메소드만으로 구성하며 interface키워드를 사용하여 선언
   - 서로 관련이 없는 클래스에서 공통적으로 사용하는 방식이 필요하지만 기능을 각각 구현할 필요가 있는 경우에 사용
   - 인터페이스 구현
     - 모든 메서드는 추상 메서드로서, abstract public 속성이며 생략 가능
     - 상수는 public static final 속성이며, 생략하여 선언
     - 인터페이스를 상속받아 새로운 인터페이스를 만들 수 있다
     ```java
        interface Phone { // 인터페이스
        int BUTTONS = 20; // 상수 필드 (public static final int BUTTONS = 20;과 동일)
        void sendCall(); // 추상 메서드 (abstract public void sendCall();과 동일)
         abstract public void receiveCall(); // 추상 메서드
        }

        
        class FeaturePhone implements Phone {
        // Phone의 모든 추상 메서드를 구현한다.
        public void sendCall() {...}
        public void receiveCall() {...}

        // 추가적으로 다른 메서드를 작성할 수 있다.
        public int getButtons() {...}
        }
        
        interface MobilePhone extends Phone { //인터페이스 상속
            void sendSmS();// 새로운 추상 메소드 추가
        }
        ```


- 추상 클래스와 인터페이스의 공통점
    - 객체 생성 불가
    - 선언만 있고 구현 내용이 없다
    - 자식 클래스가 구체적인 동작을 구현하도록 책임을 위임함다.
- 차이점
    - 추상클래스는 클래스지만 인터페이스는 클래스가 아니다.
    - 추상클래스는 단일 상속이지만 인터페이스는 다중 상속이 가능하다.
    - 추상 클래스는 "is a kind of" 인터페이스는 "can do this"
        - 추상 클래스 : Appliances-TV, Refrigerator(TV is a kind of appliances)
        - 인터페이스 : Flyable - Plane,Bird(Bird can fly)

        
### Wrapper 클래스
 - primitive type(int, long, float, double, short, byte, boolean, char) 을 객체화한 형태
    ```java
    Integer i=new Integer(10);//정수 10의 객체화
    int ii=i.intValue();//ii=10
    
    Chacter c=new Character('c');// 문자 c의 객체화
    char cc=c.charValue(); //cc='c'
    ```
  - 기본 타입의 값을 Wrapper 객체로 변환하는 것을 박싱(boxing)이라하고, 반대의 경우를 언박싱(unboxing)이라고 한다.
    - boxing : Integer i =new Integer(10);
    - unboxing : int n=i.intValue();
    ```
    Integer i=10;//자동 박싱
    int ten=i;//자동 언박싱
    ```
    
### 제네릭 Generic
- 모든 타입을 다룰 수 있도록 일반화된 매개변수로 클래스나 메서드 선언
    - 제네릭 Stack<E>를 특정 타입으로 구체화한 경우
    ```java
    class Stack<E> {
        void push(E element) {...}
        E pop() {...}
    }
    class Stack<Integer> {
        void push(Integer element) {...}
        Integer pop() {...}
    }
    ```
### 자바 컬렉션 Collection
 - generic기법으로 만들어져 있다.
 - 가변의 크기
 - List
     - 데이터를 중복해서 포함 가능
     - 순서가 있는 collection
     - Vector : 내부에서 자동으로 동기화처리
     - ArrayList : vector보다 속도가 빨라 단일 스레드에 효과적
 - Set
    - 데이터 중복 불가
    - 데이터 순서 의미 없음(집합)
 - Map
    - 검색할 수 있는 인터페이스
    - key와 value의 형태







 

