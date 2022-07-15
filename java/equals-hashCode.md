# # equals를 재정의 하는 이유
두 객체가 논리적으로 같은지 확인해야 할 때 equals를 재정의한다.

# # equals 사용시 hashCode를 재정의하는 이유

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

## set을 통한 중복 size 검사
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