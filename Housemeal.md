```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;

public class HouseMeal {
    boolean[] picked;
    // 결과 담을 result
    ArrayList<String[]> result = new ArrayList<>();

    public ArrayList<String[]> missHouseMeal(String[] sideDishes) {
        // TODO: 1. dfs 재귀 해보자
        Arrays.sort(sideDishes);
        // 방문 결과 담을 picked 초기화
        picked = new boolean[sideDishes.length];
        // 0개에서 전부 고르는 경우까지 순회하며 pickMeals 메서드 실행
        for (int n = 0; n <= sideDishes.length; n++) {
            String[] pickedMeals = new String[n];
            pickMeals(sideDishes, picked, 0, n, pickedMeals, 0);
        }
        //결과 정렬 후 반환
        result.sort(Comparator.comparing(Arrays::toString));
        return result;
    }

    // n 개의 반찬중에 0개, 1개, 2개, 3개 ... 고르기
    // 반찬종류 배열, 해당 배열 중 pick 한 경우 true 해줄 picked, 현재 pick한 갯수 담을 meals, 목표 갯수 담을 n, 현재 담은 배열, 순회 시작할 인덱스
    public void pickMeals(String[] sideDishes, boolean[] picked, int meals, int n, String[] pickedMeals, int idx) {
        //고른 반찬 수 = 고를 반찬 수 -> 배열 정렬 후 복사하여 result에 추가
        if (meals == n) {
            Arrays.sort(pickedMeals);
            result.add(Arrays.copyOf(pickedMeals, pickedMeals.length));
            return;
        }
        //반찬 순회하면서 추가
        for (int i = idx; i < sideDishes.length; i++) { //0,1,2 순회
                //안고른 항목이면 추가하고 표시
                if (!picked[i]) {
                    pickedMeals[meals] = sideDishes[i];
                    picked[i] = true;
                    //방금 고른 항목의 이후 항목을 순회하도록 재귀호출, 호출이 종료되면 방문 
                    pickMeals(sideDishes, picked, meals + 1, n, pickedMeals, i + 1);
                    picked[i] = false;
                }
        }
    }
}
```
