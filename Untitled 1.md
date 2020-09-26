> eomcs.java.io.ex04

# 데이터 출력

## int 입출력

### 1바이트만 입출력

```java
FileOutputStream out = new FileOutputStream("temp/test3.data");
int money = 1_3456_7890; // = 0x080557d2
out.write(money); //항상 출력할 때는 맨 끝 1바이트만 출력한다.
```

- 위의 코드를 실행하여 출력한 데이터를 InputStream.read()로 읽는다.

- read()는 1바이트를 읽어 int 값으로 만든 후 리턴한다.

```java
FileInputStream in = new FileInputStream("temp/test3.data");
int value = in.read(); // 실제 리턴한 값은 0xD2이다.
in.close();
System.out.printf("%1$x(%1$d)\n", value); // d2(210)
```



### 모든 바이트 입출력

```java
FileOutputStream out = new FileOutputStream("temp/test3.data");
int money = 1_3456_7890; // = 0x080557d2

out.write(money >> 24); // 00000008|0557d2
out.write(money >> 16); // 00000805|57d2
out.write(money >> 8);  // 00080557|d2
out.write(money);       // 080557d2
```

- 위의 코드를 실행하여 출력한 데이터를 read()로 읽는다.
- 파일에서 4바이트를 읽어 4바이트 int 변수에 저장한다.
- 읽은 바이트를 비트 이동 연산자로 값을 이동시킨 후 변수에 저장해야 한다.

```java
int value = in.read() << 24;   // 00000008 => 08000000
value += (in.read() << 16);    // 00000005 => 00050000 +
value += (in.read() << 8);     // 00000057 => 00005700 +
value += in.read();            // 000000d2 => 000000d2 +
											       //             080557d2
System.out.printf("%x\n", value); //80557d2
```



## long 입출력

### 모든 바이트 입출력

```java
FileOutputStream out = new FileOutputStream("temp/test3.data");
long money = 400_0000_0000_0000L; // = 0x00016bcc41e90000

// long 메모리의 모든 바이트를 출력하려면,
// 각 바이트를 맨 끝으로 이동한 후 write()로 출력한다.
// 왜? write()는 항상 변수의 마지막 1바이트만 출력하기 때문이다.
out.write((int)(money >> 56)); // 0000000000000000|016bcc41e90000
out.write((int)(money >> 48)); // 0000000000000001|6bcc41e90000
out.write((int)(money >> 40)); // 000000000000016b|cc41e90000
out.write((int)(money >> 32)); // 0000000000016bcc|41e90000
out.write((int)(money >> 24)); // 00000000016bcc41|e90000
out.write((int)(money >> 16)); // 000000016bcc41e9|0000
out.write((int)(money >> 8));  // 0000016bcc41e900|00
out.write((int)money);         // 00016bcc41e90000|
```

- 위 코드를 실행하여 출력한 데이터를 read()로 읽는다.
- 파일에서 8바이트를 읽어서 8바이트 long 변수에 저장한다.

```java
FileInputStream in = new FileInputStream("temp/test3.data");
// => 파일에 들어 있는 값 예: 00016bcc41e90000
long value = (long)in.read() << 56;   
value += (long)in.read() << 48;
value += (long)in.read() << 40;
value += (long)in.read() << 32;
value += (long)in.read() << 24;
value += (long)in.read() << 16;
value += (long)in.read() << 8;     
value += (long)in.read();            

in.close();
System.out.printf("%x\n", value);
```



## String 입출력

`str.getBytes(문자코드표)`

- 문자열을 지정한 문자 코드표에 따라 인코딩하여 바이트 배열을 만든다.
- `str.getBytes("UTF-8")`:  UCS2 문자 => UTF-8 문자

