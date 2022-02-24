# 무엇이든 담을 수 있는 제네릭(Generic) 프로그래밍
## 제네릭 자료형 정의
- 클래스에서 사용하는 변수의 자료형이 여러개지만 그 기능(메서드)은 동일한 경우 클래스의 자료형을 특정하지 않고 추후 해당 클래스를 사용할 때 지정 할 수 있도록 선언
- 실제 사용되는 자료형의 변환은 컴파일러에 의해 검증되므로 안정적인 프로그래밍 방식
- 컬렉션 프레임워크에서 많이 사용되고 있음

## 제네릭 타입을 사용하지 않는 경우
- 하나의 프린터에서 재료에따라 각각 다른 class를 생성
```java
// 재료가 Powder인 경우
public class ThreeDPrinter1{
	private Powder material;
	
	public void setMaterial(Powder material) {
		this.material = material;
	}
	
	public Powder getMaterial() {
		return material;
	}
}

// 재료가 Plastic인 경우
public class ThreeDPrinter2{
	private Plastic material;
	
	public void setMaterial(Plastic material) {
		this.material = material;
	}
	
	public Plastic getMaterial() {
		return material;
	}
}
```
```java
// 여러 타입을 대체하기 위해 Object를 사용할 수 있음
public class ThreeDPrinter{

	private Object material;
	
	public void setMaterial(Object material) {
		this.material = material;
	}
	
	public Object getMaterial() {
		return material;
	}
}
// 하지만 Object를 사용하는 경우는 형 변환을 하여야 함
// 형변환을 하지않으면, 넣엇던 재료를 다시 꺼내는경우 오브젝트를 꺼내는과정에서 오류가발생
ThreeDPrinter printer = new ThreeDPrinter();

Powder powder = new Powder();
printer.setMaterial(powder);

Powder p = (Powder)printer.getMaterial(); // 그래서 object를 Powder로 형변환해서 꺼내야함
```

<br>

## 제네릭 클래스 정의
- 자료형 매개변수 T(type parameter) : 이 클래스를 사용하는 시점에 실제 사용할 자료형을 지정, static 변수는 사용할 수 없음
- GenericPrinter : 제네릭 자료형(자료형 매개변수를 사용한것)
- E : element, K: key, V : value 등 여러 알파벳을 의미에 따라 사용 가능
```java
// GenericPrinter.java
public class GenericPrinter<T> { // <T> 로 내부에 제네럴타입을 쓰겠다고 명시
	private T material; // 실제로 쓸 자료형을 설정
	
	public void setMaterial(T material) {
		this.material = material;
	}
	
	public T getMaterial() {
		return material;
	}
	
	public String toString(){
		return material.toString();
	}
}
```

## 제네릭 클래스 사용
```java
// Powder.java
public class Powder {
	
	public String toString() {
		return "재료는 Powder 입니다";
	}
}
// Plastic.java
public class Plastic {

	public String toString() {
		return "재료는 Plastic 입니다";
	}
}
// GenericPrinter.java
public class GenericPrinter<T> {
	private T material;   //T 자료형으로 선언한 변수
	
	public void setMaterial(T material) {
		this.material = material;
	}
	
	public T getMaterial() {   //T 자료형을 반환하는 제네릭 메서드
		return material;
	}
	
	public String toString(){
		return material.toString();
	}
}
```

```java
// GenericPrinterTest.java
public class GeneriPrinterTest {

	public static void main(String[] args) {

		GenericPrinter<Powder> powderPrinter = new GenericPrinter<Powder>();
		powderPrinter.setMaterial(new Powder());
		System.out.println(powderPrinter);
		
		GenericPrinter<Plastic> plasticPrinter = new GenericPrinter<Plastic>();
		plasticPrinter.setMaterial(new Plastic());
		System.out.println(plasticPrinter);	
	}
}
```

## 다이아몬드 연산자 <>
- <T>에서 <>를 다이아몬드 연산자라 함
- ArrayList<String> list = new ArrayList<>();  //다이아몬든 연산자 내부에서 자료형은 생략가능 함
- 제네릭에서 자료형 추론(자바 10부터)
```java
ArrayList<String> list = new ArrayList<String>()  => var list = new ArrayList<String>();
```