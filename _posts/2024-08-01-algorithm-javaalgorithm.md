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

### 예제
```java
package study.blog.codingnojam;

import java.util.LinkedList;
import java.util.Queue;

public class StudyBFS {
	
	public static void main(String[] args) {
		
		// 그래프를 2차원 배열로 표현해줍니다.
		// 배열의 인덱스를 노드와 매칭시켜서 사용하기 위해 인덱스 0은 아무것도 저장하지 않습니다.
		// 1번인덱스는 1번노드를 뜻하고 노드의 배열의 값은 연결된 노드들입니다.
		int[][] graph = {{}, {2,3,8}, {1,6,8}, {1,5}, {5,7}, {3,4,7}, {2}, {4,5}, {1,2}};
		
		// 방문처리를 위한 boolean배열 선언
		boolean[] visited = new boolean[9];
		
		System.out.println(bfs(1, graph, visited));
		//출력 내용 : 1 -> 2 -> 3 -> 8 -> 6 -> 5 -> 4 -> 7 -> 
	}
	
	static String bfs(int start, int[][] graph, boolean[] visited) {
		// 탐색 순서를 출력하기 위한 용도
		StringBuilder sb = new StringBuilder();
		// BFS에 사용할 큐를 생성해줍니다.
		Queue<Integer> q = new LinkedList<Integer>();
		 
		// 큐에 BFS를 시작 할 노드 번호를 넣어줍니다.
		q.offer(start);
		
		// 시작노드 방문처리
		visited[start] = true;
		
		// 큐가 빌 때까지 반복
		while(!q.isEmpty()) {
			int nodeIndex = q.poll();
			sb.append(nodeIndex + " -> ");
			//큐에서 꺼낸 노드와 연결된 노드들 체크
			for(int i=0; i<graph[nodeIndex].length; i++) {
				int temp = graph[nodeIndex][i];
				// 방문하지 않았으면 방문처리 후 큐에 넣기
				if(!visited[temp]) {
					visited[temp] = true;
					q.offer(temp);
				}
			}
		}
		// 탐색순서 리턴
		return sb.toString() ;
	}
}
```