```java
FileOutputStream out = new FileOutputStream("temp/test3.data");
String str = "AB가각간";
out.write(str.getBytes("UTF-8"));
```
- 위의 코드의 실행 결과로 만든 파일을 읽는다.
- 바이트 배열에 들어 있는 값을 사용하여 String 인스턴스를 만든다.
- `new String(바이트배열, 시작번호, 개수, 문자코드표)`

```java
FileInputStream in = new FileInputStream("temp/test3.data");
byte[] buf = new byte[100];
int count = in.read(buf);
String str = new String(buf, 0, count, "UTF-8");
System.out.printf("%s\n", str); //AB가각간
```



## 인스턴스 입출력

### FileInput/OutputStream 사용

인스턴스의 값을 출력하는 코드

```java
public static void main(String[] args) throws Exception {
  FileOutputStream out = new FileOutputStream("temp/test4.data");
  
  Member member = new Member();
  member.name = "AB가각간";
  member.age = 27;
  member.gender = true;
  
  // 인스턴스의 값을 출력하라!
  // 이름 출력
  byte[] bytes = member.name.getBytes("UTF-8");
  out.write(bytes.length);
  out.write(bytes);
  
  // 나이 출력
  out.write(member.age >> 24);
  out.write(member.age >> 16);
  out.write(member.age >> 8);
  out.write(member.age);
  
  // 성별 출력
  out.write(member.gender ? 1 : 0);
}
```

파일이 데이터를 읽어 인스턴스로 만들기

```java
public static void main(String[] args) throws Exception {
  FileInputStream in = new FileInputStream("temp/test4.data");
  
  Member member = new Member();
  
  // 이름(String) 읽기
  int size = in.read(); // 이름이 저장될 바이트 배열의 수
  byte[] buf = new byte[size];
  in.read(buf); // 이름 배열 개수 만큼 바이트를 읽어 배열에 저장
  member.name = new String(buf, "UTF-8");
  
  // 나이(int) 읽기
  member.age = in.read() << 24;
  member.age += in.read() << 16;
  member.age += in.read() << 8;
  member.age += in.read();
  
  // 성별(boolean) 읽기
  member.gender = (in.read() == 1);
}
```



FileInput/OutputStream 객체를 생성해서 메서드를 사용해도 읽거나 출력할 데이터의 타입에 따라 다른 방식으로 처리를 해야 한다. FileInput/OutputStream 클래스를 상속받고 처리할 데이터의 타입에 따라 메서드를 추가한 클래스를 만들어보자.

### DataFileInput/OutputStream 하위 클래스 구현

```java
public class DataFileOutputStream extends FileOutputStream {
  public DataOutputStream(String filename) throws Exception {
    super(filename);
  }
  
  public void writeUTF(String str) throws Exception {
    // 상속 받은 write() 메서드를 사용하여 문자열 출력
    byte[] bytes = str.getBytes("UTF-8");
    this.write(bytes.length);
    this.write(bytes);
  }
  
  public void writeInt(int value) throws Exception {
    // 상속 받은 write() 메서드를 사용하여 int 값 출력
    this.write(value >> 24);
    this.write(value >> 16);
    this.write(value >> 8);
    this.write(value);
  }
  
  public void writeLong(int value) throws Exception {
    // 상속 받은 write() 메서드를 사용하여 long 값 출력
    this.write((int)(value >> 56));
    this.write((int)(value >> 48));
    this.write((int)(value >> 40));
    this.write((int)(value >> 32));
    this.write((int)(value >> 24));
    this.write((int)(value >> 16));
    this.write((int)(value >> 8));
    this.write((int)value);
  }
  
  public void writeBoolean(boolean b) throws Exception {
    // 상속 받은 write() 메서드를 사용하여 boolean 값 출력
    this.write(b ? 1 : 0);
  }
}
```

DataFileOutputStream을 이용하여 객체(인스턴스의 값)를 출력해보자.

```java
public static void main(String[] args) throws Exception {
  DataFileOutputStream out = new DataFile
}
```





