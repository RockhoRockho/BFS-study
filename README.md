# BFS-study
BFS 공부 및 정리
--

## Day 1 (2021-08-31)

---

### **백준 14503번**  

**문제**
로봇 청소기가 주어졌을 때, 청소하는 영역의 개수를 구하는 프로그램을 작성하시오.

로봇 청소기가 있는 장소는 N×M 크기의 직사각형으로 나타낼 수 있으며, 1×1크기의 정사각형 칸으로 나누어져 있다. 각각의 칸은 벽 또는 빈 칸이다. 청소기는 바라보는 방향이 있으며, 이 방향은 동, 서, 남, 북중 하나이다. 지도의 각 칸은 (r, c)로 나타낼 수 있고, r은 북쪽으로부터 떨어진 칸의 개수, c는 서쪽으로 부터 떨어진 칸의 개수이다.

로봇 청소기는 다음과 같이 작동한다.

1. 현재 위치를 청소한다.
2. 현재 위치에서 현재 방향을 기준으로 왼쪽 방향부터 차례대로 인접한 칸을 탐색한다.
  a. 왼쪽 방향에 아직 청소하지 않은 공간이 존재한다면, 그 방향으로 회전한 다음 한 칸을 전진하고 1번부터 진행한다.
  b. 왼쪽 방향에 청소할 공간이 없다면, 그 방향으로 회전하고 2번으로 돌아간다.
  c. 네 방향 모두 청소가 이미 되어있거나 벽인 경우에는, 바라보는 방향을 유지한 채로 한 칸 후진을 하고 2번으로 돌아간다.
  d. 네 방향 모두 청소가 이미 되어있거나 벽이면서, 뒤쪽 방향이 벽이라 후진도 할 수 없는 경우에는 작동을 멈춘다.
* 로봇 청소기는 이미 청소되어있는 칸을 또 청소하지 않으며, 벽을 통과할 수 없다.

**입력**
첫째 줄에 세로 크기 N과 가로 크기 M이 주어진다. (3 ≤ N, M ≤ 50)

둘째 줄에 로봇 청소기가 있는 칸의 좌표 (r, c)와 바라보는 방향 d가 주어진다. d가 0인 경우에는 북쪽을, 1인 경우에는 동쪽을, 2인 경우에는 남쪽을, 3인 경우에는 서쪽을 바라보고 있는 것이다.

셋째 줄부터 N개의 줄에 장소의 상태가 북쪽부터 남쪽 순서대로, 각 줄은 서쪽부터 동쪽 순서대로 주어진다. 빈 칸은 0, 벽은 1로 주어진다. 지도의 첫 행, 마지막 행, 첫 열, 마지막 열에 있는 모든 칸은 벽이다.

로봇 청소기가 있는 칸의 상태는 항상 빈 칸이다.

**출력**
로봇 청소기가 청소하는 칸의 개수를 출력한다.

---

### **풀이**

``` n,m = map(int, input().split()) # 세로크기, 가로크기
r,c,d = map(int,input().split()) # (r,c)좌표와 방향d
s = [list(map(int,input().split())) for _ in range(n)] # 지도 데이터

### 0 -> 북쪽 = 왼쪽방향 적용시 '서쪽' 3
1 -> 동쪽 = 왼쪽방향 적용시 '북쪽' 0
2 -> 남쪽 = 왼쪽방향 적용시 '동쪽' 1
3 -> 서쪽 = 왼쪽방향 적용시 '남쪽' 2 ###

dx = [-1, 0, 1, 0] # y축이동
dy = [0, 1, 0, -1] # x축이동

def turn_left(d):
  global d
  d = (d-1) % 4 # 위 조건 성립
  
x,y = r,c 
s[x][y] = 2 # 청소함(2)
count = 1 # 청소기 사용횟수

while True:
    check = False # check 함수로 구분
    for i in range(4):
      turn_left()
      nx = x + dx[d]
      ny = y + dy[d]
      if 0<=nx<n and 0<=ny<m and s[nx][ny] == 0:
        s[nx][ny] = 2 # a번 조항
        x,y = nx,ny
        count += 1 # 청소기 이동수 추가
        check = True # 아래 식 continue와 비슷한 기능
        break
    if not check:
      nx = x - dx[d]
      ny = y - dy[d]
      if 0<=nx<n and 0<=ny<m:
        if s[nx][ny] == 2:
          x,y = nx, ny # 안맞아서 c번조항에 따름
        elif s[nx][ny] == 1:
          print(count) # d번조항
          break
      else:
        print(count)
        break # d번조항 
```
        
