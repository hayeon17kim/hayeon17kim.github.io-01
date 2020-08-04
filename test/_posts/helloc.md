#include <stdio.h>
#include <stdlib.h>
// 레퍼런스
int main() {
  int i1, i2, i3;
  i1 = 100;
  i2 = 200;
  i3 = 300;

  // 주소를 담는 변수 = 포인터(pointer)
  int* p;
  p = &i1;	//i1에 들어있는 값이 아니라 i1 메모리의 주소를 이 자리에 넣어라.

  printf("%d(%x) --> %d\n", p, p, *p);
  //p에 들어있는 주소로 찾아가서 그 메모리의 값을 출력하라.

  p= &i2;
  printf("%d(%x) --> %d\n", p, p, *p);

  p = &i3;
  printf("%d(%x) --> %d\n", p, p, *p);

  *p = 500;
  printf("%d\n", i3);


  // 배열
  int i[3];
  i[0] = 100;
  i[1] = 200;
  i[2] = 300;

  printf("%d, %d, %d\n", i[0], i[1], i[2]);

  p = &i[0];
  printf("%d\n", p);

  p = &i[1];
  printf("%d\n", p);

  


  //1번 메모리의 값을 출력하여 
  p = &i[0];
  printf("%d\n", *p);  

  //p = &i[0];
  p = i;
  printf("%d\n", *(p+2));    // 4바이트를 더하라.(BYTE면 1바이트, SHORT면 2바이트. 한 단위씩 증가)
  // 배열의 인덱스가 0부터 시작하는 이유 (주소이기 때문에)

  //(byte*) p2 = malloc(12);   //byte 단위(1바이트)로 쓰기 위해서 12바이트 메모리 확보
  //int* p2 = malloc(12);    //int 단위(4바이트): int 메모리의 주소 -------

  int *p2 = (int *)malloc(sizeof(int) * 3)

  *(p2 + 0) = 110;
  *(p2 + 1) = 220;
  *(p2 + 2) = 330;

  printf("%d, %d, %d\n", *(p2 + 0), *(p2 + 1), *(p2 + 2));
  printf("%d, %d, %d\n", p2[0], p2[1], p2[2]);

  printf("%d\n", *(p2 + 3));    
  printf("%d\n", p2[3]);
  //0 => 이게 바로 c의 문제: 원래 있던 쓰레기값
  //오류가 나올 경우 => 남의 메모리에 침범 

  free(p2); //메모리 회수

  printf("%d, %d, %d\n", *(p2 + 0), *(p2 + 1), *(p2 + 2));  // 해제된 포인터에 접근 (컴파일에서 못막음)

  return 0;
}



Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
	at com.eomcs.basic.ex07.Exam0413.createArray(Exam0413.java:23)
	at com.eomcs.basic.ex07.Exam0413.main(Exam0413.java:13)



- 예외(Exception): 개발자가 구현한 로직에서 발생한다. 발생하더라도 수습할 수 있는 비교적 덜 심각한 오류를 말한다. 프로그래머가 적절히 코드를 작성해주면 비정상적인 종료는 막을 수 있다(발생할 상황을 미리 예측하여 처리할 수 있다.)
  - RuntimeException:  CheckedException과 UncheckedException을 구분하는 기준. 
- 오류(Error): 시스템에 비정상적인 상황이 생겼을 때 발생한다. 시스템 레벨에서 발생하기 때문에 심각한 수준의 오류이고, 개발자가 미리 예측하여 처리할 수 없다. 따라서 애플리케이션에서 오류에 대한 처리를 신경쓰지 않아도 된다. 시스템에 변화를 주어 문제를 처리해야 하는 경우가 일반적이다.



로컬 변수가 너무 많으면 스택 변수가 꽉 찰 수 있음