```java
import java.util.ArrayList;
import java.util.Arrays;

public class Pepero {
    public static ArrayList<Integer[]> divideChocolateStick(int M, int N) {
        // TODO: 최대공약수를 구하면 최대 공약수의 약수 갯수만큼 경우의 수가 나온다
        // 2 2   / 2 2 2 - >최대공약수2*2, (1,2,4)
        // 최대공약수는 유클리드 호제로 구하고
        // 최대공약수의 약수들로 M,N을 나눠서
        // list.add {약수, M/약수, N/약수}

        // 결과를 담을 ArrayList선언
        ArrayList<Integer[]> list = new ArrayList<>();
        // gcd메서드로 최대공약수 구하고, int gcd에 저장
        int gcd = gcd(M, N);
        // divisors 메서드를 통해 약수들을 담을 divisor 배열 선언
        Integer[] divisor = divisors(gcd);
        // list의 요소가 될 배열 distribution 선언
        Integer[] distribution = new Integer[3];

        // divisor 배열을 순회하며 각 약수로 M, N을 나눠 분배하고 결과를 distribution에 담아 list에 추가
        for (Integer I : divisor) {
            distribution[0] = I;
            distribution[1] = M / I;
            distribution[2] = N / I;
            list.add(Arrays.copyOf(distribution, 3));
        }

        return list;
    }

    //최대공약수 구하는 메서드(유클리드 호제법)
    public static int gcd(int M, int N) {
        int gcd = 0;
        //M>N 기준으로 작성. N>M이면 gcd(N,M)으로 실행
        if (N > M) return gcd(N, M);
        // 나누어떨어지면 N이 최대공약수
        if (M % N == 0) return N;
            // 나누어떨어지지 않으면, 이전 divider와 나머지로 gcd 호출
        else return gcd(N, M % N);
    }

    //최대공약수의 약수 구하는 메서드(소인수분해)
    public static Integer[] divisors(int gcd) {
        //결과 담을 ArrayList
        ArrayList<Integer> list = new ArrayList<>();
        //1~gcd까지 순회하며 gcd가 i로 나누어떨어지면 list에 추가(약수)
        for (int i = 1; i <= gcd; i++) {
            if (gcd % i == 0) list.add(i);
        }
        //약수를 담은 list를 Integer[]로 변환하여 반환
        return list.toArray(new Integer[0]);
    }
}
```
