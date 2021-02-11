**Java Enum**

## 열거타입, Enum 정의 하는법

데이터를 저장할 때 우리는 가끔 한정된 값으로만 데이터를 저장해야할 때가 많이 있을 거시다. 예를 들어서 4계절 을 따져볼 때, FourSeason(Spring, Summber, Fall, Winter) 이 4가지를 제외하고 다른 계절이 올 수 없을 것이다. 그럼 우리는

```java


    public static void main( String[] args )
    {
        String season = "winter";
    }
```

다음과 같이 직접 String타입의 필드를 선언해주고, 리터럴을 입력해줘야 하지만, Enum(열거타입)을 사용하게 되면, 이를 \*_enum 타입으로 정의할 수 있다. \*_

### enum 정의 하는 법

#### enum.java

```java


public enum Season {
    SPRING, // 봄
    SUMMER, // 여름
    FALL, // 가을
    WINTER // 겨울
}
```

public enum (열거타입 이름)으로 선언이 올 수 있다. enum 키워드는 반드시 소문자로 만들어야 한다. 그리고 열거 상수를 선엉ㄴ하면 되는데, **관례적으로 열거 상수는 모두 대문자로 작성하도록 한다.**

#### 클래스 내부에서 정의

```java


public class Vacation {
    LocalDate data = LocalDate.now();

    FourSeason fourSeason = FourSeason.SPRING;

    enum FourSeason{
        SPRING,
        SUMMER,
        FALL,
        WINTER
    }
}
```

#### enum 변수 선언

```java


public class Vacation {
    LocalDate data = LocalDate.now();
    Season season;
    // 열거타입 + 변수 ; 
    public Vacation(Season season){
        this.season = season;
    }
}
```

다음과 같이 enum 타입 열거타입으로 변수를 선언이 가능 하다.

#### enum 변수 초기화

```java


    public static void main(String[] args) {
          // 열거타입 변수 = 열거타입.열거상수;
        Vacation summerVacation = new Vacation(Season.SUMMER);
        Vacation winterVacation = new Vacation(Season.WINTER);
          // null 로도 선언 가능
        Vacation nullVacation = new Vacation(null);
    }
```

## Enum 메소드

#### 1\. name()

```java


    public static void main(String[] args) {

        Season nowSeason = Season.SPRING;
        String name = nowSeason; // Error
        String name = nowSeason.name(); // "Spring" 반환 
    }
```

enum 의 상수를 String 문자열로 반환한다.

#### 2\. Ordinal()

enum 상수가 몇번째에 있는지 확인해주는 메소드 이다.

```java
public enum Season {
    SPRING, // 0
    SUMMER, // 1
    FALL, // 2
    WINTER // 3
}
```

```java
    public static void main(String[] args) {
        Season nowSeason = Season.SPRING;
        int num = nowSeason.ordinal();
        System.out.println(num); // 0 출력
    }
```

하지만 별로 사용하는 걸 추천하지 않는다. 순서에 따라 출력이 되기 때문에 지금은 Spring 이 0 번 째 이지만, 만약 SPRING 앞에 다른 열거상수 가 들어오게 되면 SPRING 은 유동적으로 1로 변하기 때문이다.

#### valueOf()

valueOf(" str ") 을 통해 enum 에 정의된 " str " 과 똑같은 문자열의 객체를 반환 하게 된다.
```java
   public static void main(String[] args) {
        Season season = Season.valueOf("SPRING");
        if(season == Season.SPRING){
            System.out.println("season == SPRING"); //출력
        }
    }
```

#### values

values 메소드는 이넘의 모든 객체들을 **배열로 만들어 리턴한다.**

```
    public static void main(String[] args) {

        Season[] seasons = Season.values();
        for(Season s : seasons){
            System.out.println();
        }
    }
```

#### java.lang.Enum

이런 메소드들은 어디에 정의되어있는 걸까? 바로 java.lang.enum 에 추상클래스의 형태로 정의되어있다. 모든 클래스는 Object가 조상클래스 이듯이, 모든 이넘 클래스들은 java.lang.enum 이 조상인데, 안타깝지만 모든 메소드들은 final 로 정의 되어있기 때문에 overriding이 불가능 하다.

```java
package org.example;

public enum Season {
    SPRING, // 0
    SUMMER, // 1
    FALL, // 2
    WINTER // 3

    public String name(){ //Error
        //code
    }
}

Error : 'name()' cannot override 'name()' in 'java.lang.Enum'; overridden method is final
```

#### EnumSet

EnumSet 이란 enum 타입을 set 자료구조의 형태로 사용할 수 있는 형태를 말한다.

```java
    public static void main( String[] args )
    {
        String season = "winter";

        EnumSet<Season> seasonEnumSet = EnumSet.allOf(Season.class);
        // 모든 Season의 열거 원소를 set에 추가
        EnumSet<Season> seasonEnumSet1 = EnumSet.of(Season.SPRING, Season.SUMMER);
        // 선택된 Season 원소를 Set에 추

        Iterator<Season> iter =  seasonEnumSet.iterator(); // 이터레이터 형태로도 사용 가능
        while (iter.hasNext()){
            System.out.println(iter.next());
        }
    }
```