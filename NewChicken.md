혼자 다시 작성해본 
```java
public class NewChicken {
    //choiceNum은 불변 -> 클래스변수 num에 할당
    int num;
    //makeChicken 메서드에서 넣은 재료를 기록할 boolean[] put 선언
    boolean[] put;
    //makeChicken 메서드에서 재료를 담을 choice배열 선언
    Integer[] choice;
    // 결과를 담을 result 생성
    ArrayList<Integer[]> result = new ArrayList<>();

    public ArrayList<Integer[]> newChickenRecipe(int[] stuffArr, int choiceNum) {
        // TODO: 주어진 재료 중 0이 3개이상 -> 폐기, choiceNum만큼 선택해 순열
        // 재귀로 dfs구현해서 풀이

        // choice 배열 생성
        choice = new Integer[choiceNum];
        // 클래스변수에 choiceNum 할당
        num = choiceNum;
        // 신선한 재료만 가져오기
        Integer[] freshStuff = getFreshStuff(stuffArr);
        // 신선한 재료가 0개거나, num보다 작으면 null return;
        if(freshStuff.length==0||freshStuff.length<num) return null;
        // put 배열을 freshStuff 의 길이에 맞게 생성 후 초기화
        put = new boolean[freshStuff.length];
        // 치킨만들기 메서드 실행
        makeChicken(freshStuff,0,choice,put);
        // 메서드 실행 후 만들어진 결과 반환
        return result;
    }
    //신선한 재료만 선별하는 메서드
    public Integer[] getFreshStuff(int[] stuffArr){
        //신선한 재료를 담을 ArrayList선언
        ArrayList<Integer> fresh = new ArrayList<>();
        //stuffArr를 순회
        for(int i=0;i<stuffArr.length;i++){
            //stuffArr를 String으로 만들고, 스트림 적용하여 0이 3개미만인 경우만 '신선'
            String str = "";
            str += stuffArr[i];
            //0이 3개미만이면 fresh에 추가
            if(str.chars().filter(s->s=='0').count()<3) {
                fresh.add(stuffArr[i]);
            }
        }
        //fresh를 Integer타입 배열로 변환하고 정렬
        Integer[] freshStuff = new Integer[fresh.size()];
        freshStuff = fresh.toArray(freshStuff);
        Arrays.sort(freshStuff);

        //신선한 재료만 담은 freshStuff반환
        return freshStuff;
    }
    //주어진 재료로 순열을 만드는 메서드(재료, 현재넣은재료수, 현재 넣은 재료, 재료 넣었는지 여부 기록)
    public void makeChicken(Integer[] freshStuff, int currentNum, Integer[] choice, boolean[] put){
        // 넣은 재료수가 num과 같으면 현재 넣은 재료를 기록하고 return
        if(num == currentNum) {
            result.add(Arrays.copyOf(choice, choice.length));
            return;
        }
        //재료를 순회
        for(int i=0;i<freshStuff.length;i++){
            //넣지 않은 재료면 넣고 put[i]에 기록
            if(!put[i]) {
                choice[currentNum] = freshStuff[i];
                put[i] = true;
                //재귀호출
                makeChicken(freshStuff, currentNum+1, choice, put);
                //재귀호출이 끝나면 put[i] 초기화
                put[i] = false;
            }
        }
    }
}
```


페어분의 도움으로 만들어본 코드
```java
public class Solution {
	ArrayList<Integer[]> result = new ArrayList<>();
    static Integer[] freshStuffArr;
    static int choicenum;

    public ArrayList<Integer[]> newChickenRecipe(int[] stuffArr, int choiceNum) {
        ArrayList<Integer> freshStuff = new ArrayList<>();
        choicenum = choiceNum;
        Integer[] choice = new Integer[choicenum];

        String str;
        for(int i=0;i<stuffArr.length;i++){
            str = "";
            str+=stuffArr[i];
            //split쓰면 index = 0 에 1이올때 이상해짐
            //따로 메서드빼서 %10 -> /10, %10 -> /10
            //charAt() == 0 ->
            //String으로 바꾸고 .chars int stream.filter
            if(isFresh(stuffArr[i])) freshStuff.add(stuffArr[i]);
        }
        if(freshStuff.size()==0||freshStuff.size()<choiceNum) return null;
        freshStuffArr = freshStuff.toArray(new Integer[freshStuff.size()]);
        Arrays.sort(freshStuffArr);

        boolean[] visited = new boolean[freshStuffArr.length];

        dfs(visited, choice, 0);
        return result;
    }
    public boolean isFresh(int stuff){
        String str = "";
        str+=stuff;
        return str.chars()
                .filter(s->s=='0')
                .count() < 3;
    }

    public void dfs(boolean[] visited, Integer[] choice, int depth){
        if(choicenum == depth) {
            result.add(Arrays.copyOf(choice,choice.length));
            return;
        }
        for(int i=0;i<freshStuffArr.length;i++){
            if(!visited[i]) {
                visited[i] = true;
                choice[depth] = freshStuffArr[i];
                dfs(visited, choice, depth+1);
                visited[i] = false;
            }
        }
    }
}
```
