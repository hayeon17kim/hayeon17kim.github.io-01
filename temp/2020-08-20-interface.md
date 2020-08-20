# Interface

## 인터페이스의 이해
- 클래스를 사용하는 쪽(User)과 클래스를 제공하는 쪽(Provider)이 있다.
- 메서드를 사용(호출)하는 쪽(User)에서는 사용하려는 메서드(Provider)의 선언부만 알면 된다. (내용은 몰라도 된다.)

직접적인 관계 (Interface X)
```java
class A {
    public void methodA(B b) {
        b.method();
    }
}

class B {
    public void methodB(){
        System.out.println("methodB()")
    }
}

class InterfaceTest { 
    public static void main(String[] args){
        A a = new A();
        a.methodA(new B());
    }
}
```

2) 매개변수를 통해 인터페이스 I를 구현한 클래스의 인스턴스를 동적으로 제공받는다.

```java
class A {
    void autoPlay(I i) {
        i.play();
    }
}

interface I {
    public abstract void play();
}

class B implements I {
    public void play() {
        System.out.println("play in B class");
    }
}

class C implements I {
    public void play() {
        System.out.println("play in C class");
    }
}

class InterfaceTest2 {
    public static void main(String[] args) {
        A a = new A();
        a.autoPlay(new B());
        a.autoPlay(new C());
    }
}
```



3) 제3의 클래스의 메서드를 통해서 인터페이스 I를 구현한 클래스의 인스턴스를 얻어온다.

```java
class InterfaceTest3 {
    public static void main(String[] args){
        A a = new A();
        a.methodA();
    }
}

class A {
    void methodA () {
        I i = InstanceManager.getInstance();
        i.methodB();
        System.out.println(i.toString());
    }
}

Interface I {
    public abstract void methodB();
}

class B implements I {
    public void methodB() {
        System.out.println("methodB in B class");
    }
    public String toString() { return "classB"; }
}

class InterfaceManager {
    public static I getInstance() {
        return new B();
    }
}
```



