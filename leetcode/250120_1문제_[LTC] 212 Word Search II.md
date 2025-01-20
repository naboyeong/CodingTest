# 250120 월 스터디

날짜: 2025년 1월 20일

<aside>
📌

**Topic**

**Array, Backtracking**

</aside>

<aside>
📌

문제
[Word Search II](https://leetcode.com/problems/word-search-ii/description/) (문제 풀이 40분, 풀이 설명 10분)

</aside>

### 212. Word Search II

**문제 간단 설명**

<aside>

board가 주어지고 words에 단어리스트가 있을때 board를 이어서 words에 있는 word를 만들 수 있는 단어들을 찾는 문제

</aside>

**문제 풀이**

<aside>

백트래킹 문제라서 백트래킹을 사용해주었다. 백트래킹을 사용하기 위해 단어 문자를 순서대로 체크할 수 있는게 필요했고 딕션어리를 이용하여 이를 구현해주었다. 예를 들어 wordlist 가 단어 사전이라고 한다면 단어가 `oath` , `eat` 가 있다고 할때 첫번째 문자가 `o, e`  이므로 wordlist 에는 {’o’:{}, ‘e’:{}} 이렇게 들어가 있는 것이다. 2번째 문자는 {’o’:{’a’:{}}, ‘e’:{’a’:{}} 이렇게 해당 키를 딕션어리로 해서 사전을 만들어주면 쉽게 백트래킹을 사용할 수 있다. 
일단 단어를 이렇게 사전으로 만들어주고 backtrack을 해볼건데 bfs에서 더이상 탐색할 필요가 없으면 돌아가는게 백트래킹이므로 현재 문자가 단어리스트에 없다면 단어가 만들어지지 않는 다는 의미이므로 return을 시켜주고 있으면 사전을 타고 들어가 다음 문자 딕션어리로 바꾸어준다. 이때 ‘#’ 키 값으로 있어면 해당 단어는 마지막 단어라는 뜻이므로 단어가 존재하게 되면 result에 추가해준다. 

</aside>

**코드**

```python
class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        m, n = len(board), len(board[0])
        nextcheck = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        wordlist = {}
        result = set()

        for i in words:
            nextword = wordlist
            for j in i:
                if j not in nextword:
                    nextword[j] = {}
                
                nextword = nextword[j]
            nextword["#"] = 1

        def backtrack(x, y, current, path):
            check = board[x][y]

            if check not in current:
                return
            
            current = current[check]
            path += check

            if "#" in current:
                result.add(path)
            
            board[x][y] = "@"
            for a, b in nextcheck:
                nx, ny = a + x, y + b
                if 0 <= nx < m and 0 <= ny < n and board[nx][ny] != "@":
                    backtrack(nx, ny, current, path)

            board[x][y] = check

            

        for i in range(m):
            for j in range(n):
                backtrack(i, j, wordlist, "")

        return list(result)
```

다른 스터디원 풀이(bfs로 풀어도 된다고 한다 시간 초과 날것 같아서 시도를 안해봤음)

```python
class Solution:
    def findWords(self, board, words):
        m, n = len(board), len(board[0])
        words = set(words) 
        wordlist = set() 

        check = [(0, 1), (1, 0), (0, -1), (-1, 0)]

        def dfs(x, y, path, visited):

            if path in words:
                wordlist.add(path)

            visited.add((x, y))

            for dx, dy in check:
                new_row, new_col = x + dx, y + dy

                if (
                    0 <= new_row < m and 0 <= new_col < n
                    and (new_row, new_col) not in visited
                ):
                    dfs(new_row, new_col, path + board[new_row][new_col], visited)

            visited.remove((x, y))

        for r in range(m):
            for c in range(n):
                dfs(r, c, board[r][c], set())

        return list(wordlist)
```