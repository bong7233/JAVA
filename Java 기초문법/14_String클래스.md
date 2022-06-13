# String, StringBuilder, StringBuffer 클래스, text block
## String 클래스
- 힙 메모리에 인스턴스로 생성되는 경우와 상수 풀(constant pool)에 있는 주소를 참조하는 두 가지 선언방법
```JAVA
String str1 = new String("abc"); // 힙에 메모리가 생성
String str2 = "abc"; // 상수풀에서 주소만 참조
```

- 힙 메모리는 생성될때마다 다른 주소 값을 가지지만, 상수 풀의 문자열은 모두 같은 주소 값을 가짐
```JAVA
public class StringTest {

	public static void main(String[] args) {
		String str1 = new String("abc"); 
		String str2 = new String("abc");
		
		System.out.println(str1 == str2); // false 각각 다른힙에 잡힘
		
		String str3 = "abc"; 
		String str4 = "abc";
		
		System.out.println(str3 == str4); // True 상수풀에있는 같은 abc를 가르킴
	}
}
```
- String을 연결하면 기존의 String에 연결되는 것이 아닌 연결된 새로운 string이 생성됨 (메모리 낭비가 발생)
  - 한번 생성된 String은 불변(immutable)이므로
```JAVA
public class StringTest2 {

	public static void main(String[] args) {
		String java = new String("java"); // 힙1에 생성
		String android = new String("android"); // 힙2에 생성
		System.out.println(System.identityHashCode(java));
		
		java = java.concat(android); 
        // 연결된 java는 힙1에 연장되는것이 아니라 힙3에 "java android"로 새로운 string 생성 -> 메모리 낭비
	}
}
```

## StringBuilder, StringBuffer 활용
- 위에서 본 메모리낭비를 피하기위한 방법
- 내부적으로 가변적인 char[]를 멤버 변수로 가짐 
- 문자열을 여러번 연결하거나 변경할 때 사용하면 유용
- 새로운 인스턴스를 생성하지 않고 char[] 를 변경함
- StringBuffer는 멀티 쓰레드 프로그래밍에서 동기화(synchronization)을 보장(두개이상의 쓰레드가 접근할때 쓰레드의 순서를 정해준다)
- 단일 쓰레드 프로그램에서는 StringBuilder 사용을 권장
```JAVA
// toString() 메서드로 String반환
public class StringBuilderTest {

	public static void main(String[] args) {
		String java = new String("java");
		String android = new String("android");
		
        // StringBuilder의 매개변수로 string 
		StringBuilder buffer = new StringBuilder(java);
		System.out.println(System.identityHashCode(buffer)); // 메모리값 2222222
		
        buffer.append("android"); // 뒤에 붙임
		System.out.println(System.identityHashCode(buffer)); // 동일한 222222
		
		java = buffer.toString();
        System.out.println(java); // javaandroid
	}
}
```

## text block 사용하기 (java 13)
- 문자열을 """ """ 사이에 이어서 생성
- html, json 문자열을 만드는데 사용
```JAVA
public class StringTextBlock {

	public static void main(String[] args) {
		
		String strBlock = """
				This is 
				textblock
				""";
		System.out.println(strBlock);
		
		System.out.println(getBlockOfHtml());
		
	}
	
	public static String getBlockOfHtml() {
		    return """
		            <html>
		                <body>
		                    <span>example text</span>
		                </body>
		            </html>""";
	}
}
```
