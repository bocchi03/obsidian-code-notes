# n 皇后问题
```c++
// 暴力搜索
int dfs(int i){
	if(i == 1 || i == 2)
		return i;
	int count = dfs(i - 1) + dfs(i - 2);
	return count;
	
}

int climbStairDFS(int n)// 
	return dfs(n);
```

```cpp
// 记忆化搜索
int dfs(int i, vecotr<int>& mem){
	if(i == 1 || i == 2)
		return i;
	if(mem[i] != -1)
		return mem[i];
	int count = dfs(i - 1, mem) + dfs(i - 2, mem);
	mem[i] = count;
	return count;

	int climbStairDFS(int n){
		vector<int> mem(n + 1, -1);
		return dfs(n, mem);
	}
	int cl
}
```
```cpp
// 动态规划
int climbStairDFS(int n){
	if(n == 1 || n == 2)
		return n;
	vector<int> dp(n + 1);
	dp[1] = 1;
	dp[2] = 2;
	for(int i = 3; i <= n; i++)
		dp[i] = dp[i - 1] + dp[i - 2];
	return dp[n];
}</mark>

```

```c++
int main(){
	


}
```

```c++
int main(){
	



}
```

```c++
int m
```

```c++
int main(){


}
```


```c++
int main()

}
```


```c++
int main(){
	


}
```

```c++
int main()
fsdffdaaasf
```