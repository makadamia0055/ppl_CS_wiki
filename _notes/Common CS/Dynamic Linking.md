---
tags:
  - linking
  - Dynamic
aliases:
  - 런타임 링킹
  - Runtime Linking
  - Dynamic Linking
  - 동적 링킹
---
# Dynamic Linking(=Runtime Linking)
- 동적 링킹 혹은 런타임 링킹이란 [[Linking]]의 한 종류로, "실행 가능한 목적 파일을 만들 때 프로그램에서 사용하는 모든 [[Library]] 모듈을 복사하지 않고 해당 모듈의 주소만을 가지고 있다가, 런타임에 실행 파일과 라이브러리가 위치될 때 해당 [[모듈]]의 주소로 가서 필요한 것을 들고 오는 방식"
	- [[Runtime|런타임]]에 [[OS]]에 의해서 이루어진다.
- 동적 링킹이라는 명칭은 [[Linking|링킹]]이 동적으로 이루어진다는 것을 강조한 관점이고, 런타임 링킹이라는 명칭은 링킹이 [[Runtime|런타임]]에 일어난다는 것을 강조한 관점이다.

## 동적 링킹 방식의 목적
- 동적 링킹 방식을 이용하면, 5 개의 프로그램에서 A라는 외부 함수를 이용한다고 해도, A라는 함수의 정보는 하나만 있으면 된다.
	- 각각의 실행 가능한 목적 파일에서는 A를 복사해서 A 그 자체를 가지고 있는 것이 아니라(=[[Static Linking|정적 링킹]]의 방식), A의 주소만 가지고 있기 때문이다. 
	- 때문에 많이 쓰이는 라이브러리 함수를 메모리에 한 번만 올리고, 프로그램이 라이브러리 함수를 호출할 때 메모리에 있는 함수 주소로 점프해 실행한 뒤 다시 돌아오도록 하는 방법이다.
		- 이러한 동적 링크 라이브러리를 [[Windows]]에서는 [[DLL(Dynamic Link Library)]]라고 하고, [[Unix]]나 [[Linux]]에서는 [[Shared Library]]로 부른다. 

## 동적 링킹의 장점과 단점
- 동적 링킹은 실행 파일에 라이브러리 코드를 가지고 있는 것이 아니라, 주소만 가지고, 런타임에 실행 파일과 라이브러리가 메모리에 위치할 때 해당 모듈의 주소로 가서 필요한 것을 들고 온다.
	- 장점 
		- 외부 라이브러리의 코드가 아니라 주소를 가지고 있기 때문에, ([[Static Linking|정적 링킹]]에 비해) 실행가능한 목적 파일의 크기가 작다.
			- 즉, 메모리와 디스크 공간을 아낄 수 있다.
		- 또, A라는 함수에 변화가 생겨도 그 변화를 적용하기 위해 다시 컴파일하고 다시 링킹할 필요가 없다.
			- A의 코드가 아니라 주소를 담았기에, 런타임시 링킹하면 변화한 A를 가져올 수 있다.
		- [[Static Linking|정적 링킹]]을 사용한 프로그램은 실행을 위해 메모리에 로드되는 데 일정한 시간이 걸리지만, 동적 링킹 방식은, 동적 라이브러리가 메모리에 이미 존재하는 경우, 로드되는 시간을 단축시킬 수 있다. 
	- 단점
		- 동적 링킹 방식은 런타임 시 매번 주소를 따라가야 하는 [[오버헤드]]가 존재하여 정적 링킹 방식보다 느리다. 
			- 시작 지연(Startup Overhead) : 동적 링킹은 실행 시에 필요한 라이브러리를 찾아 로드하는 과정이 필요하여, 최초 실행 시에 시작 지연이 발생.
			- 런타임 오버헤드 :  동적 링킹은 프로그램이 실행 중에 외부 라이브러리나 모듈을 추가로 불러오는데, 이는 런타임 오버헤드를 유발.
			- [[Dependency|의존성]] 관리/compatibility issues :  동적 링킹을 사용하는 경우 실행 파일은 필요한 라이브러리의 위치를 알고 있는데, 실행 파일과 라이브러리의 위치가 변경되는 경우, 프로그램이 제대로 동작하지 않을 수 있다.
				- 해당 프로그램의 실행 가능 목적 파일에는 함수 A의 주소가 있어서 A가 존재하는 것처럼 움직이지만, 실제 A가 시스템에 존재하지 않을 수 있기 때문이다.
			- 보안 고려사항 : 동적 링킹은 실행 중에 외부 코드를 동적으로 로드하므로, 보안 측면에서 조심해야 한다. 악성 코드의 삽입이나 코드 변조의 가능성이 높아질 수 있다. 

# [[Lazy loading]]과 [[Eager loading]]
- 동적 링킹 자체는 [[Runtime]]에 동적으로 [[Linking|링킹]]이 일어난다는 규정이다.
	- 때문에 라이브러리를 불러올 때, 프로그램 최초 실행시 불러올 것인지, 아니면 해당 라이브러리가 호출되기 직전에 불러올 것인지는 아래와 같은 동적 링킹의 세부 범주에 해당한다.
		- [[Eager loading]] : 프로그램 최초 실행 시 동적 링킹이 일어나 함수와 라이브러리에 대한 로딩과 링킹이 이루어진다.
		- [[Lazy loading]] : 함수나 라이브러리가 호출되기 직전(=처음 사용되는 시점)에 로딩 및 링킹이 이루어진다.




## 레퍼런스
- [정적 링킹과 동적 링킹](https://live-everyday.tistory.com/69?category=835430)
- GPT 3.5