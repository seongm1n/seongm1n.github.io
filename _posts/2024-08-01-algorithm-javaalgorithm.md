---
title: "JAVA로 알고리즘 구현해보기"
date: 2024-08-01 00:00:00 +0800
categories: [algorithm, java]
tags: [알고리즘]
math: true
render_with_liquid: false
---


JAVA로 직접 알고리즘을 구현하면서 공부


### DFS
- 깊이 우선 탐색

## 예제
```java
package study.blog.codingnojam;

public class Study_DFS_Recursion {

	// 방문처리에 사용 할 배열선언
	static boolean[] vistied = new boolean[9];
	
	// 그림예시 그래프의 연결상태를 2차원 배열로 표현
	// 인덱스가 각각의 노드번호가 될 수 있게 0번인덱스는 아무것도 없는 상태라고 생각하시면됩니다.
	static int[][] graph = {{}, {2,3,8}, {1,6,8}, {1,5}, {5,7}, {3,4,7}, {2}, {4,5}, {1,2}};
	
	public static void main(String[] args) {
		dfs(1);
	}
	
	static void dfs(int nodeIndex) {
		// 방문 처리
		vistied[nodeIndex] = true;
		
		// 방문 노드 출력
		System.out.print(nodeIndex + " -> ");
		
		// 방문한 노드에 인접한 노드 찾기
		for (int node : graph[nodeIndex]) {
			// 인접한 노드가 방문한 적이 없다면 DFS 수행
			if(!vistied[node]) {
				dfs(node);
			}
		}
	}
}
```