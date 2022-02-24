# 연결 리스트 (LinkedList) 구현
## LinkedList 특징
- 동일한 데이터 타입을 순서에 따라 관리하는 자료 구조
- 자료를 저장하는 노드에는 자료와 다음 요소를 가리키는 링크(포인터)가 있음
  - 자료를 추가하거나 삭제할때는 그 자료를 가리키는 포인터(바로앞의 노드)를 알아야 연결을 변경할 수 있다
- 자료가 추가 될때 노드 만큼의 메모리를 할당 받고 이전 노드의 링크로 연결함 (정해진 크기가 없음)
- 연결 리스트의 i 번째 요소를 찾는게 걸리는 시간은 요소의 개수에 비례 : O(n)
- jdk 클래스 : LinkedList

- 처음 요소를 헤드로 구성한 링크드리스트가있고, 헤드포인터를 따로 생성해서 포인터가 링크드리스트의 처음요소를 가르키도록 하는방법이있다
  - 예시는 첫구성요소를 헤드로 구성한 링크드리스트이다

##  LinkedList 구현하기
```JAVA
// MyListNode.java
public class MyListNode {

	private String data;       // 자료
	public MyListNode next;    // 다음 노드를 가리키는 링크
	
	public MyListNode(){
		data = null;
		next = null;
	}
	
	public MyListNode(String data){
		this.data = data;
		this.next = null; // 추가된 노드는 null을 가르키도록함
	}
	
	public MyListNode(String data, MyListNode link){
		this.data = data;
		this.next = link;
	}
	
	public String getData(){
		return data;
	}
}
```
```JAVA
// MyLinkedList.java
public class MyLinkedList {

	private MyListNode head;
	int count;
	
	public MyLinkedList()
	{
		head = null;  // 리스트의 첫번째 노트를 헤드로 설정
		count = 0;
	}
	
	public MyListNode addElement( String data )
	{
		
		MyListNode newNode; // 새로들어갈 노드
		if(head == null){  // 맨 처음일때
			newNode = new MyListNode(data);
			head = newNode; // 헤드에다가 노드를 넣음
		}
		else{ // 맨처음이 아닐때
			newNode = new MyListNode(data);
			MyListNode temp = head;
			while(temp.next != null)  // 널을 가르키는(즉 맨마지막인)노드를 찾음 
				temp = temp.next;
			temp.next = newNode;
		}
		count++;
		return newNode;
	}
	
    // 중간에 노드를 추가하는경우
	public MyListNode insertElement(int position, String data )
	{
		int i;
		MyListNode tempNode = head;
		MyListNode newNode = new MyListNode(data);
		
		if(position < 0 || position > count ){
			System.out.println("추가 할 위치 오류 입니다. 현재 리스트의 개수는 " + count +"개 입니다.");
			return null;
		}
		
		if(position == 0){  //맨 앞으로 들어가는 경우(헤드로 들어가는)
			newNode.next = head; // 원래 헤드였던것을 새로운 노드의 다음으로 한칸미루고
			head = newNode; // 원래 헤드자리에 새로운노드를 추가
		}
		else{
			MyListNode preNode = null;	
			for(i=0; i<position; i++){
				preNode = tempNode;
				tempNode = tempNode.next;
				
			}
			newNode.next = preNode.next; // 기존의노드를 새로운노드뒤로 미루고
			preNode.next = newNode; // 기존의노드자리에 새로운노드 삽입
		}
		count++;
		return newNode;
	}
	
	public MyListNode removeElement(int position)
	{
		int i;
		MyListNode tempNode = head;
		
		if(position >= count ){
			System.out.println("삭제 할 위치 오류입니다. 현재 리스트의 개수는 " + count +"개 입니다.");
			return null;
		}
		
		if(position == 0){  //맨 앞을 삭제하는걍우
			head = tempNode.next; // 그냥 헤드자리에 그다음노드를 넣으면 됨
		}
		else{
			MyListNode preNode = null;	
			for(i=0; i<position; i++){
				preNode = tempNode; // 목표노드를 바로 앞노드로 바꾸고
				tempNode = tempNode.next; // 바꾼노드가 목표노드 다음노드를 가르키도록 바꿈
			}
			preNode.next = tempNode.next;
		}
		count--;
		System.out.println(position + "번째 항목 삭제되었습니다.");
		
		return tempNode;
	}
```
