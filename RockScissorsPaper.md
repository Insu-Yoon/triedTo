혼자 다시 작성해본 코드
```java
import java.util.*;

public class Solution {
    //결과를 담을 ArrayList 선언
    ArrayList<String[]> result = new ArrayList<>();
    //rounds는 불변이므로 필드에서 관리
    int round;
    //가위바위보를 담은 배열
    String[] rps = {"rock", "paper", "scissors"};

    public ArrayList<String[]> rockPaperScissors(int rounds) {
        // TODO: rounds 만큼 가위바위보를 하여, 선택할 수 있는 모든 경우의 수 반환
        // 재귀를 통해 구현
        // for(String s:rps){결과[currentRound] = s; 재귀호출}, 탈출조건은 rounds = currentRound
        round = rounds;
        String[] str = new String[round];
        dfs(0, str);
        return result;
    }

    //ArrayList에 추가만 하는 방식으로 메서드를 구성하면 return타입이 void여도 된다.
    public void dfs(int currentRound, String[] str) {
        //탈출 조건을 만족하면 str을 복사해 결과에 추가하고 리턴
        if (round == currentRound) {
            result.add(Arrays.copyOf(str, round));
            return;
        }
        //rps를 순회하며 str에 가위바위보 할당
        for (int i = 0; i < rps.length; i++) {
            str[currentRound] = rps[i];
            //재귀호출
            dfs(currentRound + 1, str);
        }
        return;
    }
}
```
***
페어분의 도움으로 작성한 코드
```java
import java.util.ArrayList;
import java.util.Arrays;

public class RockPaperScissors {
    String[] rps = {"rock", "paper", "scissors"};
    ArrayList<String[]> result = new ArrayList<>();
    int round;

    public ArrayList<String[]> rockPaperScissors(int rounds) {
        // TODO: (가위, 바위, 보)의 중복순열을 구한다.
        String[] choice = new String[rounds];
        round = rounds;
        dfs(0, choice);
        return result;
    }

    public void dfs(int currentRound, String[] choice) {
        if (currentRound == round) {
            result.add(Arrays.copyOf(choice, round));
            return;
        }
        for (int i = 0; i < rps.length; i++) {
            choice[currentRound] = rps[i];
            dfs(currentRound + 1, choice);
        }
    }
}
```
