# static 변수
- 여러 인스턴스에서 공통으로 사용하는 변수
- 공유하는 변수의 기준값이 필요한 경우 사용된다
- 데이터영역(상수,static영역) 메모리에 저장되어 모든 인스턴스에서 접근이 가능
  - 이전에 공부한 스택,힙 영역은 데이터영역이 아닌 코드영역에 해당한다

## static 변수 선언과 사용하기
- static int serialNum;
- 인스턴스가 생성될 때 만들어지는 변수가 아닌, 처음 프로그램이 메모리에 로딩될 때 메모리를 할당
- **클래스 변수, 정적변수**라고도 함(vs. 인스턴스 변수)
- 인스턴스 생성과 상관 없이 사용 가능하므로 클래스 이름으로 직접 참조가능
  - Student.serialNum = 100;
```JAVA
public class Employee {

    // 기준되는 시리얼번호 1000을 스태틱변수로 선언
	public static int serialNum = 1000;
	
	private int employeeId;
	private String employeeName;
	private String department;
		
	public int getEmployeeId() {
		return employeeId;
	}
	public void setEmployeeId(int employeeId) {
		this.employeeId = employeeId;
	}
	public String getEmployeeName() {
		return employeeName;
	}
	public void setEmployeeName(String employeeName) {
		this.employeeName = employeeName;
	}
	public String getDepartment() {
		return department;
	}
	public void setDepartment(String department) {
		this.department = department;
	}	
}

// 테스트
public class EmployeeTest {

	public static void main(String[] args) {
		Employee employeeLee = new Employee();
		employeeLee.setEmployeeName("이순신");
		System.out.println(employeeLee.serialNum);
		
		Employee employeeKim = new Employee();
		employeeKim.setEmployeeName("김유신");
        // Kim 으로 접근하여 증가시킴
		employeeKim.serialNum++;
		
        // kim 에서 증가시켯지만 lee에도 적용된것을 확인할 수 있음
		System.out.println(employeeLee.serialNum); // 1001
		System.out.println(employeeKim.serialNum); // 1001
	}
}
```
- static 변수는 인스턴스에서 공통으로 사용하는 영역
- 힙: 동적메모리(필요할때 가져다쓰는) 스택: 지역변수, 데이터: 프로그램이 실행될떄 동시에 생성되는영역 // 정도로 기억해두자
![static](img/static.png)

