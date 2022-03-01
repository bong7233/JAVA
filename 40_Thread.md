# Thread
- process : 실행 중인 프로그램, 프로그램이 실행되면 OS로 부터 메모리를 할당받아 프로세스 상태가 됨
- thread : 하나의 프로세스는 하나 이상의 thread를 가지게 되고, 실제 작업을 수행하는 단위는 thread  
![thread](./img/thread.png)

<br>

## multi-threading 
- 여러 thread가 동시에 수행되는 프로그래밍, 여러 작업이 동시에 실행되는 효과
- thread는 각각 자신만의 작업 공간을 가짐 ( context )
- 각 thread 사이에서 공유하는 자원이 있을 수 있음 (자바에서는 static instance)
- 여러 thread가 자원을 공유하여 작업이 수행되는 경우 서로 자원을 차지하려는 race condition이 발생할 수 있음
  - 이렇게 여러 thread가 공유하는 자원중 경쟁이 발생하는 부분을 critical section 이라고 함
  - critical section에 대한 동기화( 일종의 순차적 수행)를 구현하지 않으면 오류가 발생할 수 있음  
![critical](./img/critical.PNG)

<br>

## Thread 클래스 상속하여 만들기
```java
class MyThread extends Thread{
	
	public void run() {
		
		int i;
		for(i = 0; i<200; i++) {
			System.out.print(i + "\t");
		}
	}
}

public class ThreadTest {

	public static void main(String[] args) {

		System.out.println(Thread.currentThread());
		MyThread th1 = new MyThread();
		th1.start();
		
		MyThread th2 = new MyThread();
		th2.start();
	}

}
```

## Runnable 인터페이스 구현하여 만들기
- 자바는 다중 상속이 허용되지 않으므로 이미 다른 클래스를 상속한 경우 thread를 만들기 위해 Runnable interface를 구현하도록 한다.
```java
class MyThread2 implements Runnable{

	public void run(){
		
		int i;
		for(i=0; i<200; i++){
			
			System.out.print(i + "\t");
	
			
		}
	}
}


public class ThreadTest2 {

	public static void main(String[] args) {

		System.out.println("main start");
		
		MyThread2 mth = new MyThread2();
		Thread th1 = new Thread(mth);
		th1.start();
		
		Thread th2 = new Thread(new MyThread2());
		th2.start();
		
		System.out.println("main end");
	}

}
```
<br>

## Thread Status
```
프로그램이 실행되면 -> Runnable한 쓰레드들이 쓰레드풀에서 CPU배분을 기다린다 -> start가되면 시작하고 스레드가종료되면 Dead상태가된다

sleep, wait, join 등을 사용하여 좀비상태인 Not Runnable상태로 만들수있고 이때는 CPU를 점유할 수 없다
각각의 좀비상태의 쓰레드는 각각 다른메서드로 다시 Runnable한 상태로 변경될 수 있다
```
![status](./img/status.png)  
<br>

## Thread 우선순위
- Thread.MIN_PRIORITY(=1) ~ Thread.MAX_PRIORITY(=10)
- 디폴트 우선순위 : Thread.NORMAL_PRIORITY(=5)
- 우선 순위가 높은 Thread가 CPU의 배분을 받을 확률이 높다
- setPriority()/getPriority()
```java
// Thread 우선순위 예제
class PriorityThread extends Thread{
	
	public void run(){
	
		int sum = 0;
		
		Thread t = Thread.currentThread();
		System.out.println( t + "start");
		
		for(int i =0; i<=1000000; i++){
			
			sum += i;
		}
		
		System.out.println( t.getPriority() + "end");
	}
}


public class PriorityTest {

	public static void main(String[] args) {

		int i;
		PriorityThread pt1 = new PriorityThread();
		PriorityThread pt2 = new PriorityThread();
		PriorityThread pt3 = new PriorityThread();
		
        
        pt1.setPriority(Thread.MIN_PRIORITY);
        pt2.setPriority(Thread.NORM_PRIORITY);
        pt3.setPriority(Thread.MAX_PRIORITY);

		pt1.start();
        pt2.start();
        pt3.start();
		
        // MAX(10) -> NORM(5) -> MIN(1) 순으로 작업이 끝난다
	}
}
```
<br>

