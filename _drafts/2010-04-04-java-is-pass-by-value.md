---
title: "Java is pass-by-value"
date: 2010-04-04 08:26:28
tags: [java]
---

# Java is pass-by-value
Java는 오직 pass-by-value(_값을 통한 전달_)을 한다. 이전부터 오해의 소지가 있었던 부분이라 명확하게 정리해 보고자 한다.

## Variable Types
Java는 원시형(_primitive type_) 변수와 객체형(_object type_) 인스턴스 변수의 선언에 따라 할당되는 값이 달라지게 된다. 원시형 선언의 경우 해당하는 값이 직접 변수에 할당되지만, 객체형 선언의 경우 객체가 생성된 메모리의 참조 주소가 할당되게 된다.

```
// primitive type: 상수 20 할당. (4 bytes)
int a = 20;

// reference type: 객체가 생성된 메모리의 참조값 할당. (4 bytes)
Object b = new Object();
```
예 1. 기본형과 참조형 변수

객체형 변수의 경우 참조값이 전달되게 되므로 callee에서 참조 인자를 통한 내부 접근 및 변경이 가능하다. 이 부분이 Java는 '값을 통한 전달'과 '참조를 통한 전달'로 구분되어 지원하는 것 마냥 보이게 된 것이 아닌가 싶다. 실제로 caller에서 변수 선언 시점에 변수의 형식(_원시형 또는 참조형_)은 이미 결정되고, 메서드의 호출에 넘겨 줄 인자는 전부 '값을 통한 전달'로 이루어지게 된다. 다시 말해, 원시형 변수의 값이든 객체형 변수의 참조-주소값이든, Java는 오직 변수에 할당된 값의 복사를 통해 호출할 인자로 전달한다.

## Pass-by-value Test
다음은 단순히 원시형과 참조형 변수를 인자로 취하는 메서드에서 인자를 통한 가공과 재-할당이 발생하는 경우, 원-호출자(caller)의 원본 값과 상태가 어떻게 달라지는지 확인할 수 있는 간략한 코드이다.

```
/**
 * Dummy Class
 */
class Dummy {
	private String value;

	public Dummy() {
		value = "default";
	}

	public void setValue(String value) {
		this.value = value;
	}

	@Override
	public String toString() {
		return "[" + value + "] " + super.toString();
	}
}

/**
 * Test
 */
public class Test {
	public void foo(int x) {
		x = 6;

		System.out.println("2: " + x);
	}

	public void foo(Dummy dummy) {
		dummy.setValue("GG");
		System.out.println("5: " + dummy);

		dummy = new Dummy();
		System.out.println("6: " + dummy);
	}

	public static void main(String[] args) {
		Test test = new Test();

		// primitive type test
		int x = 10;
		System.out.println("1: " + x);

		test.foo(x);
		System.out.println("3: " + x);

		// reference type test
		Dummy dummy = new Dummy();
		dummy.setValue("abcd");
		System.out.println("4: " + dummy);

		test.foo(dummy);
		System.out.println("7: " + dummy);
	}
}
```
예 2. Pass-by-value test code

`Dummy` 인스턴스의 참조 변수를 통해, 내부 값을 변경하는 부분과 참조 변수를 재-할당하는 부분을 참고하기 바란다.

```
1: 10
2: 6
3: 10
4: [abcd] Dummy@c2e1f26
5: [GG] Dummy@c2e1f26
6: [default] Dummy@dcf3e99
7: [GG] Dummy@c2e1f26
```
예 3. 결과 로그

-6-번째 로그 출력 부분을 보면, 새로운 객체를 생성하여 대입한 참조 변수는 caller에 전혀 영향을 끼치지 않았다. 이로써 `foo(Dummy)` 메서드의 인자 `dummy`는 '값을 통한 전달'을 통해 메서드 내부로 참조값이 복사되었음을 확인할 수 있다.
