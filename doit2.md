

6번

```java
public class Exam0106 {
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);

    System.out.println("1부터 n까지의 합을 구합니다.");
    System.out.print("n의 값: ");
    int n = in.nextInt();

    int sum = 0;
    int i = 1;
    while (i <= n) {
      sum += i;
      i++;
    }

    System.out.println("1부터 " + n + "까지의 합은 " + sum + "입니다.");
    System.out.println("i는 " + i + "입니다.");
  }
}

```

7번

```java
public class Exam0107 {
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    int n = in.nextInt();
    int sum = 0;
    for (int i = 1; i <= n; i++) sum += i;
    System.out.println(sum);
  }
}
```

8번

```java
public class Exam0108 {
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    int n = in.nextInt();
    int result = (n + 1) * (n / 2) + (n % 2 == 0 ? 0 : n / 2 + 1);
    System.out.println(result);
  }
}
```

9번

```java
public static int sumof(int a, int b) {
  int result = 0;
  for (int i = (a < b ? a : b); i <= (a < b ? b : a); i++) 
    result += i;
  return result;
}
```

10번

```java
public class Exam0110 {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    int a, b;

    while (true) {
      a = sc.nextInt();
      b = sc.nextInt();
      System.out.println("a의 값: " + a);
      System.out.println("b의 값: " + b);
      if (b > a)
        break;
      System.out.println("a보다 더 큰 값을 입력하세요!");
    }
    System.out.println("b - a는 " + (b - a) + "입니다.");
  }
}
```

