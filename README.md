# Strassen Matrix Algorithm
: 행렬 곱셈 알고리즘    
Strassen 알고리즘은 분할 정복을 이용한 알고리즘으로 두 행렬의 크기가 n x n인 정사각행렬의 곱셈에서 사용할 수 있다.
일반적인 행렬의 곱셈은 시간복잡도가 O(n^3)으로 행렬의 크기가 커질수록 연산이 기하급수적으로 많아지지만, Strassen 알고리즘을 이용하면 곱셈 연산 횟수가 줄어들어 시간복잡도가 O(n^2.807)로 더 효율적이다.

## Strassen 알고리즘 설명
- A와 B의 행렬의 크기가 (2^n) x (2^n)이 아니면 모자라는 행과 열을 0으로 채운다.    

1. 정사각행렬 A, B, C를 4개의 (n/2) x (n/2) 행렬로 나눈다. (AXB=C)    
![1](https://postfiles.pstatic.net/MjAyMTA0MjdfNDMg/MDAxNjE5NTAxNTU0ODU3.Wz39l0If1A6pk1wl9v8x7TDh81FQn9cba_eHQwtO1Ssg.Lh0LS6VH60I54IjrN6GhoRbHxOEf94vXE_FsRoK8d3Ag.PNG.hongsubakgame/image.png?type=w966)    
이때, C 행렬은 다음과 같이 나타낼 수 있다.    
![2](https://postfiles.pstatic.net/MjAyMTA0MjdfMTY1/MDAxNjE5NTAxNTYwNDc0.ag_5vOut52UNSwprc7GJJYXCQFqikOa10IvtwguiH2gg.OMNpZ-KKVsIuAlmGb7yGdGsdZxwlCSmcez7wyIfBq4Eg.PNG.hongsubakgame/image.png?type=w966)    
2. 곱셈 연산을 줄이기 위해 M 행렬을 다음과 같이 정의한다.    
![3](https://postfiles.pstatic.net/MjAyMTA0MjdfMjgz/MDAxNjE5NTAxNTY1MjMy.IdwMs0TkWAOz22Sz94bjAl5Ib_L72pqYaaf3PpXkdegg.ucrLMI04qLzaR39TzgQnoqr46js9qKARqc3qcz8AbeQg.PNG.hongsubakgame/image.png?type=w966)    
3. M 행렬을 이용하여 C행렬을 표현한다.    
![4](https://postfiles.pstatic.net/MjAyMTA0MjdfNDgg/MDAxNjE5NTAxNTcwMTEz.DGRtZKrEcy0FPrhe3Vj4vrDk2b1uLbN1Msk-XNXklqIg.NEG7pwUB09SEfro01Du0uvQqmP4gG9CmWrfiNbMzho0g.PNG.hongsubakgame/image.png?type=w966)    

## 시간복잡도
### 일반적인 방법 : O(n^3)
- 곱셈    
T(n) = n x n x n = n^3    
- 덧셈    
T(n) = (n-1) x n x n = n^3 - n^2    

-> 행렬의 곱셈을 Strassen 알고리즘을 적용하지 않고 구하면, 시간복잡도가 O(n^3)이 된다.    

### Strassen 알고리즘 적용 : O(n^2.807)
- 곱셈 연산     
T(n) = 7T(n/2)    
T(1) = 1    
-> T(n) = 7^logn = n^log7 -> n^2.807     
- 덧셈 / 뺄셈 연산    
(곱셈 7번, 덧셈 18번)    
T(n) = 7T(n/2) + 18(n/2)^2    
T(1) = 0    
-> T(n) = 6n^log7 - 6n^2 -> 6n^2.807 - 6n^2    

-> Strassen 알고리즘을 이용하여 구하면 시간복잡도가 O(n^2.807)로 줄어든다. 대신 저장해야 하는 메모리가 늘어나서 공간복잡도는 늘어나게 된다.        
- Strassen 알고리즘을 개선한 Winograd 알고리즘도 있지만 시간복잡도가 크게 차이나지는 않는다. 

## 코드 설명
: 행렬의 크기 n을 입력하면 n크기의 정사각행렬 두개가 랜덤으로 만들어진 후, 이 두 행렬의 곱셈을 strassen 함수를 통해 구한다.
- 입력 : 행렬의 크기
- 출력 : 두 행렬, 두 행렬의 곱

![capture1](https://postfiles.pstatic.net/MjAyMTA0MjdfMTUg/MDAxNjE5NTA4MzAxNTQx.gprUL8pc9bs2MVI6nVfBr6q5SPyH5YwJzZNYYe0XU1kg.ruKkJvzwt0AfCb644czqHoo-__h09kbX4GWZG-jfsr4g.PNG.hongsubakgame/image.png?type=w966)

## 전체 코드
``` java
import java.util.Random;
import java.util.Scanner;

public class StrassenAlgorithm {

    public static void main(String[] args) {
        StrassenAlgorithm strassen = new StrassenAlgorithm();
        Scanner scanner = new Scanner(System.in);
        Random random = new Random();

        // 행렬의 크기 입력
        System.out.print("행렬의 크기 : ");
        int n = scanner.nextInt();

        // 크기가 n인 정사각행렬 2개(A,B) 생성
        int[][] A = new int[n][n];
        int[][] B = new int[n][n];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                A[i][j] = random.nextInt(10);
                B[i][j] = random.nextInt(10);
            }
        }

        // 행렬 A,B 출력
        System.out.println("행렬 A"); strassen.printMatrix(A);
        System.out.println("행렬 B"); strassen.printMatrix(B);

        // 행렬의 크기가 2의 제곱값이 아닌 경우 -> n을 n보다 큰 가장 작은 2의 제곱값으로 바꿔서 계산
        if (n != (n &- n)) {
            int s = n;
            int power = 1;
            while (power < n) {
                power *= 2;
            }
            n = power;

            int[][] D = new int[n][n];
            int[][] E = new int[n][n];

            for (int i = 0; i < A.length; i++) {
                for (int j = 0; j < A.length; j++) {
                    D[i][j] = A[i][j];
                    E[i][j] = B[i][j];
                }
            }

            int[][] C = strassen.mul(D, E);
            System.out.println("행렬 C");
            for (int i = 0; i < s; i++) {
                for (int j = 0; j < s; j++) {
                    System.out.print(C[i][j] + " ");
                }
                System.out.println();
            }
        }

        // 행렬의 크기가 2의 제곱값인 경우 -> 그냥 계산
        else {
            int[][] C = strassen.mul(A, B);
            System.out.println("행렬 C");
            strassen.printMatrix(C);
        }
    }

    // 행렬 출력하기
    public void printMatrix(int[][] A) {
        int n = A.length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                System.out.print(A[i][j] + " ");
            }
            System.out.println();
        }
    }

    // 행렬의 덧셈
    public int[][] add(int[][] A, int[][] B) {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                C[i][j] = A[i][j] + B[i][j];
            }
        }
        return C;
    }

    // 행렬의 뺄셈
    public int[][] sub(int[][] A, int[][] B) {
        int n = A.length;
        int[][] C = new int[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                C[i][j] = A[i][j] - B[i][j];
            }
        }
        return C;
    }

    // 행렬 분할하기
    public void divide(int[][] P, int[][] C, int X, int Y) {
        int n = C.length;
        for (int i = 0, x = X; i < n; i++, x++) {
            for (int j = 0, y = Y; j < n; j++, y++) {
                C[i][j] = P[x][y];
            }
        }
    }

    // 행렬 합치기
    public void combine(int[][] C, int[][] P, int X, int Y) {
        int n = C.length;
        for (int i = 0, x = X; i < n; i++, x++) {
            for (int j = 0, y = Y; j < n; j++, y++) {
                P[x][y] = C[i][j];
            }
        }
    }

    // 행렬 곱셈
    public int[][] mul(int[][] A, int[][] B) {
        int n = A.length;
        int[][] result = new int[n][n]; // result : 행렬 곱의 결과

        // 재귀 종료 조건
        if (n == 1) {
            result[0][0] = A[0][0] * B[0][0];
        }
        else {
            // 분할한 A 행렬을 저장할 변수 생성
            int[][] A11 = new int[n/2][n/2];
            int[][] A12 = new int[n/2][n/2];
            int[][] A21 = new int[n/2][n/2];
            int[][] A22 = new int[n/2][n/2];

            // 분할한 B 행렬을 저장할 변수 생성
            int[][] B11 = new int[n/2][n/2];
            int[][] B12 = new int[n/2][n/2];
            int[][] B21 = new int[n/2][n/2];
            int[][] B22 = new int[n/2][n/2];

            // 행렬 A 분할
            divide(A, A11, 0, 0);
            divide(A, A12, 0, n/2);
            divide(A, A21, n/2, 0);
            divide(A, A22, n/2, n/2);

            // 행렬 B 분할
            divide(B, B11, 0, 0);
            divide(B, B12, 0, n/2);
            divide(B, B21, n/2, 0);
            divide(B, B22, n/2, n/2);

            // M 행렬 정의 (곱셈 7번, 덧셈 10번)
            int[][] M1 = mul(add(A11, A22), add(B11, B22));
            int[][] M2 = mul(add(A21, A22), B11);
            int[][] M3 = mul(A11, sub(B12, B22));
            int[][] M4 = mul(A22, sub(B21, B11));
            int[][] M5 = mul(add(A11, A12), B22);
            int[][] M6 = mul(sub(A21, A11), add(B11, B12));
            int[][] M7 = mul(sub(A12, A22), add(B21, B22));

            // M 행렬을 이용하여 C 행렬 구하기 (덧셈 8번)
            // C 행렬 : 분할한 행렬의 곱셈 결과
            int[][] C11 = add(sub(add(M1, M4), M5), M7);
            int[][] C12 = add(M3, M5);
            int[][] C21 = add(M2, M4);
            int[][] C22 = add(add(sub(M1, M2), M3), M6);

            // 행렬 합치기
            combine(C11, result, 0, 0);
            combine(C12, result, 0, n/2);
            combine(C21, result, n/2, 0);
            combine(C22, result, n/2, n/2);
        }
        return result;
    }
}
```

## 시간복잡도 측정
### 행렬 크기에 따른 연산 횟수

- 곱셈 연산 시, 전역변수 count를 증가시키면서 연산 횟수 저장
  | 행렬의 크기(n) | 2   | 4   | 6   | 8   | 10   | 12   | 14   | 16   | 18   | 20  | 22   | 24   | 26   | 28  | 30   | 32   |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | 연산 횟수 | 8   | 57   | 400   | 400   | 2801   | 2801   | 2801   | 2801   | 19608   | 19608  | 19608   | 19608   | 19608   | 19608  | 19608   | 19608   |
  | O(n^2.807) | 6   | 48   | 150   | 337   | 630   | 1051   | 1618   | 2352   | 3271   | 4393  | 5738   | 7321   | 9160   | 11273  | 13675   | 16383   |
  | O(n^3) | 8   | 64   | 216   | 512   | 1000   | 1728   | 2744   | 4096   | 5832   | 8000  | 10648   | 13824   | 17576   | 21952  | 27000   | 32768   |

![graph](https://postfiles.pstatic.net/MjAyMTA0MjdfMjQz/MDAxNjE5NTIxMjc3MjIy.HhOkb0m0xqHARLqXpSfZ6EA3DgeyLjfslXrZ3u60uY8g.1LRDX4TpHji4DcUKF6otm-FLRYRMpRHaraBJL2z5jUsg.PNG.hongsubakgame/image.png?type=w966)
- 검정 : 연산횟수 -> count값 (행렬의 크기가 2^n일떄만)
- 파랑 : O(n^2.807) -> Strassen 알고리즘 이론값
- 주황 : O(n^3) -> 그냥 계산했을 때 이론값

## 결론
처음에 행렬의 크기가 작을 때는 일반적인 방법과 Strassen 알고리즘이 비슷한 연산횟수를 가졌다.
행렬의 크기가 커지면서 연산횟수의 차이도 커졌고, 이를 통해 Strassen 알고리즘의 성능이 좋다는 것을 알 수 있었다.
하지만 위의 코드에서 행렬의 크기가 (2^n) x (2^n)이 아니면 행렬의 크기는 n보다 큰 2의 제곱값으로 계산된다.
그러므로 행렬의 크기가 (2^n) x (2^n)일 경우에만 Strassen 알고리즘의 성능을 갖고, 행렬의 크기가 (2^n) x (2^n)이 아닌 경우에는 일반적인 방법보다 성능이 안 좋을 수 있다. (안 좋은 경우가 더 많았음)
또한 위에서 측정한 연산 횟수 count값은 곱셈 연산 과정만 센 것으로, 실제로 Strassen 알고리즘을 짜기 위해 필요한 분할 등의 추가적인 과정은 포함하지 않은 것이다. 
따라서 이론값으로는 Strassen 알고리즘이 일반적인 방법보다 항상 성능이 더 좋지만 내가 짠 코드는 그렇지 않았다.
특정한 임계값을 정하여 행렬의 크기가 임계값보다 크고 (2^n) x (2^n)일 때만 Strassen 알고리즘을 적용하고 나머지의 경우에는 일반적인 방법을 통해 행렬 곱을 구하도록 코드를 수정하면 더 효율적인 코드가 될 것 같다.
이렇게 하면 정사각행렬이 아닌 경우에도 행렬 곱을 구할 수 있을 것이다. 
