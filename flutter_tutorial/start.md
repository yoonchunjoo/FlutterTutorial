### Flutter 기본구조 훑어보기 
> Flutter 생성시 기본앱 구조로 기초상식 정리

- [dartpad로 실행하기](https://dartpad.dev/cd85fc03b922ed6a4437846d9c9c052c)

![](images/start.jpg)

1. 프로젝트 생성하면 lib/main.dart에 main 함수가 존재함. runApp() 함수를 실행하여 시작됨.

    - runApp()은 파라메터로 Widget을 받는다. 
    - Widget은 Flutter를 구성하는 모든 것들의 기본단위 클래스
    - Widget은 Widget을 포함시켜 계층단위(parent, child)를 만든다. => 화면 안의 화면으로 구성
    - 화면이 계층구조이다보니 Widget들의 오른쪽 들여쓰기가 많은 편임(주의필요).
    
    ~~~dart
    
    void main() {
      runApp(MyApp()); 
    }

    ~~~
    
    
2. 위에서 MyApp은 Widget에서 하위계층인 StatelessWidget을 상속받았다. 

    - StatelessWidget 위젯의 Build 메소드에서 위젯을 생성 및 반환
    - 간단한 한 개의 위젯에서 복잡한 테마를 포함할 수 있는 Container 위젯까지 다양하게 반환가능

    ~~~dart
    class MyApp extends StatelessWidget {
      // This widget is the root of your application.
      @override
      Widget build(BuildContext context) {
        return ... // 위젯생성
      }
    }
    ~~~
    
    - **StatelessWidget**은 상태를 관리하지 않는 Widget이다. 자신의 위젯 안에서 상태(데이터 변경)를 변경할 수 없다.
    - 반대로 **StatefulWidgetd**은 상태가 변하면(데이터 변경) Widget들의 화면을 갱신할 때 사용한다.  

3. build 메소드에서 MaterialApp으로 반환 

    - MaterialApp은 Android Theme의 한 종류이다. 
      - title: 앱 타이틀바 영역문구
      - theme: 머터리얼 테마설정
      - home : 화면을 그리는 영역(위젯)
      

    ~~~dart
    ...

    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.red, // 캡션 색상을 변경
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      //home: Text("AAA"), // 이렇게 하면 테마가 적용되지않은 AAA가 표시됨
      home: Scaffold(body: Text("AAA")), // 머터리얼 테마가 적용된 AAA
      // home: MyHomePage(title: 'Flutter Demo Home Page'), // 화면갱신가능한 StatefulWidget 위젯

    );
    ~~~
    
    - Scafold 안에 위젯을 만들어 넘겨주어야 머터리얼 테마로 화면이 구성된다. 
    - Scafold 안의 중요한 파라메터들은 다음과 같다. 
      - appBar : 타이틀바 관련 위젯
      - body   : 타이틀바 외의 모든 위젯


4. 위는 StatelessWidget 위젯이다. 그러므로 화면갱신이 안된다. 만약 화면 갱신하고 싶으면 home에 StatefulWidget 위젯을 넘긴다. 

    ~~~dart
    class MyHomePage extends StatefulWidget {
      MyHomePage({Key key, this.title}) : super(key: key);
      
      final String title;
      @override
      _MyHomePageState createState() => _MyHomePageState();
    }
    ~~~
    
    - StatefulWidget의 createState() 메소드를 오버라이드한다. 
      - State<T> 객체를 넘긴다.  
      - State 객체(_MyHomePageState)는 build() 메소드에서 화면을 만든다.  
      - setState( (){ /*값을 변경*/ })을 호출하면 화면이 갱신된다. 

    ~~~dart
    class _MyHomePageState extends State<MyHomePage> {
      // 관리하는 변수값
      int _counter = 0;

      // 값을 변경 및 화면갱신요청
      void _incrementCounter() {
        setState(() {
          _counter++;
        });
      }

      ... 

      // 화면갱신 
      @override
      Widget build(BuildContext context) {
        ...
      }
    }  
    ~~~  

5. flutter에서 State lifecycle은 다음과 같다. 
   - initState() : **최초 1회 실행**. super.initState()를 반드시 실행.
   - didChangeDependencies() : initState() 호출 후, 실행됨. Status와 연관된 데이터(InheritedWidget)가 변경되면 호출됨.
   - build() : **화면구현**
   - didUpdateWidget() : 화면갱신시 호출. 과거 위젯과의 변화를 비교가능.
   - deactivate()
   - dispose() : 메모리에서 삭제됨


