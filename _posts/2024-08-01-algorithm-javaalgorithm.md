---
title: "JAVA로 알고리즘"
date: 2024-08-01 00:00:00 +0800
categories: [algorithm, java]
tags: [알고리즘]
math: true
render_with_liquid: false
---


JAVA로 직접 알고리즘을 구현하면서 공부


### DFS
- 깊이 우선 탐색
- 자기 자신을 호출하는 순환 알고리즘의 형태

## 예제
BOJ - 3184 [양](https://www.acmicpc.net/problem/3184)

그래프를 탐색하여 같은 구역내에 양이 많으면 양만 카운트, 같거나 늑대가 많으면 늑대만 카운트 해준다.

```java
import java.io.*;
import java.util.*;

public class Main {

    static int n, m;
    static String[] graph;
    static boolean[][] visited;
    static int[] dx = {-1, 0, 1, 0};
    static int[] dy = {0, -1, 0, 1};
    static int o, v;

    static void dfs(int x, int y) {
        visited[x][y] = true;
        for (int k = 0; k < 4; k++) {
            int xx = x + dx[k];
            int yy = y + dy[k];
            if (xx < 0 || xx >= n || yy < 0 || yy >= m) continue;
            if (visited[xx][yy] || graph[xx].charAt(yy) == '#') continue;
            if (graph[xx].charAt(yy) == 'o') o++;
            if (graph[xx].charAt(yy) == 'v') v++;
            dfs(xx, yy);
        }
    }

    public static void main(String[] args) throws Exception {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer tr = new StringTokenizer(br.readLine());
        n = Integer.parseInt(tr.nextToken());
        m = Integer.parseInt(tr.nextToken());

        graph = new String[n];
        visited = new boolean[n][m];

        for (int i = 0; i < n; i++) {
            graph[i] = br.readLine();
        }

        int ans[] = {0, 0};
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (graph[i].charAt(j) == 'o' && !visited[i][j]) {
                    o = 1;
                    v = 0;
                    dfs(i, j);
                    if (o > v) {
                        ans[0] += o;
                    } else {
                        ans[1] += v;
                    }
                } else if (graph[i].charAt(j) == 'v' && !visited[i][j]) {
                    o = 0;
                    v = 1;
                    dfs(i, j);
                    if (o > v) {
                        ans[0] += o;
                    } else {
                        ans[1] += v;
                    }
                }
            }
        }

        System.out.println(ans[0] + " " + ans[1]);

    }
}
```
알고리즘 자체는 이미 알고 있지만 변수를 선언하거나 사용하는 부분에서 아직 낯선 부분이 많다. 

## BFS
- 너비 우선 탐색
- 큐를 이용한 선입선출 형태

### 예제
BOJ - 17836 [공주님을 구해라!](https://www.acmicpc.net/problem/17836)


```java
import java.io.*;
import java.util.*;

public class Main {
    static int n, m, t;
    static int[][] graph;
    static boolean[][][] visited;
    static int[] dx = {-1, 0, 1, 0};
    static int[] dy = {0, 1, 0, -1};

    public static class Node {
        int x, y, dist;
        boolean isGram;

        public Node(int x, int y, int dist, boolean isGram) {
            this.x = x;
            this.y = y;
            this.dist = dist;
            this.isGram = isGram;
        }
    }

    public static int bfs(int x, int y) {
        Queue<Node> q = new LinkedList<>();
        q.offer(new Node(x, y, 0, false));
        visited[x][y][0] = true;

        while(!q.isEmpty()) {
            Node nNode = q.poll();
            if (nNode.dist > t) break;
            if (nNode.x == n - 1 && nNode.y == m - 1) return nNode.dist;

            for (int i = 0; i < 4; i++) {
                int xx = nNode.x + dx[i];
                int yy = nNode.y + dy[i];
                if (xx < 0 || xx >= n || yy < 0 || yy >= m) continue;
                if (!nNode.isGram) {
                    if (!visited[xx][yy][0] && graph[xx][yy] == 0) {
                        q.offer(new Node(xx, yy, nNode.dist + 1, nNode.isGram));
                        visited[xx][yy][0] = true;
                    } else if (!visited[xx][yy][0] && graph[xx][yy] == 2) {
                        q.offer(new Node(xx, yy, nNode.dist + 1, true));
                        visited[xx][yy][0] = true;
                    }
                } else {
                    if (!visited[xx][yy][1]) {
                        q.offer(new Node(xx, yy, nNode.dist + 1, nNode.isGram));
                        visited[xx][yy][1] = true;
                    }
                }
            }
        }
        return -1;
    }

    public static void main(String[] args) throws Exception {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());
        t = Integer.parseInt(st.nextToken());

        graph = new int[n][m];
        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < m; j++) {
                graph[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        visited = new boolean[n][m][2];
        int answer = bfs(0, 0);
        if (answer == -1) System.out.println("Fail");
        else System.out.println(answer);

    }

}
```

bfs도 알고리즘은 알고 있지만 클래스형태로 노드를 q에 넣는 방식이 생소했다.