```java
public class DataFileInputStream extends FileInputStream {
  public DataFileInputStream(String filename) throws Exception {
    super(filename);
  }
  
  public String readUTF() {
    int size = this.read();
    byte[] bytes = new byte[size];
    this.read(bytes);
    return new String(bytes, "UTF-8");
  }
  
  public int readInt() {
    int value = 0;
    value = this.read() << 24;
    value += this.read() << 16;
    value += this.read() << 8;
    value += this.read();
    return value;
  }
  
  public long readLong() throws Exception {
    int value = 0;
    value += (long) this.read() << 56;
    value += (long) this.read() << 48;
    value += (long) this.read() << 40;
    value += (long) this.read() << 32;
    value += (long) this.read() << 24;
    value += (long) this.read() << 16;
    value += (long) this.read() << 8;
    value += this.read();
    return value;
  }
  
  public boolean readBoolean() throws Exception {
    return (this.read() == 1)
  }
}
```







read() 호출할 때마다 1바이트 읽어서 1바이트 리턴한다. 어떤 문자집합을 사용햇는지 따지지 않는다.



FileReader

텍스트라면 지정된 문자 집합 규칙으로 읽는다. read()로 통째로 읽어서 UCS2 코드로 리턴한다... 한글이라면 3바이트를 통째로 읽어서 2byte로 리턴한다.



FileOutputStream

getBytes()를 호출하여 바이트배열을 미리 뽑는다. 그리고 이 byte[]를 write()한다. 그러나 FileOutputStream은 바이트를 그대로 뽑는다. 

getByte()에 인코딩 정보를 넘겨줘야 한다.



Character Stream: FileWriter

캐릭터 배열 

```java
FileWriter out = new FileWriter("temp/test2.txt");
String str = new String("AB가각");

```



write()를 하게 되면 우리가 변환할 필요 없이 FileWirter가 변환한다. JVM 환경변수 file.encoding 값에 따라. 

바이트스트림을 쓰면 직접 우리가 바이트 => 문자열/ 문자열 =>바이트를 일일이 변환해줘야 한다.

캐릭터 배열을 줘도 되지만 스트릭 객체를 바로 줘도 된다. 그러면 FileWriter가 알아서 바이트배열로 변환하는 작업을 해준다.

CharacterBuffer는 내부적으로 배열이다. 이 캐릭터 버



> com.eomcs.io.ex04

## 데이터 출력: int값 출력



## DataOutputStream

### 배경

기존에 자바에서 제공해주는 FileOutputStream은 우리가 쓰기에는 불편한 점이 있었다. 

### FileOutputStream

다음은 FileOutputStream을 이용한 Member 데이터의 파일 출력 코드이다. 

```java
FileOutputStream out = new FileOutputStream("temp/test4.data");

Member member = new Member();
member.name = "AB가각간";
member.age = 27;
member.gender = true;

// 인스턴스의 값 출력
// 1) 이름 출력 
byte[] bytes = member.name.getBytes("UTF-8");
out.write(bytes.length); // 1바이트
out.write(bytes); // 문자열 바이트

// 2) 나이 출력
out.write(member.age >> 24);
out.write(member.age >> 16);
out.write(member.age >> 8);
out.write(member.age);

// 3) 성별 출력 (1바이트)
out.write(member.gender ? 1 : 0);
```





```java
public class DataOutputStream extends FileOutputStream {
  public DataOUtputStream(String filepath) throws FileNotFoundException {
    super(filepath);
  }
  
  // String을 바이트 배열로 만들어 출력하는 메서드를 추가한다.
  public void writeUTF(String str) {
    byte[] bytes = str.getBytes("UTF-8");
    this.write(bytes.length); // 1바이트
    this.write(bytes); // 문자열 바이트
  }
  
  // int 값을 4바이트로 출력하는 메서드를 추가한다.
  public void write(int i) throws IOException {
    out.write(i >> 24);
    out.write(i >> 16);
    out.write(i >> 8);
    out.write(i);
  }
  
  // boolean 값을 1바이트로 출력하는 메서드를 추가한다.
  public void writeBoolean(boolean b) throws IOException {
    this.write(b ? 1 : 0);
  }
}
```



