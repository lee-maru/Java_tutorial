**Java Exception**

## 자바가 제공하는 계층구조

우리는 java 코드를 작성하면서, 또는 개발을 하면서 많은 에러를 봐왔을 것이다. 그 중 우리는 자바에서 어떤 에러들이 있고, 그리고 **자바에서는 어떻게 에러를 관리하는지**에 대해서 알아보도록 하자

### 왜 Error에 대해서 공부해야 하는가?

만약 자바 에러처리에 대해서 배우기전에, 왜 에러에 대해서 알아야 하는가? 또는 자신은 개발을 하면서 에러문을 일기 않는다. 라고 했을 때, 다음 동영상을 꼭 보도록 하자.

자바에서 코드를 작성하면서 문제가 될 때는 크ㅈ게 2가지로 나눌 수 있다.

![image](https://user-images.githubusercontent.com/70433341/104128746-a1dc9400-53ac-11eb-89aa-f6a499a24f6a.png)

일단 모든 예외의 계층 구조에 대해서 한눈에 보도록 하자

![image](https://user-images.githubusercontent.com/70433341/104199159-6d2f1200-546a-11eb-89bd-207c4d2c149f.png)

---

### Exception 과 Error 의 다른 점

-   예외와 오류는 모드 Throwable 클래스 의 하위 클래스인데, 에러는 **보통 시스템 리소스 문제로 발생하게 되는데, 어플리케이션 리소스 문제를 알면 안되기도 한다.** Error는 보통 시스템 충돌, 메모리 부족 이 있고,
-   예외는 컴파일 시간과 런타임시 발생하는게 대부분이다. 주로 개발자가 작성한 코드로 발생한다.

#### 표

| Key        | **Error**                       | **Exception**                           |
| ---------- | ------------------------------- | --------------------------------------- |
| **타입**   | **확인도지 않은 유형으로 분류** | **Checked 와 Unchecked 로 분류**        |
| **패키지** | **java.lang.error**             | **java.lang.Exception**                 |
| **복구**   | **불가**                        | **가능**                                |
| **발생**   | **컴파일시에 발생할 수 없음**   | **런타임 & 컴파일 시간에 발생**         |
| **예**     | **OutOfMemoryError, IOError**   | **NullPointerExceoption, SqlException** |

#### Error 예

```java
public class ErrorExample {
   public static void main(String[] args){
      recursiveMethod(10)
   }
   public static void recursiveMethod(int i){
      while(i!=0){
         i=i+1;
         recursiveMethod(i);
      }
   }
}
outPut:
Exception in thread "main" java.lang.StackOverflowError
   at ErrorExample.ErrorExample(Main.java:42)
```

#### Exception 예

```java
public class ExceptionExample {
   public static void main(String[] args){
      int x = 100;
      int y = 0;
      int z = x / y;
   }
}
outPut :
java.lang.ArithmeticException: / by zero
   at ExceptionExample.main(ExceptionExample.java:7)
```

### Exception

-   Exception 은 사실 Error 와 비슷하게, 문제가 발생하면 어플리케이션이 종료되는 특징을 가지고 있다. 하지만 자바에서는 Exceptiopn Handling **(예외처리)**을 통해서, 프로그램이 정상 작동할 수 있게 도와주도록 한다. 이 예외처리는 아까 말한 **런타입 에러일 때, 예외처리가 가능하다.**

### java.lang.Exception

-   아까 말했듯이, 자바에서는 에러를 클래스로 지정했다고 하였다. 그러면 저번에 [패키지](https://catch-me-java.tistory.com/42) 에 대해서 말했는데, 이 들은 어디 클래스에 저장되어있을까?
-   모든 예외 클래스는 java.lang.Exception에 저장되어있다. 모든 예외 클래스라고 함은, 일반 예외와 실행 예외 모두 포함이지만, 좀 특이한 패키지 구성요소를 가지고 있다.

![스크린샷 2021-01-11 오후 2 55 43](https://user-images.githubusercontent.com/70433341/104150384-164f1b80-541d-11eb-96ef-28eb7c488bae.png)![스크린샷 2021-01-11 오후 2 59 03](https://user-images.githubusercontent.com/70433341/104150564-8e1d4600-541d-11eb-87ff-6d30fb11906d.png)

\*_일반예외 (Exception) : \*_일반 예외와 실행 예외 클래스를 구별하는 방법은 예외 Exception을 상속받지만, RuntimeException은 상속받지 않이함.

\*_실행예외 (Runtime Exception) : \*_ 실행 예외는 RuntimeException을 상속 받는다. 물론, 표에서 보다시피 Rumtime Exception 이 java.lang.Exception을 상속받지만, jvm에서는 runtimeException 상속 여부를 보고 판단하게 된다.

---

### RuntimeException 종류

#### 1\. NullPointerException(NPE)

> **java는 Null 에 취약하다.** 라는 말을 들어본적이 있을 것이다. 그만큼 Java 개발을 하다보면, 수많은 NullPointerException이 발생할텐데, 이 NPE는 언제 발생하는지 왜 발생한느지에 대해서 알아보도록 하자. **NullPointerException 의 발생 원인은 비어있는 객체에 무슨 행동을 하려할 때, 발생** 한다 라고 생각하면 편하다. 이 말은 누구나 할 수 있는데

```java
public class NullPointerException {
    Integer x; // null
    String str; // null
      int y; // 0
    public static void main(String[] args) {
        Son son = new Son();
        System.out.println(son.x -1); // output : NullPointerException
        System.out.println(son.str.charAt(0)); // output : NullPointerException
          System.out.println(son.y -1); // output : -1
   }
}
```

-   즉 Null 인 객체에 연산을 하거나 또는 Null 에 사용할 수 없는 ' .method\*() ' 메소드를 체이닝 걸면 다음과 같이 NullPointerException이 발생할 수 있다.
-   이 클래스에서서는 사실 모든 객체에 객체로 멤버변수로 만든 x 와 str 은 전부 생성 되자마자 데이터를 Null 을 가지고 있기 때문에 무조건 NPE가 발생하게 된다.
-   y 와 같이 맴버변수에 primitive 타입을 사용하면, 객체 생성을 하면서 y = 0 으로 초기화 되기 때문에 문제가 발생하지않는다.

##### NullPointerException 을 피하는 방법

-   보통 회사에서 프로젝트에 들어간다고 했고, java를 사용하고 있다면, 메뉴얼을 정해놓는 경우가 많다. 어떤 필드에서 NPE 가 발생할 수 있으며, 이럴 때는 어떻게 처리를 하겠다. 라는 식으로 말이다. 크게 우리가 코드를 작성하면서, 회피하는 방법은 2가지가 있을 것이다. **Optional** 을 사용하거나 **if문을 이용한 Null 체크** 가 있을 것이다. 간단한 예제 코드를 작성하겠지만, 이는 절대 **Best practice 가 아니라는 점을 참고 했으면 좋겠다.**

```java
public class NullPointerExceptionExample {

    Integer num;

    public static void main(String[] args) {
        NullPointerExceptionExample ne = new NullPointerExceptionExample();
        // if 문으로 Null 체크 하여 걸러내기
        if(ne.num == null){
            System.out.println("num 은 null 입니다.  ");
        }else{
            System.out.println(ne.num + "<- num 값 출");
        }
        // Optional 로 걸러내기
        Optional.ofNullable(ne.num).ifPresent(num -> System.out.println("num은 " + num + "이다."));
   }
}
```

#### 2\. ArrayIndexOufOfBoundsException

> ArrayIndexOutOfBoundsException 은 먼저 이름에도 알 수 있듯이, 배열을 사용함에 있어서, 배열 범위 이상을 사용하게 되면 다음 에러가 발생하게 된다. 간단한 예를 들어보도록 하자.

```java
public class ArrayIndexOutOfBoundsException {

    public static void main(String[] args) {
        int[] arr= new int[3]; // 범위는 arr[0] ~ arr[2] 까지 이다.
        for (int i = 0; i <= arr.length; i++) { //  1 ~ 3
            arr[i] = i; // 범위 초과 
            System.out.println(arr[i]);
        }
   }
}
output :
0
1
2
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
```

---

### Checked Exception 과 Unchecked Exception

#### Checked Exception

-   Checked Exception은 바로 직역하면 알 수 있듯이, 체크가 가능한 예외라는 것이다.
-   체크란, compiler 단에서 체크가 가능한 에러를 말하는 것이다. 이는 어찌보면 **Runtime Exception과 상반된다고 할 수 있다.** 이 체크된 에러들, 즉 에러가 발생할 수 있을 때에는 예외처리를 진행해줘야 하는데, 자바에서 강제적으로 try catch문을 작성하도록 하게된다. 예를 들어보자.

```java
    public static void main(String[] args) {
        Class clazz = Class.forName("java.lang.String2");

      // Output : Unhandled exception: java.lang.ClassNotFoundException , Add Exception to method signature
   }
```

다음과 같이 처리되지 않은 예외가 발생하게 된다. **Class.forname(String str)** 에서 str 매개변수의 클래스가 존재하면 Class 객체를 리턴하지만, 존재하지 않으면 ClassNotFoundException 예외를 발생 시키게 된다. ClassNotFoundException 예외는 일반 예외 이므로 컴파일러는 개발자가 예외 처리를 작성하도록 말들게 된다.

```java
   public static void main(String[] args) {
        try {
            Class clazz = Class.forName("java.lang.String2");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
   }
```

#### Uncheked Exception

-   Unchecked Exception 이라 함은, 컴파일러 자체에서 예외를 찾아내지 못하는 걸 말한다. Checked Exception과 다르게 예외처리를 강제로 지저하지는 않는 특징이 있다. 예시를 들어보자

```java
class Example {
    public static void main(String args[])
    {
        int arr[] ={1,2,3,4,5};
        System.out.println(arr[7]);
        /*
        OutPut : Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 7 out of bounds for length 5
    at Example.main(Example.java:9)
         */
    }
}
```

이렇게 명시적으로 언제 try - catch 를 사용해야할지 컴파일러가 명시해주 않기 때문에, **개발자의 경험 또는 테스트를 통해서 예외처리를 해줄 수 있도록 해야한다.**

이런 식으로 무조건 Exception이 발생할 수 있는 상화이지만, 컴파일러에서 알 방도가 없기 때문에 다음과 같이 try catch문을 강제 하지 않는다. 그럼 try-catch 의 문법에 대해서 알아 보도록 하자

---

### 예외 처리 코드

> 프로그래메서 예외가 발생을 한다면, Error 이든, Exception 이든 어플리케이션을 강제적으로 종료시키게 된다. 이 점에서 개발자들은, 강제로 종료를 시킬 것인가, 아니면 넘겨서 다른작업을 진행할 수 있게 하는가? 보통 대부분의 어플리케이션에서는 **Exception 처리가 되어도 정상적인 작동을 하게 하길 원할 것이다.** 그래서 **갑작스러운 종료를 막고, 정상 실행을 유지할 수 있도록 처리하는 코드를 예외처리 코드.** 라고 한다.

#### try - catch - finally 블록 작성 원리

![image](https://user-images.githubusercontent.com/70433341/104161782-c1b89a00-5436-11eb-89be-f0cb5a22b162.png)

> try 블록에서는 예외 발생 가능성이 있는 코드를 작성하도록 합니다. 만약 try 스코프에서 예외 발생이 하지 않는다면, **catch**문을 거치지 않고, 바로 즉각적으로 **finally 스코프 코드를 실행하며,** 만약, \*_try 스코프에서 예외처리가 발생하게 된다면, catch 블록으로 넘어가 코드를 실행하고 finally 코드를 실행하게 된다. \*_ 아까 전의 코드로 예시를 들어보도록 하자.

```java
   public static void main(String[] args) {
        try {
            Class clazz = Class.forName("java.lang.String2"); 
            //Exception 발생.
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
            // Error 스택 트레이스 찍음.
        } finally {
            System.out.println("HelloWorld");
            // HelloWorld 출력.
        }
   }

output: java.lang.ClassNotFoundException: java.lang.String2
    at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:602)
    at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:178)
    at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:522)
    at java.base/java.lang.Class.forName0(Native Method)
    at java.base/java.lang.Class.forName(Class.java:340)
    at Example.main(Example.java:8)
HelloWorld
```

#### 다중 catch

> 아래와 같은 **Exception**이 2개가 발생할 예정이다. 하지만, 실제로는 **처음 발생하는 Exception** 이 외에 다른 Exception 아래의 코드에서는 NullPointerException은 발생하지 않을 것이다.

```java

public class Main {
    Integer x;
    public static void main(String[] args) {
        try {
            Class clazz = Class.forName("java.lang.String2");
            //ClassNotFoundException 발생.

            // 다중 catch 발생하지 않음
            Main main = new Main();
            System.out.println(main.x + 1);
            // NullPointerException 발생
        } catch (ClassNotFoundException e) {
            System.out.println("ClassNotFoundException 발생");
            e.printStackTrace();
            // Error 스택 트레이스 찍음.
        } catch (NullPointerException e){
            e.printStackTrace();
            System.out.println("NullPointerException 발생");
        } finally {
            System.out.println("HelloWorld");
            // HelloWorld 출력.
        }
    }
}

```

#### 다중 catch 순서

> 이전 다중 catch 문을 작성할 때 주의할점이 있다. 상위 예외클래스가 하위 예외 클래스보다 아래에 위치해야 한다는 점이다. 이유는 간단하다. 하위 예외 클래스가 이미 상위 예외 클래스에 속해있기 때문에, 상위 예외 클래스가 앞에있으면 문제가 된다는 점이다. 그림으로 표현한다면,

![image](https://user-images.githubusercontent.com/70433341/104183525-68ac2e80-5455-11eb-9c89-09bf9de8b13b.png)

-   다음과 같이 하위 예외 클래스인 B 는 실행되지 않을 것이다. \*\*사실 컴파일러 단에서 컴파일 조차 하지 못하게 할 것 이다.

```java

public class Main {
    Integer x;
    public static void main(String[] args) {
        try {
            Class clazz = Class.forName("java.lang.String2");
            //ClassNotFoundException 발생.

            // 다중 catch 발생하지 않음
            Main main = new Main();
            System.out.println(main.x + 1);
            // NullPointerException 발생
        } catch (Exception e) {
            System.out.println("ClassNotFoundException 발생");
            e.printStackTrace();
            // Error 스택 트레이스 찍음.
        } catch (NullPointerException e){
            e.printStackTrace();
            System.out.println("NullPointerException 발생");
        } finally {
            System.out.println("HelloWorld");
            // HelloWorld 출력.
        }
    }
}
outPut : Error -> Exception 'java.lang.NullPointerException' has already been caught
```

이미 Exception e 가 있는데 굳이 NullPointerException 코드를 작성하지 말라는 것이고, NPE 코드를 **delete 하기를 권한다.** 만약 정상적인 코드로 수정한다면,

```java
package org.example;

public class Main {
    Integer x;
    public static void main(String[] args) {
        try {
            Class clazz = Class.forName("java.lang.String2");
            //ClassNotFoundException 발생.

            // 다중 catch 발생하지 않음
            Main main = new Main();
            System.out.println(main.x + 1);
            // NullPointerException 발생
        } catch (ClassNotFoundException e) {
            System.out.println("ClassNotFoundException 발생");
            e.printStackTrace();
            // Error 스택 트레이스 찍음.
        } catch (Exception e){
            e.printStackTrace();
            System.out.println("NullPointerException 발생");
        } finally {
            System.out.println("HelloWorld");
            // HelloWorld 출력.
        }
    }
}
```

이런 코드가 좀 더 적합할 것이다.

#### Java7 Multi catch

> java7 이상부터는 여러개의 catch 문들을 효과적으로 다룰 수 있는 방법을 제안하였다. **multi catch** 문인데, 이 문법은 복잡한 여러개의 catch scope를 한거번에 한개의 소코프로 합쳐주게 된다. 문법은 다음과 같다.

##### 문법

```java
try{
    // Exception 발생 코드
}catch(예외1 | 예외2 e){
    e.printStackTrace();
}finally{
    //finally 코드
}
```

##### 예제

```java
public static void main(String[] args) {
        try {
            Class clazz = Class.forName("java.lang.String2");
            //ClassNotFoundException 발생.

            // 다중 catch 발생하지 않음
            Main main = new Main();
            System.out.println(main.x + 1);
            // NullPointerException 발생
        } catch (ClassNotFoundException | NullPointerException e) {
            System.out.println("ClassNotFoundException 발생 또는 NullPointerException 발생");
            e.printStackTrace();
            // Error 스택 트레이스 찍음.
        }  finally {
            System.out.println("HelloWorld");
            // HelloWorld 출력.
        }

    }
```

#### Java7 try - catch - resource

> 자바7에서 부터 사용할 수 있는 try-catch-resource 이다. 이는, InputStream, 또는 네트웤프로그래밍에서 사용하는 Socket, ServerSocket 을 좀 더 안전하게 사용할 수 있도록 지원해주는 것이다. 이들의 특징은 c언어에서 포인터를 사용했듯이, 전부 사용하고 난다음에 return 하기전에 close(); 메서드를 실행해줘야 한다는 점이다. 한번 예를 들어보도록 하자.

##### java7 이전의 FileInputStream

```java
public static void main(String args[]){
    FileInputStream fis = null;
    try{
        fis = new FileInputStream("절대없는파일.txt");
        //실행 코드
    }catch(IOException e){
        e.printStackTrace();
    }finally{
        //FileInputStream close() 해주기
        if(fis!=null){
            try{
                fis.close();
            }catch (IOException e){
                e.printStackTrace(); // 또 다른 try-catch 문을 작성해줘야함 
            }
        }
    }
}
```

다음과 같은 복잡한 코드를 작성해야 한다. fis 를 분명 다 썻지만, 저기서 똑같이 try-catch 문을 작성햐여 FileInputStream을 닫아줘야 한다는 불편함이 있다.

##### java7 이후의 FileInputStream

```java
public class Main {
    public static void main(String args[]){
        try(FileInputStream fis = new FileInputStream("절대_없는_파일.txt")){

            //try() Exception 발생시 바로 fis.close() 메소드 실행하고 catch 로
            // 명시적으로 fis.close() 를 적어주지 않는다.

            System.out.println(fis.read());
        }catch(IOException e){
            e.printStackTrace();
        }
    }
}
```

어떻게 이런일이 가능한 것일까? try( **여기에있는 코드**)를 인식하여, close를 필요한 코드가 있으면, 바이트코드를 조작하여, close를 생성하는 것이다.

```java
디컴파일 클래스 파일

        try {
            FileInputStream fis = new FileInputStream("절대_없는_파일.txt");

            try {
                System.out.println(fis.read());
            } catch (Throwable var5) {
                try {
                    fis.close();
                } catch (Throwable var4) {
                    var5.addSuppressed(var4);
                }

                throw var5;
            }
        }

ByteCode

    LINENUMBER 8 L2
   FRAME FULL [[Ljava/lang/String; java/io/FileInputStream] [java/lang/Throwable]
    ASTORE 2
   L3
    ALOAD 1
    INVOKEVIRTUAL java/io/FileInputStream.close ()V
   L4
    GOTO L9
   L5
```

### 예외 던지기 throws

> 기본적으로 예외처리는 try-catch 문으로 하는게 보통이지만, 자신을 부른 메섣드에 Exception문을 넘겨서 처리할 수 있다. 예를들어보자 **method1에서 method2를 처리하는데 예외가 발생한다면, 이 예외는 method2에서 발생할 수도 있지만, method1 에 예외를 던져줄 수 있다는 것이다.** 그림으로 한번 예를 들어보자

![image](https://user-images.githubusercontent.com/70433341/104186680-1b7e8b80-545a-11eb-862e-f8760f2dfdee.png)

다음 과 같이 그림을 보면 이해가 갈 수 있을 것이다. 그럼 코드를 통해서 보도록 하자.

```java
    public static void method1(){
        try{
            method2();
        }catch (ClassNotFoundException e){ // method2()의 ClassNotFoundException 시행
            e.printStackTrace();
        }finally {
            System.out.println("finish");
        }
    }

    private static void method2() throws ClassNotFoundException { // 바로 method1 catch문으로 ㄱ
        Class clazz = Class.forName("java.lang.절대없는class");
    }

    public static void main(String[] args) {
        method1();
    }
```

이런식으로 **method2( )** 에서 발생할 수 있는 ClassNotFoundException을 **method1( )의 try - catch 에 작성하도록 한다.** 이를 우리는 예외 떠넘기기 라고 한다.

---

### 사용자 정의 예외 및 예외 발생

> 사용자 정의 예외는 왜 배워야 하는 것일까, 사실 지금 예시로 작성한 모든 예외는 **자바 문법에서 발생할 수 있는** 예외일 뿐, 실제로 우리가 어플리케이션에서 발생할 수 있는, 예외를 커버하기에는 예외가 적다. 자바 입장에서는 만들어주지 않아도 되고 말이다. 예를 들어보자

```java
public class Student{
    int studentNum;
    //학생 번호
    String name;
    //학생 이름
}
```

-   1.  만약 학생이라는 클래스가 존재한다고 가정하고, studentNum과 name이란 맴버변수를 작성한다.
-   2.  한개의 학급에 studentNum 즉, 학생번호는 1번부터 20번까지만 가능하다고 가정해보자
-   3.  만약 studentNum, 학생번호가 20을 초과하면 어떤 Exception을 작성해야될까?
-   4.  이런 상황을 예방하고자, 커스텀 예외를 작성해보자

```java
public class OutOfStuNumException extends Exception{
    public OutOfStuNumException(){} // 기본 생성자
    public OutOfStuNumException(String message){
        super(message); // 에러메세지 생성자
    }
}
```

> 사용자 정의 예외 클래스는 컴파일러가 체크하는 일반 예외로 선언이 가능하며, 컴파일러가 체크하지 않는 실행 예외로 선언할 수 있다. 클래스를 선언할 경우에는 클래스 네임 끝에 ~Exception을 붙혀주면 좋다. **2가지 생성자를 선언하여, 1개의 생성자는 기본생성자로 작성하고, 또다른 한개는 예외 오류문을 작성할 message를 작성할 수 있도록 해주면 된다.**

그럼 이 예외문을 어떻게 발생시킬 수 있을까? throw new 키워드를 사용해보도록 하자.

```java
    public void addStuNum(Student student) throws OutOfStuNumException{
        if(studentNum > 20){
            throw new OutOfStuNumException("학생번호가 20을 초과할 수 없습니다.");
        }else{
            studentNum++; // 학생 번호 증가 
        }
    }
```

-   다음과 같이 학생 번호를 추가시키는 addStuNum 을 작성하였다. 만약 학생의 수 studentNum이 2을 초과하게 되면 발생할 수 있도록 하였다