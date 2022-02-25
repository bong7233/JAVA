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
<br>


# <T extends 클래스> 사용
## 상위 클래스의 필요성
- T 자료형의 범위를 제한 할 수 있음
- 상위 클래스에서 선언하거나 정의하는 메서드를 활용할 수 있음
- 상속을 받지 않는 경우 T는 Object로 변환되어 Object 클래스가 기본으로 제공하는 메서드만 사용가능
## T extends 를 사용한 프로그래밍
- GenericPrinter<T> 에 material 변수의 자료형을 상속받아 구현
- T에 무작위 클래스가 들어갈 수 없게 Material 클래스를 상속받은 클래스로 한정  
![material](./img/material.PNG)
```java
// Material.java
// 재료들이 공통으로 사용할 메서드를 정의
public abstract class Material {
	
	public abstract void doPrinting();
}
```
```java
// Powder.java
public class Powder extends Material{
		
	public void doPrinting() {
		System.out.println("Powder 재료로 출력합니다");
	}
	
	public String toString() {
		return "재료는 Powder 입니다";
	}
}

// Plastic.java
public class Plastic extends Material{

	public void doPrinting() {
		System.out.println("Plastic 재료로 출력합니다");
	}
	
	public String toString() {
		return "재료는 Plastic 입니다";
	}
}
```
```java
// GenericPrinter.java
public class GenericPrinter<T extends Material> {
	private T material;
	
	public void setMaterial(T material) {
		this.material = material;
	}
	
	public T getMaterial() {
		return material;
	}
	
	public String toString(){
		return material.toString();
	}
	
	public void printing() {
		material.doPrinting();
	}
}
```

```java
// GenericPrinterTest.java
public class GenericPrinterTest {

	public static void main(String[] args) {

		GenericPrinter<Powder> powderPrinter = new GenericPrinter<Powder>();
		powderPrinter.setMaterial(new Powder());
		Powder powder = powderPrinter.getMaterial(); // 형변환 하지 않음
		System.out.println(powderPrinter);
		
		GenericPrinter<Plastic> plasticPrinter = new GenericPrinter<Plastic>();
		plasticPrinter.setMaterial(new Plastic());
		Plastic plastic = plasticPrinter.getMaterial(); // 형변환 하지 않음
		System.out.println(plasticPrinter);
		
        // 이제는 타입에 제한을 두었으므로 Water는 사용불가능
	    //GenericPrinter<Water> printer = new GenericPrinter<Water>();	
	}
}
```

## 제네릭 메서드
- 자료형 매개변수를 메서드의 매개변수나 반환 값으로 가지는 메서드는
- 자료형 매개 변수가 하나 이상인 경우도 있음( T, V, E 등등...)
- 제네릭 클래스가 아니어도 내부에 제네릭 메서드는 구현하여 사용 할 수 있음
  - public <자료형 매개 변수> 반환형 메서드이름(자료형 매개변수.....) { }

## 제네릭 메서드의 활용
- 두 점(top, bottom)을 기준으로 사각형을 만들 때 사각형의 너비를 구하는 메서드
  - 두 점은 정수인 경우도 있고, 실수인 경우도 있으므로 제네릭 타입을 사용하여 구현한다
```java
// Point.java
public class Point<T, V> {
	
	// 이 두개의점이 타입매개변수
	T x;
	V y;
	
	Point(T x, V y){
		this.x = x;
		this.y = y;
	}
	
	public  T getX() {
			return x;
	}

	public V getY() {
		return y;
    }
}

//GenericMethod.java
public class GenericMethod {
	// main에서 바로 쓰려고 static으로 만듦
	public static <T, V> double makeRectangle(Point<T, V> p1, Point<T, V> p2) {
		double left = ((Number)p1.getX()).doubleValue();
		double right =((Number)p2.getX()).doubleValue();
		double top = ((Number)p1.getY()).doubleValue();
		double bottom = ((Number)p2.getY()).doubleValue();
		
		double width = right - left;
		double height = bottom - top;
		
		return width * height;
	}
	
	public static void main(String[] args) {
		
		Point<Integer, Double> p1 = new Point<Integer, Double>(0, 0.0);
		                          //Point<Integer,Double> 안써도됨
		Point<Integer, Double> p2 = new Point<>(10, 10.0);
		
		// static 메서드이므로 genericmethod. 으로 사용가능
		double rect = GenericMethod.<Integer, Double>makeRectangle(p1, p2);
		System.out.println("두 점으로 만들어진 사각형의 넓이는 " + rect + "입니다.");
	}
}
```