--

## Day 2 (2021-09-01)

--

### **백준 1260번**  

**문제**  
그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.  

**입력**  
첫째 줄에 정점의 개수 N(1 ≤ N ≤ 1,000), 간선의 개수 M(1 ≤ M ≤ 10,000), 탐색을 시작할 정점의 번호 V가 주어진다. 다음 M개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.  

**출력**  
첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다. V부터 방문된 점을 순서대로 출력하면 된다. 

--

**풀이**  

``` n,m,v = map(int,input().split())
visit = [0 for _ in range(n+1)]
s = [[0]*(n+1) for _ in range(n+1)]
for _ in range(m):
  x,y = map(int,input().split())
  s[x][y] = 1
  s[y][x] = 1 # branch는 x,y/y,x 의 그래프에 입력하여 그림

def dfs(v):
  print(v, end=' ')
  visit[v] = 1
  for i in range(1, n+1):
    if visit[i] == 0 and s[v][i] == 1:
      dfs(i)

def bfs(v):
  q = [v]
  visit[v] = 0
  while(q):
    v = q[0]
    print(v, end=' ')
    del q[0]
    for i in range(1, n+1):
      if visit[i] == 1 and s[v][i] == 1:
        visit[i] = 0
        q.append(i) # dfs와 달리 바로 다음으로 넘어가지 않고 끝처리 마무리 후 

dfs(v)
print() # 한칸 뛰기
bfs(v) 
```

---

## Day 3 (2021-09-02)

--

### **백준 2178번**  

**문제**  
N×M크기의 배열로 표현되는 미로가 있다.  

1	0	1	1	1	1  
1	0	1	0	1	0  
1	0	1	0	1	1  
1	1	1	0	1	1  

미로에서 1은 이동할 수 있는 칸을 나타내고, 0은 이동할 수 없는 칸을 나타낸다. 이러한 미로가 주어졌을 때, (1, 1)에서 출발하여 (N, M)의 위치로 이동할 때 지나야 하는 최소의 칸 수를 구하는 프로그램을 작성하시오. 한 칸에서 다른 칸으로 이동할 때, 서로 인접한 칸으로만 이동할 수 있다.  

위의 예에서는 15칸을 지나야 (N, M)의 위치로 이동할 수 있다. 칸을 셀 때에는 시작 위치와 도착 위치도 포함한다.  

**입력**  
첫째 줄에 두 정수 N, M(2 ≤ N, M ≤ 100)이 주어진다. 다음 N개의 줄에는 M개의 정수로 미로가 주어진다. 각각의 수들은 붙어서 입력으로 주어진다.  

**출력**  
첫째 줄에 지나야 하는 최소의 칸 수를 출력한다. 항상 도착위치로 이동할 수 있는 경우만 입력으로 주어진다.  

```
from collections import deque # 임포트 deque 함(bfs는 거의 필수)

n,m = map(int,input().split())
s = [list(map(int, input())) for _ in range(n)] # 지도 그리기 문자열이 붙여서 나오므로 split는 생략!
v = [[0]*m for _ in range(n)] # 2차원 방문확인

dx = [-1, 1, 0, 0] # x축이동
dy = [0, 0, -1, 1] # y축이동

def bfs():
  q = deque()
  q.append((0,0))
  v[0][0] = 1
  while q:
    x,y = q.popleft()
    for i in range(4):
      nx = x + dx[i]
      ny = y + dy[i]
      if 0<=nx<n and 0<=ny<m and v[nx][ny] == 0 and s[nx][ny] == 1: # 범위안에서 방문x, 1로 쓰여진 지도값 찾기
        v[nx][ny] = 1
        s[nx][ny] = s[x][y] + 1
        q.append((nx, ny)) # dfs라면 여기서 함수값이 들어가지만 bfs는 그전 트리가 처리된 이후에 한다!
bfs()
print(s[n-1][m-1])
```

--

## Day 3 (2021-09-03)

--

### **백준 2606번**  

**문제**  
신종 바이러스인 웜 바이러스는 네트워크를 통해 전파된다. 한 컴퓨터가 웜 바이러스에 걸리면 그 컴퓨터와 네트워크 상에서 연결되어 있는 모든 컴퓨터는 웜 바이러스에 걸리게 된다.  

