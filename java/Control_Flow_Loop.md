# [Java] 제어문 - 반복문 (while, for) 및 제어 키워드

동일한 작업을 반복해서 수행할 때 사용하는 **반복문**과 반복의 흐름을 제어하는 **키워드**를 정리함.

## 1. 반복문의 종류

### 1-1. while 문
조건식이 참(true)인 동안 블록 내부를 반복 실행함. 반복 횟수가 유동적이거나 특정 조건 충족 시까지 실행해야 할 때 유리함.
- **구성 요소**: 조건 변수 초기화, 조건식 검사, 조건 변수 변경(증감식)의 3단계로 이루어짐.

### 1-2. for 문
초기화식, 조건식, 증감식을 한 줄에 작성하여 가독성이 높으며, 반복 횟수가 명확할 때 주로 사용함.
- **중첩 루프**: 루프 내부에 또 다른 루프를 배치하여 2차원적인 데이터 처리(예: 구구단)에 활용함.

### 1-3. 실습 예제 코드 (Standard Edition)
```
public class LoopSummary {
    public static void main(String[] args) {
        // 1. while문 기본 구조
        int count = 0;
        while (count < 5) {
            System.out.println("현재 카운트: " + count);
            count++; // 조건 변수 변경
        }

        // 2. for문을 이용한 중첩 루프 (구구단)
        for (int dan = 2; dan <= 9; dan++) {
            for (int su = 1; su <= 9; su++) {
                System.out.printf("%d * %d = %d\t", dan, su, dan * su);
            }
            System.out.println(); // 단 변경 시 줄바꿈
        }
    }
}
```

## 2. 반복 제어 키워드 (break, continue)

| 키워드 | 기능 및 설명 |
| :--- | :--- |
| **break** | 현재 실행 중인 가장 가까운 반복문을 즉시 탈출함. 무한 루프 종료 시 필수적임. |
| **continue** | 아래 코드를 무시하고 다음 반복(증감식/조건식)으로 즉시 이동함. |

### 2-1. Labeled break
중첩 루프 내부에서 하위 루프뿐만 아니라 상위 루프까지 한 번에 빠져나가고 싶을 때 레이블(`label:`)을 정의하여 사용함.

### 2-2. 실습 예제 코드 (Standard Edition)
```
public class ControlKeyword {
    public static void main(String[] args) {
        // Labeled break: 특정 조건 만족 시 전체 루프 종료
        exitLabel:
        for (int i = 1; i <= 5; i++) {
            for (int j = 1; j <= 5; j++) {
                if (i + j == 7) {
                    System.out.println("목표값 발견. 전체 종료.");
                    break exitLabel; 
                }
            }
        }

        // continue: 특정 조건에서만 실행 건너뛰기 (홀수만 출력)
        for (int k = 1; k <= 10; k++) {
            if (k % 2 == 0) continue; 
            System.out.print(k + " ");
        }
    }
}
```

## 3. 무한 루프와 사용자 입력 제어
`while(true)` 문을 사용하여 프로그램이 계속 실행되게 하고, 특정 입력값(예: "X")이 들어올 때 `break`를 통해 안전하게 종료하는 로직 설계에 활용함.

### 3-1. 실습 예제 코드 (Standard Edition)
```
import java.util.Scanner;

public class InfiniteMenu {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        while (true) {
            System.out.print("명령 입력(종료 시 X): ");
            String cmd = sc.next();
            
            // 문자열 비교 시 .equalsIgnoreCase() 사용이 유연함
            if (cmd.equalsIgnoreCase("X")) {
                System.out.println("프로그램을 종료함.");
                break;
            }
            System.out.println("실행 명령: " + cmd);
        }
        sc.close();
    }
}
```