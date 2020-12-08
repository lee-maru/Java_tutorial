# 🐵 Class

---
### 🙈 클래스?
> 어떤 객체지향 언어에서든지, 클래스라는 개념을 한번쯤은 들어봤을 것이다. 먼저 간단한 코드를 분석해보자, 
>
> * **자바에서 클래스를 선언**
```java
public class App {
}
```
>정말 쉽다, 이런식으로 **public** + **class** + **name** 다음과 같이 만들면 클래스 하나가 생성된 거다. 
> 이제 클래스를 직접 만들어보면 애완동물 클래스인 Pet을 만들고, 여기에 변수도 넣어보자
```java
 public class Pet {

    String kind; // 펫의 종류
    String name; // 펫 이름
    Integer age; // 펫 나이

}
```
> 이렇게 만들어 놓은 클래스는 우리가 마음대로 찍어낼 수 있다, 내가 강아지를 키우고 있다면 클래스를 변수로 선언하여, 클래스에다가 마음대로 집어넣을 수 있다. 마치 **C의 구조체와 같이** 말이다. 
>
```java
    public static void main(String[] args) {
        Pet pet = new Pet();
        pet.age = 10;
        pet.kind = "dog";
        pet.name = "복실이";
        System.out.println(pet.toString());
    }
```
> * **toString**  : 객체가 가진 정보를 String 으로 만들어 주는 함수 <span style="color:green">**@Overriding**</span> 해야함
> 이 **Pet** 클래스를 선언함으로 써, 유저가 원하는 다양한 **객체를 클래스를** 통해서 선언이 가능해진다. 
> 우리는 이 과정을 **인스턴스 화** 라고 한다. 
> > <span style="color:#50bcdf">**결과 콘솔**</span>
> > ``` java
> > > Pet{kind='dog', name='복실이', age=-10}
> > ```

---
## 🙉 이런 클래스를 가진 OOP 의 특징이 있다.

<center> <span style="font-size : 20px; font-weight:bold;"> 무결성, 은닉성, 캡슐화 </span> </center>

* 이들의 특징과 장점은 나중에 알아보도록 하고, 이런게 있구나하고 넘어가도록 하자, Getter를 왜 사용하는지
* Setter를 왜 사용하는지, 그리고 class안에 method를 정의하면서 어떻게 활용이 가능한지 알 수 있다. 

## 🙉 객체 생성부터 변수 생성까지
---
# 🐵 내부 클래스 (Inner Class)

### 🙈 내부 클래스?
>
---
## 인스턴스 클래스 (Instance Class)
> 

## 스태틱 클래스 (Static Class)
>

## 지역 클래스 (Local Class)
>

## 익명 클래스 (Annonymous class)
>

## 익명 내부 클래스 (Annonymous Inner Class)
>

