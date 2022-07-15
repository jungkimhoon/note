
# equals를 재정의 하는 이유
두 객체가 논리적으로 같은지 확인해야 할 때 equals를 재정의한다.

# equals 사용시 hashCode를 재정의하는 이유
아래 예시를 통해 확인해본다. 

## Car 클래스 equals 재정의

```java
public class Car {
    private final String name;

    public Car(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Car car = (Car) o;
        return Objects.equals(name, car.name);
    }
}
```

## car1, car2 객체 비교
```java
public static void main(String[] args) {
    Car car1 = new Car("foo");
    Car car2 = new Car("foo");
    
    // true 출력
    System.out.println(car1.equals(car2));
}


console> true
```

## set을 통한 size 검사
```java
public static void main(String[] args) {
    Set<Car> cars = new HashSet<>();
    cars.add(new Car("foo"));
    cars.add(new Car("foo"));

    System.out.println(cars.size());
}


console> 2
```
* hashCode를 equals와 함께 재정의하지 않으면 위처럼 예상치 못한 결과를 얻을 수 있다.
* hashcode를 재정의 하지 않으면 같은 값 객체라도 해시값이 다를 수 있다. 따라서 HashTable에서 해당 객체가 저장된 버킷을 찾을 수 없다.

Car 클래스는 해당 클래스에서 hashCode를 재정의하지 않아 Object의 hashCode 메서드를 사용한다. Object 클래스의 hashCode 메서드는 객체의 고유한 주소 값을 int 값으로 변환하기 때문에 객체마다 다른 값을 리턴한다.


## Car 클래스 hashCode 재정의
```java
public class Car {
    private final String name;

    public Car(String name) {
        this.name = name;
    }

    // intellij Generate 기능 사용
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Car car = (Car) o;
        return Objects.equals(name, car.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}
```

위처럼 hashCode 재정의를 통해 객체의 동등성을 설정하여 올바르게 사용할 수 있다.

# 결론 
HashTable, HashMap 요놈들은 Key의 hashcode를 통해 value값을 쉽게 찾아낼 수 있다. equals() 메서드는 해시 충돌이 일어났을 때 버킷 리스트 내부에서 사용되고, hashCode()가 버킷 인덱스를 결정하게 된다. 따라서 해시 관련 자료구조를 사용하려면 클래스에서 equals() 메서드와 hashCode() 메서드를 둘 다 정의해야 한다.