## join()
- 동시에 두 개 이상의 Thread가 실행 될 때 다른 Thread의 결과를 참조 하여 실행해야 하는 경우 join() 함수를 사용
- join() 함수를 호출한 Thread가 not-runnable 상태가 감
- 다른 Thread의 수행이 끝나면 runnable 상태로 돌아옴  
![join](./img/join.png)

```java
// 1부터 50, 51부터 100 까지의 합을 구하는 두 개의 Thread를 만들어 그 결과 확인
public class JoinTest extends Thread{
	
	int start;
	int end;
	int total;
	
	public JoinTest(int start, int end){
		this.start = start;
		this.end = end;
	}
	
	public void run(){
	
		int i;
		for(i = start; i <= end; i++){
			total += i;
		}
	}
	

	public static void main(String[] args) {

		JoinTest jt1 = new JoinTest(1, 50);
		JoinTest jt2 = new JoinTest(51, 100);
		
				
		jt1.start();
		jt2.start();
		
		try{
			jt1.join();
			jt2.join();
			
		}catch (InterruptedException e) { // join이나 wait을 사용할떄는 예외처리를 해줘야 한다
			System.out.println(e);
		}
		
        int lastTotal = jt1.total + jt2.total;
        //만약 join 을 하지않으면 lastTotal은 main 스레드에서 가장먼저 종료되버리므로 나머지 스레드 두개의 연산이 끝나기전에 0으로 선언되어버림
				
		System.out.println("jt1.total = " + jt1.total); // 1275
		System.out.println("jt2.total = " + jt2.total); // 3775
		
		System.out.println("lastTotal = " + lastTotal); // 5050		
	}

}
```
<br>

## interrupt()
- 다른 Thread에 예외를 발생시키는 interrupt를 보낸다.
- Thread가 join(), sleep(), wait() 함수에의해 not-runnable 상태일 때 interrupt() 메서드를 호출하면 다시 runnable 상태가 될 수 있음
<br>

## Thread 종료하기
- Thread를 종료할 때 사용함
- 무한 반복의 경우 while(flag)의 flag 변수값을 false로 바꾸어 종료를 시킴  

```
## Thread 종료하기 예제 ##
세 개의 thread를 만든다.
각각 무한 루프를 수행하게 한다. 	
작업 내용 this.sleep(100);

‘A’ 를 입력 받으면 첫 번째 thread를
‘B’ 를 입력 받으면 두 번째 thread를
‘C’ 를 입력 받으면 세 번째 thread를 
‘M’을 입력 받으면 모든 thread와 main() 함수를 종료한다.
```
```java
public class TerminateThread extends Thread{

	private boolean flag = false;
	int i;
	
	public TerminateThread(String name){
		super(name);
	}
	
	public void run(){
		
		
		while(!flag){
			try {
				sleep(100);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
		
		System.out.println( getName() + " end" );
		
	}
	
	public void setFlag(boolean flag){
		this.flag = flag;
	}
	
	
	public static void main(String[] args) throws IOException {

		TerminateThread threadA = new TerminateThread("A");
		TerminateThread threadB = new TerminateThread("B");
		TerminateThread threadC = new TerminateThread("C");
		
		threadA.start();
		threadB.start();
		threadC.start();
		
		int in;
		while(true){
			in = System.in.read();
			if ( in == 'A'){
				threadA.setFlag(true);
			}else if(in == 'B'){
				threadB.setFlag(true);
			}else if( in == 'C'){
				threadC.setFlag(true);
			}else if( in == 'M'){
				threadA.setFlag(true);
				threadB.setFlag(true);
				threadC.setFlag(true);
				break;
			}else{
				System.out.println("type again");
			}
		}
		
		System.out.println("main end");
		
	}
}
```

