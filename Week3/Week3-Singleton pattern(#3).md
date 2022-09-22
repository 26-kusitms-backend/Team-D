# [백엔드 스터디 in 큐시즘 - D조] Week 3: 스프링의 전반적인 이해
## #3. 싱글톤 디자인 패턴은 무엇인가
### 정의
싱글톤 패턴은 객체의 인스턴스를 한개만 생성되게 하는 패턴이다. 즉, 인스턴스가 필요할 때 똑같은 인스턴스를 만들지 않고 기존의 인스턴스를 활용하는 패턴인 것이다.

싱글톤 패턴은 주로 공통된 객체를 여러 개 생성해서 사용해야하는 상황에서 사용되게 된다.

데이터베이스: 커넥션풀, 스레드풀, 캐시, 로그 기록 객체 등
안드로이드 앱: 각 액티비티 들이나, 클래스마다 주요 클래스들을 하나하나 전달하는게 번거롭기 때문에 싱글톤 클래스를 만들어 어디서든 접근하도록 설계

또한 인스턴스가 절대적으로 한 개만 존재하는 것을 보증하고 싶을 때도 사용할 수 있다.

### 장점

1. 메모리 측면의 이점
싱글톤 패턴을 사용하게 된다면 한개의 인스턴스만을 고정 메모리 영역에 생성하고 인스턴스를 사용하기 때문에 메모리 낭비를 방지할 수 있다.

2. 속도 측면의 이점
생성된 인스턴스를 사용할 때는 이미 생성된 인스턴스를 활용하여 속도 측면에 이점이 있다.
두 번째 이용부터는 객체 로딩 시간이 줄어 성능이 좋아지는 장점이 있다.

3. 데이터 공유가 쉽다
전역으로 사용하는 인스턴스이기 때문에 다른 여러 클래스에서 데이터를 공유하며 사용할 수 있다. 하지만 동시성 문제가 발생할 수 있어 이 점은 유의하여 설계하여야 한다.

-> 이러한 장점을 가진 싱글톤 패턴은 `DBCP` (DataBaseCommection Pool)처럼 `공통된 객체`를 `여러개 생성`해서 사용해야 하는 상황에서 많이 사용된다.

### 단점
1. 결합도의 문제
싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우에 다른 클래스의 인스턴스들 간에 `결합도가 높아`져 `개방-폐쇄 원칙`을 `위배`하게 된다. 이는 객체 지향 설계 원칙에 어긋나기 때문에 `수정이 어려`워지고 `유지보수의 비용이 높아`질 수 있다.

2. 동시성 문제
또한 `멀티쓰레드 환경`에서 동기화 처리를 안하면 `인스턴스가 2개` 생성될 수 있는 가능성이 생기게 된다.

-> 이러한 이유로 싱글톤 패턴은 꼭 필요한 경우가 아니라면 지양해야 한다.

### 멀티스레드 환경에서의 예제
멀티스레드 환경에서 안전한 싱글톤 만드는 법은 대표적으로 아래 세가지 방법이 있다.

#### 1. Lazy Initialization (게으른 초기화)
``` java
public class ThreadSafe_Lazy_Initialization{
 
    private static ThreadSafe_Lazy_Initialization instance; // private static으로 인스턴스 변수를 만들고
 
    private ThreadSafe_Lazy_Initialization(){} //private으로 생성자를 만들어 외부에서의 생성을 막는다.
     
    public static synchronized ThreadSafe_Lazy_Initialization getInstance(){ //synchronized 동기화를 활용해 스레드를 안전하게 만든다.
        if(instance == null){
            instance = new ThreadSafe_Lazy_Initialization();
        }
        return instance;
    }
 
}
```
하지만, `synchronized`는 큰 성능저하를 발생시키므로 권장하지 않는 방법이다.


#### 2. Lazy Initialization + Double-checked Locking

두번째 방법은 1번의 성능저하를 완화시키는 방법이다.

``` java
public class ThreadSafe_Lazy_Initialization{
    private volatile static ThreadSafe_Lazy_Initialization instance;

    private ThreadSafe_Lazy_Initialization(){}

    public static ThreadSafe_Lazy_Initialization getInstance(){
    	if(instance == null) {
        	synchronized (ThreadSafe_Lazy_Initialization.class){
                if(instance == null){
                    instance = new ThreadSafe_Lazy_Initialization();
                }
            }
        }
        return instance;
    }
}
```
1번과는 달리, 먼저 조건문으로 인스턴스의 존재 여부를 확인한 다음 두번째 조건문에서 synchronized를 통해 동기화를 시켜 인스턴스를 생성하는 방법이다.

스레드를 안전하게 만들면서, 처음 생성 이후에는 synchronized를 실행하지 않기 때문에 성능저하 완화가 가능하지만 완전히 완벽한 방법은 아니다.


#### 3. Initialization on demand holder idiom (holder에 의한 초기화)

3번은 클래스 안에 클래스(holder)를 두어 JVM의 클래스 로더 매커니즘과 클래스가 로드되는 시점을 이용한 방법이다.

``` java
public class Something {
    private Something() {
    }
 
    private static class LazyHolder { // 클래스 안에 선언한 클래스인 holder에서 선언된 인스턴스는 static이기 때문에 클래스 로딩시점에서 한번만 호출된다.
        public static final Something INSTANCE = new Something(); //final을 사용해서 다시 값이 할당되지 않도록 만드는 방식을 사용
    }
 
    public static Something getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```
2번처럼 동기화를 사용하지 않는 방법을 선호하지 않는 이유는 개발자가 직접 동기화 문제에 대한 코드를 작성하면서 회피하려고 하면 프로그램 구조가 그만큼 복잡해지고 비용 문제가 발생할 수 있기 떄문이다. 

또한 코드 자체가 정확하지 못할 때도 많다. 이 때문에 3번과 같은 방식으로 JVM의 클래스 초기화 과정에서 보장되는 `원자적 특성`을 이용해 싱글톤의 초기화 문제에 대한 책임을 `JVM`에게 떠넘기는 걸 활용한다.

실제로 가장 많이 사용되는 일반적인 싱글톤 클래스 사용 방법이 3번이다.

참고한 블로그
<br>
[싱글톤 패턴(Singleton pattern)](https://gyoogle.dev/blog/design-pattern/Singleton%20Pattern.html)
<br>
[싱글톤(Singleton) 패턴이란?](https://velog.io/@seongwon97/%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80)
<br>
[[디자인 패턴] 싱글톤 패턴(Singleton Pattern) 정리 및 예제 - 생성 패턴](https://devmoony.tistory.com/43)