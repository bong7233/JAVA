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
<br>

## static 메서드(클래스 메서드)에서는 인스턴스 변수를 사용할 수 없다
- static 메서드는 인스턴스 생성과 무관하게 클래스 이름으로 호출 될 수 있음
- 인스턴스 생성 전에 호출 될 수 있으므로 static 메서드 내부에서는 인스턴스 변수를 사용할 수 없음
<br>

## 변수의 유효 범위와 메모리
- 변수의 유효 범위(scope)와 생성과 소멸(life cycle)은 각 변수의 종류마다 다름
- 지역변수, 멤버 변수, 클래스 변수는 유효범위와 life cycle, 사용하는 메모리도 다름
- static 변수는 프로그램이 메모리에 있는 동안 계속 그 영역을 차지하므로 너무 큰 메모리를 할당하는 것은 좋지 않음
- 클래스 내부의 여러 메서드에서 사용하는 변수는 멤버 변수로 선언하는 것이 좋음
- 멤버 변수가 너무 많으면 인스턴스 생성 시 쓸데없는 메모리가 할당됨
![변수](img/변수.png)

## 싱글톤 패턴 (singleton pattern)
- 프로그램에서 인스턴스가 단 한 개만 생성되어야 하는 경우 사용하는 디자인 패턴
- static 변수, 메서드를 활용하여 구현 할 수 있음
```JAVA
// 생성자는 private으로 선언
private Company() {}

// 클래스 내부에 유일한 private 인스턴스 생성
private static Company instance = new Company();

// 외부에서 유일한 인스턴스를 참조할 수 있는 public 메서드 제공
public static Company getInstance() {
		
	if( instance == null) {
		instance = new Company();
	}
	return instance;
		
}

// CompanyTest
public class CompanyTest {

	public static void main(String[] args) {
		Company company1 = Company.getInstance();
		//               = new Comapney.~ 가 아니여도 생성된다
		
		Company company2 = Company.getInstance();
		
		System.out.println(company1);
		System.out.println(company2); // 동일한 위치값이 나옴
		
		// Calendar calendar = Calendar.getInstance();
		// new 없이 싱글톤패턴으로 하나만 바로 생성가능
	}
}
```