예를 들어 7대의 컴퓨터가 <그림 1>과 같이 네트워크 상에서 연결되어 있다고 하자. 1번 컴퓨터가 웜 바이러스에 걸리면 웜 바이러스는 2번과 5번 컴퓨터를 거쳐 3번과 6번 컴퓨터까지 전파되어 2, 3, 5, 6 네 대의 컴퓨터는 웜 바이러스에 걸리게 된다. 하지만 4번과 7번 컴퓨터는 1번 컴퓨터와 네트워크상에서 연결되어 있지 않기 때문에 영향을 받지 않는다.  



어느 날 1번 컴퓨터가 웜 바이러스에 걸렸다. 컴퓨터의 수와 네트워크 상에서 서로 연결되어 있는 정보가 주어질 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 출력하는 프로그램을 작성하시오.  

**입력**  
첫째 줄에는 컴퓨터의 수가 주어진다. 컴퓨터의 수는 100 이하이고 각 컴퓨터에는 1번 부터 차례대로 번호가 매겨진다. 둘째 줄에는 네트워크 상에서 직접 연결되어 있는 컴퓨터 쌍의 수가 주어진다. 이어서 그 수만큼 한 줄에 한 쌍씩 네트워크 상에서 직접 연결되어 있는 컴퓨터의 번호 쌍이 주어진다.  

**출력**  
1번 컴퓨터가 웜 바이러스에 걸렸을 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 첫째 줄에 출력한다.  

```
from collections import deque

n=int(input())
m=int(input())
v = [0 for _ in range(n+1)]
s = [[0]*(n+1) for _ in range(n+1)]
for i in range(m):
  x,y = map(int,input().split())
  s[x][y] = 1
  s[y][x] = 1

r = [] # dfs문제에서 해당 조건에 맞는 값을 리스트'r'에 추가한다.
def dfs(a):
  v[a] = 1
  for i in range(n+1):
        if s[a][i] == 1 and v[i] == 0:
            r.append(i)
            dfs(i)
  return len(r) # 길이 값을 리턴함

print(dfs(1))

```

--

## Day 4 (2021-09-04)

--

### **백준 2667번**  

**문제**  
<그림 1>과 같이 정사각형 모양의 지도가 있다. 1은 집이 있는 곳을, 0은 집이 없는 곳을 나타낸다. 철수는 이 지도를 가지고 연결된 집의 모임인 단지를 정의하고, 단지에 번호를 붙이려 한다. 여기서 연결되었다는 것은 어떤 집이 좌우, 혹은 아래위로 다른 집이 있는 경우를 말한다. 대각선상에 집이 있는 경우는 연결된 것이 아니다. <그림 2>는 <그림 1>을 단지별로 번호를 붙인 것이다. 지도를 입력하여 단지수를 출력하고, 각 단지에 속하는 집의 수를 오름차순으로 정렬하여 출력하는 프로그램을 작성하시오.  

**입력**  
첫 번째 줄에는 지도의 크기 N(정사각형이므로 가로와 세로의 크기는 같으며 5≤N≤25)이 입력되고, 그 다음 N줄에는 각각 N개의 자료(0혹은 1)가 입력된다.  

**출력**  
첫 번째 줄에는 총 단지수를 출력하시오. 그리고 각 단지내 집의 수를 오름차순으로 정렬하여 한 줄에 하나씩 출력하시오.  

```
from collections import deque

n = int(input())
s = [list(map(int,input())) for _ in range(n)] # 지도그리기
v = [[0]*n for _ in range(n)] # 방문기록
dx = [-1, 1, 0, 0] # x축 이동
dy = [0, 0, -1, 1] # y축 이동

def dfs(cnt, i, j): # cnt를 추가하는 이유 - 단지수를 재기 위해 매개변수를 추가함 / dfs로 추가하면 편함
  q = deque()
  q.append((i,j))
  v[i][j] = 1
  while q:
    x,y = q.popleft()
    for i in range(4):
      nx = x + dx[i]
      ny = y + dy[i]
      if 0<=nx<n and 0<=ny<n and s[nx][ny] and v[nx][ny] == 0:
        v[nx][ny] = 1
        cnt = dfs(cnt+1, nx, ny) # 단지수를 기록하기 위해
  return cnt # 최종적으로 단지수로만 출력하기 위함

dab = [] # 단지 수 기록하는 리스트
for i in range(n):
  for j in range(n):
    if s[i][j] == 1 and v[i][j] == 0:
      dab.append(bfs(1, i, j))

print(len(dab)) # 총 갯수
for i in sorted(dab): # 단지 수
  print(i)
  
```

-----

## Day 4 (2021-09-04)

-----

### **백준 7569번**  

