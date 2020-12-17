# 🐵 new 키워드 이해하기 (new Operator)

---
> 자 저번에는 우리가 어떻게 Class를 생성하고 만드는지, 그리고 클래스의 간단한 종류에 대해서 알아 보았다.
>  그럼 다시 간단한 클래스를 정의 해보자
```java
public class Study{ // Study 클래스 정의
	String name;
}

public class App{
	public static class void main(String args[]){
		Study study; // Study instance 생성 
	}
}
```
> 지금 **study**라는 인스턴스는 여타 String, Integer 와 같이 현재 저렇게 딸랑 정의 해 놓으면 아무 상태도 아니다. 
> 만약 저 상태에서 우리가 **study를 출력**하려 한다면 **initialize**, 즉 초기화를 해줘야 한다는 문구가 뜰 것이다. 

```java
public class App{
	public static void main(String args[]){
	Study study;
	System.out.println(study); // 컴파일 오류 : Variable 'study' might not have been initialized	
	}
}
```
> 안타깝게도, **Class로 선언된 객체(Object) 들은 저렇게 타입을 지정한다고, 선언이 되지 않는다. **
>  그렇기 때문에 우리가 이제  **new** 와 **Constructor** 에 대해서 알아 봐야 한다.
> ![KakaoTalk_20201214_104453162](https://user-images.githubusercontent.com/70433341/102031557-b18eaa00-3df9-11eb-9a90-fdb18e23b2ac.jpg)

---
## new 키워드 사용
```java
public class 
public class App{
	Study study = new Study();
	// Class + 변수 + '=' + New + Constructor
}
```
## Object 생성 (new)
---
> 자바에서는, 객체를 new 연산자를 통해서 생성할 수 있다
> new 연산자는 객체 타입에 앞에 선언되어야 한다. 예를 들어서 A 라는 클래스를 정의 했다고 가정을 하고, A라는 객체를 생성을 하고 싶다면 **new Object();** 와 같은 방식으로 생성해야 한다. 
> 
```java
    public static void main( String[] args )
    {
        App app = new App();
        String str = new String(" ");
        Integer i = new Integer(3);
        Double d = new Double(3.141592);
        Float f = new Float(3.14);
        Character c = new Character('c');
        Long l = new Long(3_000_000L);
    }
```
> 우리가 정의한 Class 이외에도, primitive 타입이 아닌 **reference 타입** 또한 new 를 통한 
> 생성자를 이용할 수 있지만, 아래와 같이 사용할 수 있다. 둘이 뭐가 다른 걸까? 
```java
    public static void main( String[] args )
    {
        String str = " ";
        Integer i = 3;
        Double d = 3.141592;
        Float f = 3.14;
        Character c = 'c';
        Long l = 3_000_000L;
    }
```

---

## String str = " " vs String str = new String(" ")

> 우리는 무의식 적으로 Reference Type을 사용할 때, **(String, Char, Integer, Long, Float)**을 
> 사용 할 때, 이 들이 **Class**로 정의 된걸 알면서도 **new 생성자를 사용하지 않는다.** 그러면 
> **Reference 타입을 통해 생성된 객체들이 생성** 되는 걸까? 
>
> ### new 생성은 Heap 메모리 영역에, ' =  " "' 은 상수 풀 (Runtime constant pool)에 저장
> 말로만 들으면 이해하기 어려우니, 그림을 통해 알아보자, 만약 JVM의 메모리 구조에 대해서 학습하지 않았다면,
> [JVM의 구조](https://catch-me-java.tistory.com/12) , [JDK의 구조](https://catch-me-java.tistory.com/11)에 대해서 충분히 알아보고 이 글을 보도록 하자 **먼저 코드를 보면서 확인해 보자**
>

```java
public static void main(String args[]){
	Integer aaaa = 2;
	Integer cccc = new Integer(3);
	Integer dasada = new Integer(4);
}
```
> 다음 코드를 JVM 에서는 어떻게 관리를 할까? 이 결과를 알아보기 위해서 **ByteCode** 까보도록 하자, 밑에 사진은 바이트 코드에서 상수풀 관리 코드를 발췌했다. 
>

![image](https://user-images.githubusercontent.com/70433341/102055118-b1a89d00-3e2d-11eb-8601-a5c65c9843b4.png)

>
> *  **상수풀 (Constant Pool)** 이란 우리가 Integer, String 같은 레퍼런스 타입 의 데이터 값 또는, 
> 메소드 호출 Class 호출 등을 저장하는 JVM의 메모리 공간 중 하나이다. 
> * 벤더마다 상수 풀의 위치기 heap에 있거나, method 영역에 있을 수 있다. 
> 바이트 코드를 보면 뭔가 다른점이 있지 않은가? 바로 Ljava/lang/Integer가 aaaa 만 있고, **new 로 생성한 변수는 Constant pool (상수 풀)에 저장되지 않는다. **

![image](https://user-images.githubusercontent.com/70433341/102055485-4ad7b380-3e2e-11eb-9674-248267ac8918.png)

> new Integer 로 지정한 변수들의 값은 상수풀이 아닌 **HEAP 메모리 에 저장된다.** 순간 new 에 대해 알아보다가, 너무 low level 로 넘어간 듯한 느낌이 들긴 하지만, 재밌으니 한번 보도록 하자, 
> 각자 jvm 메모리에 저장되는걸 그림으로 표현을 해보자면,

![image](https://user-images.githubusercontent.com/70433341/102056737-22e94f80-3e30-11eb-9465-f91f704a3860.png)

> 이렇게 복잡한 방법을 사용하는 이유는 뭘까? 그냥 전부 new로 heap 메모리에 때려넣으면 그만인데, 
> 왜 굳이 상수 풀이라는 방식을 사용하는 걸까?  **이유는 리터럴 재사용, 메모리 절약 ** 정도가 있겠다. 이를 그림으로 표현을 해보면 

![image](https://user-images.githubusercontent.com/70433341/102057330-1a454900-3e31-11eb-9874-1e9b13c70bc2.png)

#### 다음은 생성자(Constructor) 에 대해서 알아보도록 하자