```java
public static void main(String[] args) {
  DataOutputStream out = new DataOutputStream("temp/test4.data");
  
  // 1) 이름 출력
  out.writeUTF(member.name);
  
  // 2) 나이 출력 (4바이트)
  out.writeInt(member.age);
  
}
```





```java
public class DataInputStream extends FileInputStream {
  public DataInputStream(String filepath) throws FileNotFoundException {
    super(filepath);
  }
  
  public String readUTF() throws IOException {
    int size = this.read(); // 문자의 바이트 배열 수
    byte[] buf = new byte[size];
    this.read(buf);
    return new String(buf, "UTF-8");
  }
  
  public int readInt() throws IOException {
    int value= 0;
    value = this.read() << 24;
    value += this.read() << 16;
    value += this.read() << 8;
    value += this.read();
    return value;
  }
  
  public boolean readBoolean() throws IOException {
    return in.read() == 1;
  }
}
```



```java
public static void main(String[] args) {
  DataInputStream in = new DataInputStream("temp/test4.data");
  
	Member member = new Member();
  member.nam
}
```



버퍼 없이 읽는 것보다 버퍼 있게 읽는 것이 더 빠르다.

동시에 몇천명이 동시에 실행하면 각각의 프로그램이 4M를 쓰면 시스템 메모리가 부족한 상황이 발생한다. 보통 8k나 16k로 세팅해놓고 쓴다.



```java
public class BufferedInputStream extends FileInputStream {
  byte[] buf = new byte[8192];
  int size = 0;
  int cursor = 0;
  
  
  // 기존의 read() 메서드를 재정의하였다.
  // => read()를 호출하면,
  //    - 일단 파일에서 8192 바이트를 왕창 읽어온다.
  //    - 그런 후 1바이트를 리턴한다.
  //    - 버퍼에 받아 놓은 바이트가 다 떨어질 때까지 반복한다.
  //    - 버퍼에 받아 놓은 바이트가 다 떨어지면 다시 파일에서 8192 바이트를 왕창 읽어온다.
  //
  @Override
  public int read() throws IOException {
    if (cursor == size) { // 버퍼에 데이터가 없거나, 버퍼에서 데이터를 다 읽었으면
      // 버퍼 파일에서 데이터를 읽어 버퍼에 채운다.
      this.size = this.read(buf);
      
      if (this.size == -1)
        return -1;
      this.cursor = 0;
    }
    return (buf[cursor++] && 0x000000ff);
  }
}
```

작은 수를 큰 메모리에 젖아하면 나머지 는 부호비트로 채워진다. -10을 int로 저장하게 되면 1111111111_111111111_1111111111_11110100

```java
System.out.println(b & ~); // 그냥 리턴하는 게 아니라 엔드시키는 이유

```



우물에 가서 한 컵씩 가져오는 것과 같음

버퍼라는에 쌓아놓은 거 다 쓸 때까지... 우물에서 양동이 한바가지 가져다 놓고 쓰다가 다시 양동이로 푸는 것

```java
public class BufferedOutputStream extends FileOutputStream {
  byte[] buf = new byte[8192];
  int cursor = 0;
  
  public BufferedOutputStream(String filepath) throws FileNotFoundException {
    super(filepath);
  }
  
  @Override
  public void write(int b) throws IOException {
    if (cursor == buf.length) { // 버퍼가 꽉 찼다면,
      this.write(buf);
      cursor = 0;
    }
    buf[cursor++] = (byte) b;
  }
}
```

### `flush()`

