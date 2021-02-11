**Java Thread**

## java Thread , Multi Thread

### Multi Thread 란

Thread class, Runnable interface 사실 이 둘은 궁극적으로 멀티 쓰레드를 지원하기 위한 녀석들이다. 이 둘을 알아보기 전에 우리는 멀티스레드에 대해서 좀더 알아봐야할 필요가 있다. 나는 처음에 멀티쓰레드에 대해서 잘 이해하지 못했다. **한거번에 2가지의 일을 한다는게 이게 말이 되는 걸까..?**

간단하게 생각해보자 인간은 태초에 선형으로 생각하고 행동해 왔다. 다음의 그림으로 예를 들어보도록 하자.

![image](https://user-images.githubusercontent.com/70433341/104879686-aeab4a00-59a1-11eb-83e6-7b415ecf4057.png)

사람은 기본적으로 다음과 같이 행동하게 된다. 밥을 먹으면서 낮잠을 잔다거나, 잠을 자면서 공부를 한다는게 말이 안되는 것이다. 하지만 컴퓨터는 가능하다. 아니 가능한 것처럼 보이게 했다.

![image](https://user-images.githubusercontent.com/70433341/105002190-3a8da680-5a74-11eb-98bb-e5cc8ca2db7d.png)

이런 방식으로 컴퓨터는 일을 처리하는게 가능하게 끔 보이게 한다. 이유는 간단하다. 부드러운 user experience 를 제공해주기 위해서 이다. 우리가 만약 구글 크롬 또는 Outlook 메일 함을 사용하면서, 파일을 다운 받도록 해보자, 그러면 이런 멀티 쓰레드 환경이 아닌, 싱글 쓰레드로 진행했다고 생각해보자 파일이 5G 로 꽤 큰 용량을 다운 받아야하고 다운받아야 하는 시간이 1시간 이라면, 다운로드 버튼을 클릭 하는 순간 우리는 그 프로그램이 다운로드가 동작이 될 때, 까지 멈춰 있어야 한다. 그래서 초기에 어떻게 해결을 해왔을까? 그건 바로 컴퓨터가 번갈아 가면서 일을 처리해 주는 것이다. 위의 그림은 사실 우리 눈으로 봐왔을 때 저렇게 보이는 것 뿐이지 실상에서는 다음과 같은 그림이 좀 더 컴퓨터가 동작하는 방식과 같다고 생각할 수 있다.

![image](https://user-images.githubusercontent.com/70433341/104880899-b53ac100-59a3-11eb-9525-66d122cd5bb9.png)

### 멀티 쓰레드의 오버헤드? (컨텍스트 스위칭)

사실 컴퓨터 자체가 또는 프로그램 자체가 **나루토 처럼 그림자 분신술을 해서, 여러명이 작업하는 그런 용빼는 제주가 있는게 아니구나..** 라고 생각할 수 있다. 맞다. 그렇기 때문에, 이런 멀티쓰레드 환경은 잘못 자칫 하면 **오버헤드를 발생시킬 수도 있다.** 이 얘기가 무슨 얘기냐면, 컴퓨터도 결국엔 기억을 해야 된다는 것이다.

**\- 밥을 먹고 있다. 멸치 볶음을 먹고, 다음 할일은 제육볶음을 먹는 일이다. 멸치볶음을 먹다가 JPA 공부를 하러 가기 시작한다.**

**\- JPA 공부를 어느정도 끝내고 밥을 먹어야 하는데, 내가 무슨반찬을 먹고 있었지? '아 멸볶 먹고있었지~' 멸치볶음을 마저 먹어야 겠다.**

**\- 이제 낮잠을 자야하는데.. 잠이 잘 안오네?**

사실 이런 과정들이 전부 컴퓨터 한테는 그 전에 하던일을 기억해야 하기 때문에 오버헤드가 발생할 수 있다는 얘기이고, 좀 더 옳바른 멀티쓰레딩 구조를 가져야 하는게 개발자가 해야할 일이다. 그럼에도 멀티 쓰레딩을 지원해야 하는 이유는 보다 부드러운 **UX(User Experience) 때문이다 라고 말할 수 있겠다.**

---

## Main Thread

전에 Multi Thread 에 대해서 이야기를 했다. 그럼 멀티 쓰레드에 대해서 더 알아보기전에 우리는 MainThread에 대해서 알아볼 필요가 있다.

```java


public static void main(String args[]){
    System.out.println("Main Thread 실행")
}
```

모든 자바 어플리케이션에서 메인 쓰레드는 다음과 같은 main 메소드를 통해서 실행하게 된다. 메인 메소드가 실행이 되면 코드 한줄 한줄 순차적으로 시작하게 되고, **return** 을 만나거나, main 메서드의 끝이오면 종료하게 된다.

```java


public class App {
    public static void main(String[] args) {
        System.out.println("Main Thread 시작");

        for (int i = 0; i < 10001; i++) {
            System.out.println("hello");
        }

        return; // 종료
    }
}
```

이런 main 메소드만 존재하는 상황을 싱슬 스레도 어플리케이션이라고 하는데, 메인 쓰레드가 종료 되면, 프로세스 자체도 종료된다. 정확히는 프로세스 내에 쓰레드가 모두 종료 되어야 종료 된다느게 맞는 얘기일 것이다. 이런 메인 쓰레드 구조에서 우리 개발자들은 작업 쓰레드를 여러개 생성하여, 멀티 쓰레드를 구성 할 수 있는데, 작업 쓰레드를 구성하기 이전에 먼저 그림을 통해 한번 확인해 보자,

![image](https://user-images.githubusercontent.com/70433341/104884266-6f80f700-59a9-11eb-9d2c-c95d594f9426.png)

다음과 같이 main 쓰레드가 끝나고도 Thread2 가 끝나야 프로세스가 종료되게 된다. **하지만 데몬 쓰레드 에서는 예외로 하자**

#### 데몬 쓰레드

데몬 쓰레드는 main 쓰레드를 보조하는 쓰레드를 이야기 한다. **보조를 하는 역할 이기 때문에, 메인쓰레드가 종료되면 데몬 쓰레드도 강제적으로 종료된다.** 이게 무슨 이야기인지 잘 모르겠지만 예를 들어보도록 하자. 예를 들어서 크롬의 유튜브 재생을 볼 수 있다.

![image](https://user-images.githubusercontent.com/70433341/104894716-65b2c000-59b8-11eb-9f07-7f2d21d2679b.png)

크롬이라는 메인 메소드가 실행이 되면서, 유튜브의 백그라운드 재생, google docs 의 문서 자동저장 등을 데몬 쓰레드라고 볼 수 있다. 데몬 쓰레드는 어떻게 지정할 수 있을까?

```java
    public static void main(String[] args) {

        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                for(int i = 0; i < 100; i++){
                    System.out.println("유튜브 영상 실행중");
                    try {
                        Thread.sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
        thread.setDaemon(true); // 데몬 쓰레드 설정
        thread.start();
        System.out.println("메인 메소드 종료");
    }
output
메인 메소드 종료
유튜브 영상 실행중
```

메인메소드 종료가 되면서 유튜브 영상 실행중이라는 코드 하나를 출력하고 종료되고 만다.

```java
   public static void main(String[] args) {

        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                for(int i = 0; i < 100; i++){
                    System.out.println("유튜브 영상 실행중");
                    try {
                        Thread.sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
        thread.setDaemon(flase); // 데몬 쓰레드 설정 -> false
        thread.start();
        System.out.println("메인 메소드 종료");
    }
output
메인 메소드 종료
유튜브 영상 실행중
유튜브 영상 실행중    
유튜브 영상 실행중    
유튜브 영상 실행중    x 100
```

만약 deamon 쓰레드를 설정을 false 로 하면 지속적으로 유튜브 영상이 계속 실행되는걸 확인할 수 있다.

---

### 작업 쓰레드 생성하기

#### Thread 와 Runnable

쓰레드에 대해서 대충 알게 되었으니, 멀티 쓰레드를 하기 위해서, 어떻게 작업단위를 나누어 주는지 확인해 보자. 자바에서는 **java.lang.Thread** 를 통해서 쓰레드 객체를 직접 생성할 수 있는데, 예전에 나왔던, Exception 예외 처럼 쓰레드도 클래스로 관리할 수 있도록 한다.

```java
Thread thread = new Thread(Runnable target);
```

다음과 같이 Runnable 을 파라미터로 가진 생성자를 호출 할 수 있다. 저번 [인터페이스](https://catch-me-java.tistory.com/45) 에서 알 수 있듯이, Runnable, ~ble 로 끝나는데, 눈치를 챘을 수 있지만 Runnable 은 인터페이스의 특징을 가지고 있다. 그러면 Runnable을 파라미터로 가지는 이유는 무엇일까? **Runnable 에는 run() 메소드 하나가 정의되어있는데 이는 Thread가 실행하여, 작업해야 하는 코드를 말한다.** 그래서 Thread의 클래스 파일을 보면,

```java
public class Thread implements Runnable {
    //code
}
```

다음과 같이 Runnable 인터페이스를 implements 한걸 볼 수 있다. 즉 **Runnable 은 Thread의 작업 내용을 가지고 있는 객체일 뿐이지 실제 쓰레드는 아니라는 것이다.** Runnable을 사용하는 몇가지 예를 들업도록 하자.

```java


package example;

public class Task implements Runnable {

    @Override
    public void run() {
        System.out.println("task 실행");
    }
}

package example;

public class App {
    public static void main(String[] args) {
        Runnable task = new Task();

        Thread thread = new Thread(task);
    }
}
```

이와 같이 Task 클래스에 Runnable 인터페이스를 implements 한 뒤, run() 메소드를 오버라이딩 해서 구현체를 작성하도록 한다. \*\*만약 작업 단위가 작거나, 한번밖에 쓰이지 않을 Thread 작업이라면, interface를 스코프 안에서 생성하는 것도 좋은 방법이다.

```java
  public static void main(String[] args) {
        //Runnable 함수형 인터페이스 구현
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Task 실행");
            }
        });
        thread.start();
        // 함수형 인터페이스 람다식으로 변경
        Thread thread1 = new Thread(() ->
                System.out.println("Task2 실행")
        );
        thread1.start();
    }
```

이런 쓰레드의 작업이 어떻게 진행이 되는지, 그림을 통해서 확인을 해보도록 하자.

![image](https://user-images.githubusercontent.com/70433341/104890861-7ca2e380-59b3-11eb-9754-235b7cf8df95.png)

---

## Thread 우선순위

아까 컴퓨터가 어떻게 멀티 쓰레딩을 지원하는지에 대해서 얘기를 했다. 실질적으로 여러개의 작업을 끊었다. 지속했다. 하는 식으로 말이다. 하지만 꼭 그렇지는 않다. 저 이야기는 정말 90년대의 **팬티엄 cpu 정도의 코어가 1개 있을 때를 이야기 한다.**

### 동시성 (Concurrency) or 병렬성 (Parallelism)

쓰레드에서 말하는 동시성에 대해서 명확하게 이해할 필요가 있는데, 동시성이란, 코어 1개가 여러개의 쓰레드 작업 테이블을 왔다 갔다 하면서 작업을 하는 것이다. 보통 쓰레드의 개수가 코어의 갯수 보다 많을 경우, 동시성으로 멀티 쓰레드를 지원하게 된다.

그렇지 않고, 만약 컴퓨터가 좋아서(코어 개수가 많거나), 작업을 해야할 쓰레드가 코어보다 적을 경우 병렬성으로, 각 코어마다 task Thread 를 할당 받아서 작업하게 된다.

![image](https://user-images.githubusercontent.com/70433341/104912114-365b7d80-59cf-11eb-874c-9b4401eb34d9.png)

그럼 core 1개가 쓰레드를 처리할 때 우선순위를 어떻게 지정할까? 분명 Thread1 과 Thread2가 있을 때, 어떻게 처리를 해야할까?

#### 자바 스레드 스케줄링

스레드 스케줄링은 크게 2가지 방식을 이용하게 된다.

-   우선순위 : 우선순위가 높은 스레드가 실행 상태를 더 많이 가지도록 스케줄링을 하는 것을 말한다.

```java


package example;

public class App {
    public static void main(String[] args) {
        System.out.println("메인쓰레드 시작");
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                for(int i =0; i < 1000; i++){
                    try {
                        Thread.sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println("우선순위 : " + Thread.currentThread().getPriority());
                }
            }
        });
        thread.setPriority(1);
        Thread thread2 = new Thread(new Runnable() {
            @Override
            public void run() {
                for(int i =0; i < 1000; i++){
                    try {
                        Thread.sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println("우선순위 : " + Thread.currentThread().getPriority());
                }
            }
        });
        // 10이 제일 높고 1이 제일 낮음
        thread.setPriority(10);
        thread2.setPriority(1);
        thread.start();
        thread2.start();
        System.out.println("메인쓰레드 종료");
    }
}

output: 
메인쓰레드 시작
메인쓰레드 종료
우선순위 : 10
우선순위 : 1
우선순위 : 10
우선순위 : 1
    ... 반복
```

쓰레드의 우선순위를 정해주는 방법은 크게 2가지가 있다. 첫번째로는 우선순위의 매개값으로 1~10까지의 우선순위를 주는건데 **10이 제일 높으며 1이 우선순위가 제일 낮다.** 또는 **MAX\_PRIORITY , NORM\_PRIORITY, MIN\_PRIORITY 로 각자 10 , 5, 1 로 지정하여 쓰레드의 우선순위를 지정할 수 있다.**

-   순환 할당 : 순환 할당이라 함은, 각자 시간 할당량을 정해서 하나의 쓰레드를 정해진 시간만큼만 실행하고 다시 다른 스레드를 실행하는 것을 말하는데, 이런 순환 할당 방식은 JVM이 정하기 때문에 코드로 제어할 수는 없다.

---

### 스레드 동기화

싱글 스레드 즉, main 스레드 한개로 구성된 어플리케이션에서는 문제가 되지 않겠지만, 만약 멀티 스레드 상황에서 객체를 생성을 한다면, 여러개의 스레드가 객체를 건들 수 있는 상황이 생길 수 있다. 스레드 A를 사용하던 객체가 만약 스레드 B에 의해 객체의 값이 변경 될 수 있다는 뜻이다. 그림으로 예를 들어보자.

**게임 캐릭터는 나만 건들 수 있어야 하는데, 내가 화장실을 간 사이에 누군가가 와서 템을 바꿔 놓는다면 얼마나 짜증이 나겠는가?**

![image](https://user-images.githubusercontent.com/70433341/104919030-f6999380-59d8-11eb-8378-697f95c7aa34.png)

```java


package example;

public class App {
    public static void main(String[] args) {
        System.out.println("소환사의 협곡에 오신걸 환영합니다.");
        Ash ash = new Ash();
        User1 user1 = new User1();
        user1.setAsh(ash);
        user1.start();

        User2 user2 = new User2();
        user2.setAsh(ash);
        user2.start();
    }
}

---

public class Ash extends Thread {

    String item = " ";

    public void setItem(String item){
        this.item = item;
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        System.out.println(Thread.currentThread().getName() + "현재 아이템 : " + this.item);
    }

    public String getItem(){
        return item;
    }
}

---

public class User1 extends Thread {
    private Ash ash;
    public void setAsh(Ash ash){
        this.setName("User1");
        this.ash = ash;
    }

    @Override
    public void run(){
        ash.setItem("도란검");
    }
}
---

public class User2 extends Thread{
    private Ash ash;
    public void setAsh(Ash ash){
        this.setName("User2");
        this.ash = ash;
    }

    @Override
    public void run(){
        ash.setItem("도란링");
    }
}

outPut :
User2현재 아이템 : 도란링
User1현재 아이템 : 도란링

```

이런 상황에서 지금 사용중인 객체(애쉬)를 다른 스레드가 변경할 수 없도록 하려면 스레드가 작업이 끝날 때 까지 객체에 잠금을 걸어서 다른 스레드가 사용할 수 없도록 해야한다.

-   임계 영역 : 멀티 스레드에서 단 하나의 스레드만 실행할 수 있는 코드 영역을 임계영역 이라고 한다.

자바는 이런 임계영역을 지정하기 위해서 동기화 메소드와 동기화 블록을 제공하게 된다. 동기화 메소드를 만드는 방법은 메소드 선언에 **synchronized** 키워드를 붙이면 된다.

수정된 코드를 확인해보면 정상적으로 각자 아이템이 출력되는걸 확인할 수 있다.

```java


package example;

public class Ash extends Thread {

    String item = " ";

    public synchronized void setItem(String item){
        this.item = item;
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        System.out.println(Thread.currentThread().getName() + "현재 아이템 : " + this.item);
    }

    public String getItem(){
        return item;
    }
}

output : 
소환사의 협곡에 오신걸 환영합니다.
User1현재 아이템 : 도란검
User2현재 아이템 : 도란링
```

물론 메소드에 syncronized 를 붙히는 방법도 있긴 하지만, 동기화 블록을 선언하는 것도 좋은 방법중 하나이다.

![image](https://user-images.githubusercontent.com/70433341/104919707-05cd1100-59da-11eb-96e4-25cefb8c360a.png)

---

## 스레드 상태 & 교착 상태

코드로 보면 Thread 의 실행시점은 **.start() 메소드로 실행을 하는 것 처럼 보이지만 실제로는 그렇지 안다.** start() 메소드를 실행하게 되면, 스레드의 실행 대기 상태가 되고, 스케줄러가 스레드를 실행시키는 방식으로 되어있다.

![image](https://user-images.githubusercontent.com/70433341/104923501-7aef1500-59df-11eb-8569-1684ed7281b0.png)

스레드에 .start() 메소드를 호출하면 실행 대기 상태에 돌입하게 된다. 실행 대기상태에 들어간 스레드는 스케줄러를 통해서 실행상태와 실행 대기 상태를 반복적으로 왔다갔다 하게 되는데, **java5 에서 추가된 thread의 현재 상태를 출력해주는 .getState() 메소드를 이용해서 현재 상태를 출력해보도록 하자**

```java


public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("스레드 시작");
            }
        });

        System.out.println(thread.getState()); // 스레드의 new 상태 

        thread.start();

        System.out.println(thread.getState()); // 스레드의 new 상태 

}
output :
NEW //스레드 객체가 생성, 아직 메소드가 호출되지 않은 상태
스레드 시작
RUNNABLE // 실행 대기 상태 , 실행 상태로 언제든지 들어갈 수 있는 상태
```

그러면 일시정지 상태는 언제 발생하는 것일까? 스레드 내에서 Thread.sleep 또는 Object 메소드인 wait 또한 thread 를 일시 정지 상태로 만들게 된다.

```java


   public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("스레드 시작");
                try {
                    Thread.sleep(1000); // 스레드 일시 정지 시키기
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        thread.start();

        Thread.sleep(50); 
        System.out.println(thread.getState());
    }
output :
스레드 시작
TIMED_WAITING // Thread.sleep(1000); 코드로 인해 일시 정지 상태가 진행됨, 주어진 시간동안 기다리는 상태
```

물론 suspend() 메소드 또는 wait 메소드를 사용할 수 있지만, suspend() 메소드는 **데드락을 일으키기 쉽기 때문에 resume(), stop() 메소드와 함께 deprecated 되었다.** 그럼 데드락은 뭘까?

### 데드락 (죽음의 포응)

![image](https://user-images.githubusercontent.com/70433341/104930680-d1ad1c80-59e8-11eb-8b2f-2e7800ea218d.png)

이 그림을 보면 데드락 상황에 대해서 좀 더 이해하기 쉬울 것이다.

-   스레드 1 은 자원 1을 점유하고 있는 상태에서 자원 2가 필요하다.
-   스레드 2 는 자원 2를 점유하고 있는 상태에서 자원 1이 필요한 상황이다.
-   하지만 스레드 1은 자원 2가 필요한 상황에서 자원 1을 빌료줄 수 있는 상황이 아니고,
-   스레드 2또한 자원 1이 필요한 상태에서 자원 2를 빌려줄 수 없는 상황인 것이다.

서로 절대 불가능한 일을 계속적으로 기다리는 상황을 이야기 한다. 이걸 코드로 해석해 보자.

```java


package example;

public class App {
    public static Object 자원1 = new Object();
    public static Object 자원2 = new Object();
    private static class ThreadDemo1 extends Thread {
        public void run() {
            synchronized (자원1) {
                System.out.println("스레드 1: 자원1 점유 중");

                try { Thread.sleep(10); }
                catch (InterruptedException e) {}
                System.out.println("스레드 1: 자원2 사용 대기 중");

                synchronized (자원2) {
                    System.out.println("스레드 1 : 자원1 & 2 점유 중");
                }
            }
        }
    }

    private static class ThreadDemo2 extends Thread {
        public void run() {
            synchronized (자원2) {
                System.out.println("스레드 2: 자원2 점유 중");

                try { Thread.sleep(10); }
                catch (InterruptedException e) {}
                System.out.println("스레드 2: 자원1 사용 대기 중");

                synchronized (자원1) {
                    System.out.println("스레드 2: 자원1 & 2 점유 중");
                }
            }
        }
    }
    public static void main(String[] args) throws InterruptedException {
        ThreadDemo1 t1 = new ThreadDemo1();
        ThreadDemo2 t2 = new ThreadDemo2();
        t1.start();
        t2.start();

        Thread.sleep(1000);
        System.out.println(t1.getState()); // 현재 스레드 상태를 출력 BLOCKED
        System.out.println(t2.getState()); // 현재 스레드 상태를 출력 BLOCKED
    }
}

output : 

스레드 2: 자원2 점유 중
스레드 1: 자원1 점유 중
스레드 1: 자원2 사용 대기 중
스레드 2: 자원1 사용 대기 중
BLOCKED
BLOCKED

    // 지속적으로 사용대기 상태, 데드락 발생
```

여기서 각각 t1 과 t2에게 현재 상태를 출력하는 메소드를 부르면 BLOCKED 상태가 되있는걸 볼 수 있을 것이다.

**BLOCKED 메소드는 사용하고자 하는 객체의 락이 풀릴 때 까지 기다리는 상태** 를 이야기 하며, 지금 데드락 상황에서는 **BLOCKED 상태**가 지속적이다.

### 스레드 양보

스레드 양보라는 개념은 간단하다 현재 진행하고 있는 작업이 있다면, 이 작업을 다른 스레드에게 양보하는걸 의미하는데 코드를 통해 확인해 보자

```java


package example;

public class App {
    private static class ThreadDemo1 extends Thread {
        public ThreadDemo1(String name){
            super(name);
        }
        public void run() {
            for (int i = 1; i <= 5; i++) {
                if((i % 5) == 0){
                    System.out.println(Thread.currentThread() +" yield 진행 ");
                    Thread.yield(); //스레드를 양보 Runnable 상태로 
                }else{
                    System.out.println(Thread.currentThread() +" 실행중");
                }
            }
        }
    }
    public static void main(String[] args) {
        new ThreadDemo1("스레드 1").start();
        new ThreadDemo1("스레드 2").start();
    }
}

output : 
Thread[스레드 1,5,main] 실행중
Thread[스레드 1,5,main] 실행중
Thread[스레드 1,5,main] 실행중
Thread[스레드 1,5,main] 실행중
Thread[스레드 1,5,main] yield 진행 
Thread[스레드 2,5,main] 실행중
Thread[스레드 2,5,main] 실행중
Thread[스레드 2,5,main] 실행중
Thread[스레드 2,5,main] 실행중
Thread[스레드 2,5,main] yield 진행 
```

스레드.yield(); 메소드를 사용하게 되면, 현재 진행중이던 스레드가 Runnable 상태로 돌아가게 된다.

![image](https://user-images.githubusercontent.com/70433341/104998756-18455a00-5a6f-11eb-84d2-f15870fb9461.png)

각자 상태를 제어할 수 있는 메소드는 다음과 같다.

---

## Thread pool

개발을 하다보면 스레드 풀에 대해서 들어본적이 있을 것이다. 그럼 스레드풀이 왜 필요한지에 대해서 알아보도록 하자.

### 기존 스레드의 문제점

-   정확히는 문제점이라기 보다는, 스레드를 생성하는데 드는 비용이 많다는 것이다.
-   스레드 생성과 스케줄링으로 인해 CPU가 바빠지고, 메모리 사용량이 늘어난다.

결국에는 스레드를 생성하고 죽이는 과정 자체가 컴퓨터의 cpu 그리고 어플리케이션 자체에 무리를 준다는 것이다. 그렇기 때문에 이런 생각을 하게 된다. **스레드를 미리 생성해 놓고 각자 일을 부여시켜주면 되지 않을까?** 이다.

![image](https://user-images.githubusercontent.com/70433341/105000672-f1d4ee00-5a71-11eb-8649-0306a1dd912a.png)

다음과 같이 해야할일을 task queue 라는 명목하에 집어 넣어준다. 그리고 queue 에 들어간 task를 각자의 스레드에게 부여해주는데,

특징

**1\. 할일보다 스레드가 부족하다고 스레드를 더 생성하지 않는다.**  
**2\. 스레드가 일이 끝난다고, 종료하는게 아니라 queue 에 들어간 다른 작업을 할당 받는다.**

라는게 큰 개념으로 보면된다. 실제 코드를 보게 되면

```java


  public static void main(String[] args) throws Exception{
            ExecutorService executorService = Executors.newFixedThreadPool(4); 
              // 스레드개수 4개

            for(int i=0; i<10; i++){
                Runnable runnable = new Runnable() {
                    @Override
                    public void run() {
                        // 스레드 총 개수 및 작업 스레드 이름 출력
                        ThreadPoolExecutor threadPoolExecutor = (ThreadPoolExecutor) executorService;
                        int poolSize = threadPoolExecutor.getPoolSize();  // poolSize 총 스레드 개수
                        String threadName = Thread.currentThread().getName();
                        System.out.println("[총 스레드 개수 : " + poolSize + "] 작업 스레드 이름 : " + threadName);

                    }
                };
                executorService.submit(runnable);

                Thread.sleep(10);
            }
            executorService.shutdown();
        }

output : 
[총 스레드 개수 : 1] 작업 스레드 이름 : pool-1-thread-1
[총 스레드 개수 : 2] 작업 스레드 이름 : pool-1-thread-2
[총 스레드 개수 : 3] 작업 스레드 이름 : pool-1-thread-3
[총 스레드 개수 : 4] 작업 스레드 이름 : pool-1-thread-4
[총 스레드 개수 : 4] 작업 스레드 이름 : pool-1-thread-1
[총 스레드 개수 : 4] 작업 스레드 이름 : pool-1-thread-2
[총 스레드 개수 : 4] 작업 스레드 이름 : pool-1-thread-3
[총 스레드 개수 : 4] 작업 스레드 이름 : pool-1-thread-4
[총 스레드 개수 : 4] 작업 스레드 이름 : pool-1-thread-1
[총 스레드 개수 : 4] 작업 스레드 이름 : pool-1-thread-2

Process finished with exit code 0
```

실제로 스레드 개수가 4개로만 고정되어 있으며, 더이상 늘리지도 줄이지도 않는다.

스레드 풀을 선언하는 방법이 여러가지가 있는데 , 앞 선 코드는 스레드의 총 개수만 설정해두었지만, 직접 생성자를 호출하게 되면 세부적으로 설정할 수 있다.

```java
 // 1번 방법
ExecutorService ex = new ThreadPoolExecutor(
    3, //코어 스레드 개수
    100,// 최대 스레드 개수
    120L,// 스레드 놀고 있는 시간
    TimeUnit.SECONDS,//시간 단위
    new SynchronousQueue<>()  // 작업큐
);
 // 2번 방법
public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
}
```

1번 방법은 여러가지를 설정해 줄 수 있지만 그와 다르게 **newFixedThreadPool 메소드는 스레드 개수와 최대 스레드 개수는 동일하며, 스레드의 대기시간 은 없는 것으로 자동 default 된다.**