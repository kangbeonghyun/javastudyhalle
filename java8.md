# 이것이 자바다 13~

# 13 제네릭

## 13.1 왜 제네릭을 사용해야 하는가

* 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거할 수 있게 되었다.(강한 타입 체크)

* 클래스, 인터페이스, 메소드를 정의할 때 타입을 파라미터로 사용할 수 있도록 한다.

  * public class A<T>{...}
  * public interface A<T>{...}

* 타입 변환을 제거한다

  ```java
  //비 제네릭
  List list=new ArrayList();
  list.add("hello");
  String str=(String)list.get(0);//타입 변환 해야하고 이는 성능 저하를 일으킨다
  
  //제네릭
  List<String> list=new ArrayList<String>();
  list.add("hello");
  String str=list.get(0);//타입 변환을 하지 않는다.
  ```

## 13.2 제네릭 타입

* 다양한 객체를 저장하고 싶어 Object 클래스를 활용했을 때 타입 변환이 일어난다.

  ```java
  public class Box{
    private Object object;
    public void set(Object object){this.object=object;}
    public Object get(){return object;}
  }
  
  Box box=new Box();
  box.set("hello");//자동 타입 변환
  String str=(String)box.get();//강제 타입 변환
  ```

* 제네릭은 실제 클래스가 사용될 때 구체적인 타입을 지정함으로써 타입 변환을 최소화 시킨다.

```java
//타입 변환이 일어나지 않는다->성능 저하를 줄임
public class Box<T>{
  private T t;
  public void set(T t){this.t=t;}
  public T get(return t;)
}

Box<String> box=new Box<String>();//이렇게 선언하면 위의 T가 모두 String으로 변하는 느낌.
box.set("hello");
String str=box.get();

```

## 13.3 멀티 타입 파라미터

* 콤마로 구분

  ```java
  public class Product<T,M>{
    private T kind;
    private M model;
    
    public T getKind(){return this.kind;}
    public M getKind(){return this.model;}
    
    public void setKind(T kind){this.kind=kind;}
    public void setModel(M model){this.model=model;}
  }
  
  //main
  Product<Tv,String> pT=new Product<Tv,String>();//뒷 부분 <>이렇게 표시할 수 있음.
  pT.setKind(new Tv());
  pT.setModel("스마트Tv");
  Tv tv=pT.getKind();
  String tvModel=pT.getModel();
  ```

## 13.4 제네릭 메소드

* 매개 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드, 리턴 타입 앞에 <>기호 추가 및 리턴과 매개를 타입 파라미터로.

* public<T> Box<T> boxing(T t){...}

* 제네릭 메소드 호출방법

  1. 타입 파라미터의 구체적인 타입을 명시적으로 지정

  2. 컴파일러가 매개값의 타입을 보고 구체적인 타입을 추정하도록.

     ```java
     Box<Integer> box=<Integer>boxing(100);//1
     Box<Integer> box=boxing(100);//2
     ```

* 메소드의 파라미터로 제네릭 타입이 오는 경우

  ```java
  //utils 클래스
  public static <K,V> boolean compare(Pair<K,V> p1,Pair<K,V> p2){
          boolean keyCompare=p1.getKey().equals(p2.getKey());
          boolean valueCompare=p1.getValue().equals(p2.getValue());
          return keyCompare&&valueCompare;
      }
  //public class Pair<K,V>{...}
  
  //메인 
  Pair<Integer,String> p1=new Pair<>(1,"사과");
  Pair<Integer,String> p2=new Pair<>(1,"사과");
  boolean result=Utils.compare(p1,p2);
  ```

## 13.5 제한된 타입 파라미터

* 예를 들어 숫자 연산을 하는 제네릭 메소드는 매개값으로 Number 또는 하위 클래스 타입의 인스턴스만 가져야 한다.

* 상위 타입은 클래스 뿐만 아니라 인터페이스도 가능하다.(인터페이스라도 implements를 쓰지 않는다)

  ```java
  //Utils 클래스
  public <T extends Number> int compare(T t1,T t2){
    ...
    return Double.compare(v1,v2);  
  }
  
  //메인
  int result=Utils.compare(10,20);//number로 제한했기 때문에 Utils.compare("a","b"); 안된다
  ```

## 13.6 와일드 카드 타입

* 제네릭 타입을 매개값이나 리턴 값으로 사용할 때 3가지 형태로 와일드 카드를 사용할 수 있다

  1. 제네릭타입<?>: 제한없음, 모든 클래스나 인터페이스 타입이 올 수 있다
  2. 제네릭타입<? extends 상위타입>: ?에는 젤 높아봐야 상위타입까지 올 수 있다(<상위타입).(상위 클래스 제한)
  3. 제네릭타입<? super 하위타입>: ?에는 젤 낮아봐야 하위타입까지 올 수 있다(>하위타입).(하위 클래스 제한)

* cf) 타입 파라미터로 배열 생성 방법

  ```java
  private T[] students;
  students=(T[])(new Object[30]);//new T[n] 형태 안된다.
  ```

## 13.7 제네릭 타입의 상속과 구현

* 제네렉 타입도 상속을 가질 수 있고 자식 제네릭 타입은 더 많은 타입 파라이터를 가질 수 있다.

  ```java
  public class ChildProduct<T,M,C> extends Product<T,M>{..}
  ```

* 제네릭 인터페이스를 구현한 클래스도 제네릭 타입이 된다.

  ```java
  public interface Storage<T>{
    public void add(T item,int index);
    public T get(int index);
  }
  
  public class StorageImpl<T> implements Storage<T>{
    ...
  }
  
  ```

  
