더 빠르다! 근데 치명적인 문제가 있다. 버퍼가 꽉 차지 않은 상태에서는 버퍼 내용이 파일에 출력이 안된다는 문제다.  따라서 출력 시에는 버퍼에 남아 있는 데이터를 파일에 출력하는 `flush()` 메서드를 오버라이딩해야 한다.

목적: 버퍼에 남아 있는 데이터를 깔끔하게 털어내기 위해서

```java
  // 버퍼에 남아 있는 데이터를 파일에 출력한다.
  // flush: 방출하다. 내보내다.
  @Override
  public void flush() throw IOException {
    if (cursor > 0) {
      this.write(buf, 0, cursor);
    }
  }
```



## BufferedInputStream / BufferedOutputStream

### 버퍼 사용 전

```java
public static void main(String[] args) throws Exception {
  FileInputStream in = new FileInputStream("temp/fls11.pdf");
  
  int b;
  
  
}
```



## ByteArrayInputStream / ByteArrayOutputStream





## 상속관계

### 문제점



바이트값을 읽어 String, int, boolean 값으로 바꾸는 코드를 장신구(decorator) 처럼 붙였다 뗐다 할 수 있게 만든 것 데코레이터 

DataInputStream의 메서드를 FileInputStream과 ByteArrayInputStream을 장신구처럼 붙였다 떼였다 할 수있게 만들고 싶다. 이때 File~과 Byte~는 모두 InputStream의 자식이다. 기존에는  DataInputStream이 상속 받는 관계였다면 지금은 사용하는, 또는 포함하는







## 포함관계: 데코레이터

> com.eomcs.io.ex08

자바 API는 이렇게 포함 관계로 되어 있다. 어떤 놈에게 기능을 덧붙이는 것이다.



### DataInputStream

```java
public class DataInputStream {
  // FileInputStream이나 ByteArrayInputStream을 포함하는 레퍼런스
  InpuStream in;
  public DataInputStream(InputStream in) {
    this.in = in;
  }
}
```



```java
ByteArrayInputStream in = new ByteArrayInputStream(buf);

// 문자열, int, long, boolean 값을 읽는 것은 DataInputStream에 맞긴다.
// => DataInputStream은 ByteArrayInputStream을 포함시킨다.
DataInputStream in2 = new DataInputStream(in);
```

DataInputStream은은 실제 데이터가 저장된 장소로부터 데이터를 읽는 일을 하지 않는다. 단지 읽어 온 데이터를 중간에서 가공 처리하여 리턴하는 일을 한다. 이런 류의 클래스를 "데이터 처리 프로세싱 스트림 클래스 (data processing stream class)"

상속 관계를 포함 관계로 변경했을 때의 이점

- 코드가 중복되지 않는다.

- 장식품처럼 여러 스트림 클래스에 붙여서 사용할 수 있다.
- 즉 재사용성이 높다.

DataInputStream의 생성자에 ByteArrayInputStream 대신 위 예제와 같이 FileInputStream ㅋ르래스처럼 다른 클래스로 쉽게 교체할 수 있다.



### BufferedInputStream / BufferedOutputStream

```java
FileInputStream in = new FileInputStream("temp/test4.data");
// 기존의 FileInputStream 에 버퍼 기능을 덧붙이기 위해서
// 다음과 같이 장신구를 사용한다.


Member member = null;


```



네트워크 통신





9/22~9/27: 오후 8시~9시 하루에 1시간씩 투자하여 미니 프로젝트 만들기

- 회원이 단어장을 CRUD 가능하게



Word

- String eng
- String kor
- 품사: nouns, pronouns, verbs, adverbs, adjective, articles, prepositions, conjunctions, interjections
  - 상수나 enum으로



Member

- `String name`
- `String id`
- `String password`

- `List<Word> wordList`



회원 : 원하는 단어 객체를 만들 수 있다.

- 단어 리스트 필드
- 이름

단어장 이름: 안외워지는 단어 

원하는 단어를 넣고



- - 



Member

- Word



Word