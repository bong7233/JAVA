# 큐(Queue) 구현
## Queue의 특징
- 맨 앞(front) 에서 자료를 꺼내거나 삭제하고, 맨 뒤(rear)에서 자료를 추가 함
- Fist In First Out (선입선출) 구조 
- 일상 생활에서 일렬로 줄 서 있는 모양
- 순차적으로 입력된 자료를 순서대로 처리하는데 많이 사용 되는 자료구조
- 콜센터에 들어온 문의 전화, 메세지 큐 등에 활용됨
- jdk 클래스 : ArrayList

## 연결 리스트를 활용하여 Queue 구헌

```java
// MyListQueue.java
import linkedlist.MyListNode; // 18_linkedList 사용
import linkedlist.MyLinkedList;

// 인터페이스 활용
interface Queue{
	public void enQueue(String data);
	public String deQueue();
	public void printAll();
}

public class MyListQueue extends MyLinkedList implements Queue{

	MyListNode front;
	MyListNode rear;
		
	
	public MyListQueue()
	{
		front = null;
		rear = null;
	}
	
    // 넣기
	public void enQueue(String data)
	{
		MyListNode newNode;
		if(isEmpty())  // 리스트가 비어있다면
		{
			newNode = addElement(data); // data를 newNode에 넣음
			front = newNode;
			rear = newNode;
		}
		else
		{ // 리스트가 비어있지 않아서 맨 뒤로 들어가는경우
			newNode = addElement(data);
			rear = newNode; // 맨뒤만 data로 바꿔주면 된다
		}
		System.out.println(newNode.getData() + " added");
	}
	
    // 빼기
	public String deQueue()
	{
		if(isEmpty()){ // 비어있다면 에러
			System.out.println("Queue is Empty");
			return null;
		}
		String data = front.getData(); // 맨 처음 요소를 찾고
		front = front.next; // 두번째 요소를 첫요소로 변경

		if( front == null ){  // 맨 마지막을 삭제하려면
			rear = null; // 맨마지막요소만 null로 변경
		}
		return data;
	}
	
	public void printAll()
	{
		if(isEmpty()){
			System.out.println("Queue is Empty");
			return;
		}
		MyListNode temp = front;
		while(temp!= null){
			System.out.print(temp.getData() + ",");
			temp = temp.next;
		}
		System.out.println();
	}
}
```

```java
// MyListQueueTest.java
public class MyListQueueTest {

	public static void main(String[] args) {

		MyListQueue listQueue = new MyListQueue();
		listQueue.enQueue("A"); // A added
		listQueue.enQueue("B"); // B added
		listQueue.enQueue("C"); // C added
		
		listQueue.printAll(); // A->B->C

		System.out.println(listQueue.deQueue()); // A
	}
}
```