**문제**  
철수의 토마토 농장에서는 토마토를 보관하는 큰 창고를 가지고 있다. 토마토는 아래의 그림과 같이 격자모양 상자의 칸에 하나씩 넣은 다음, 상자들을 수직으로 쌓아 올려서 창고에 보관한다.  



창고에 보관되는 토마토들 중에는 잘 익은 것도 있지만, 아직 익지 않은 토마토들도 있을 수 있다. 보관 후 하루가 지나면, 익은 토마토들의 인접한 곳에 있는 익지 않은 토마토들은 익은 토마토의 영향을 받아 익게 된다. 하나의 토마토에 인접한 곳은 위, 아래, 왼쪽, 오른쪽, 앞, 뒤 여섯 방향에 있는 토마토를 의미한다. 대각선 방향에 있는 토마토들에게는 영향을 주지 못하며, 토마토가 혼자 저절로 익는 경우는 없다고 가정한다. 철수는 창고에 보관된 토마토들이 며칠이 지나면 다 익게 되는지 그 최소 일수를 알고 싶어 한다.  

토마토를 창고에 보관하는 격자모양의 상자들의 크기와 익은 토마토들과 익지 않은 토마토들의 정보가 주어졌을 때, 며칠이 지나면 토마토들이 모두 익는지, 그 최소 일수를 구하는 프로그램을 작성하라. 단, 상자의 일부 칸에는 토마토가 들어있지 않을 수도 있다.  

**입력**  
첫 줄에는 상자의 크기를 나타내는 두 정수 M,N과 쌓아올려지는 상자의 수를 나타내는 H가 주어진다. M은 상자의 가로 칸의 수, N은 상자의 세로 칸의 수를 나타낸다. 단, 2 ≤ M ≤ 100, 2 ≤ N ≤ 100, 1 ≤ H ≤ 100 이다. 둘째 줄부터는 가장 밑의 상자부터 가장 위의 상자까지에 저장된 토마토들의 정보가 주어진다. 즉, 둘째 줄부터 N개의 줄에는 하나의 상자에 담긴 토마토의 정보가 주어진다. 각 줄에는 상자 가로줄에 들어있는 토마토들의 상태가 M개의 정수로 주어진다. 정수 1은 익은 토마토, 정수 0 은 익지 않은 토마토, 정수 -1은 토마토가 들어있지 않은 칸을 나타낸다. 이러한 N개의 줄이 H번 반복하여 주어진다.  

토마토가 하나 이상 있는 경우만 입력으로 주어진다.  

**출력**  
여러분은 토마토가 모두 익을 때까지 최소 며칠이 걸리는지를 계산해서 출력해야 한다. 만약, 저장될 때부터 모든 토마토가 익어있는 상태이면 0을 출력해야 하고, 토마토가 모두 익지는 못하는 상황이면 -1을 출력해야 한다.  


```
from collections import deque

m,n,h = map(int, input().split())
s = [[list(map(int, input().split())) for _ in range(n)] for _ in range(h)] # 지도그리기
v = [[[0]*m for _ in range(n)] for _ in range(h)] # 방문기록
dx = [-1, 1, 0, 0, 0, 0] # x축
dy = [0, 0, -1, 1, 0, 0] # y축
dz = [0, 0, 0, 0, -1, 1] # z축
q = deque() # 각자 따른곳에서 동시에 출발하기에 미리 deque를 해놓는다.

def bfs():
  while q:
    x,y,z = q.popleft()
    for i in range(6):
      a = x + dx[i]
      b = y + dy[i]
      c = z + dz[i]
      if 0<=a<n and 0<=b<m and 0<=c<h and s[c][a][b] == 0 and v[c][a][b] == 0:
        v[c][a][b] = 1
        s[c][a][b] = s[z][x][y] + 1
        q.append((a, b, c))

for k in range(h):
  for i in range(n):
    for j in range(m):
      if s[k][i][j] == 1:
        q.append((i, j, k)) # 바로 bfs를 하지 않는 이유는 1이 하나에서 출발하는게 아닌 여러곳부터 출발하기에 q에 s가 1인 값을 다 넣어준다.

bfs()
isTrue = True
max_num = 0
for k in range(h):
  for i in range(n):
    for j in range(m):
      if s[k][i][j] == 0:
        isTrue = False # 0값이 있는지 체크하는 것
      max_num = max(max_num, s[k][i][j]) # max(map(max, s))를 하면 type error다

print(max_num-1 if isTrue else -1)
```

위 코드는 다만 문제되는 것이 있다면 초기 append값에 v가 1로 처리되지 않았다는 것이 걸린다. 하지만 진행에 문제가 없다.

------
