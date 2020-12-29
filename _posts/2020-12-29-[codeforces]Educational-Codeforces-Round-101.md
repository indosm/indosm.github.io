---
title: "[codeforces]Educational Codeforces Round 101"
excerpt_separator: "<!--more-->"
categories:
  - review
tags:
  - codeforces
  - Programming
---

**A. Regular Bracket Sequence**

'(', ')', '?' 세개의 문자로 구성된 문장이 올바른 괄호짝으로 구성된 문장인지 여부를 판단하는 문제.

맨 처음에는 그냥 단순하게 '?'를 '('로 치환한 문장과, ')'로 치환한 문장 2가지로 경우를 나누면서 DFS를 돌렸는데,
문제 조건상 '(', ')'는 각각 한단어만 나오고, 나머지 모든 단어는 '?'로만 구성되어있어 TLE가 떴다.

그래서 그냥 단순하게 절대로 괄호짝이 나올수 없는 경우를 제외하고 나머지를 가능한 경우로 판단하는 방법으로 solve.
```yaml
string s;
scanf("%s",&s);
if(strlen(s)%2==1){
  printf("NO\n");
}
else if(s[0]==')'){
  printf("NO\n");
}
else if(s[strlen(s)-1]=='('){
  printf("NO\n");
}
else {
  printf("YES\n");
}
```

**B. Red and Blue**

입력을 Red와 Blue를 나누어서 각각 list를 구성하고, 최대부분합을 구하는데, 이때 부분합의 조건이 앞에서부터 순차적으로 원소를 포함해야되는 문제.
```yaml
int n,m;
int r[110], b[110];
int max_red=0, max_blue=0;
int tmp_red=0, tmp_blue=0;
scanf("%d",&n);
for(int i=0;i<n;i++){
  scanf("%d",&r[i]);
  tmp_red +=r[i];
  if(tmp_red>max_red)max_red = tmp_red;
}
scanf("%d",&m);
for(int i=0;i<m;i++){
  scanf("%d",&b[i]);
  tmp_blue += b[i];
  if(tmp_blue>max_blue)max_blue = tmp_blue;
}
printf("%d\n",max_red+max_blue);
```

**C. Building a Fence**

문제에서 요구하는 조건 하에 울타리를 알맞게 배열할 수 있는지 여부를 출력하는 문제.

A에서와 마찬가지로, 놓을 수 있는 모든 경우를 DFS하게 되면 TLE가 뜨게 되므로, 수학적으로 놓을수 있는 최소높이와 최대높이를 저장해서,
울타리 길이인 n번만큼 DP로 탐색해가는 문제.
```yaml
int solve(int now, int end, int k, int height[200020], int minheight, int maxheight){
	if(now==end){
		if(minheight==height[now])
			return 1; //return yes
		else
			return 0;
	}
	if(minheight>height[now]+k-1){
		return 0;
	}
	if(minheight>maxheight){
		return 0;
	}
	int minh,maxh;
	minh = max(minheight+1-k, height[now+1]);
	maxh = min(maxheight+k-1, height[now+1]+k-1);
	return solve(now+1,end,k,height,minh,maxh);
}
```

**D. Ceil Divisions**

주어진 1~n까지의 숫자를 나눗셈을 이용해서 n-1개의 1과, 1개의 2로 만드는 문제.

맨 처음에는 단순하게 3~n-1까지의 숫자를 n으로 나누어 1로 만들고, n은 1이 될때까지 2로 나누는 것을 생각했는데,
주어진 n의 범위인 2*10^5의 경우 2로 나눠서 1이 될려면 문제 조건인 n+5번을 넘어버리게 된다.

그래서 n을 최대한 빠르게 1까지 나눠지게끔 하는 수를 고르기 위해서, sqrt(n)을 이용하면 n/sqrt(n)은 sqrt(n)이 되고, 다시 그 sqrt(n)을 sqrt(n)으로 나누면 1이 되게끔, 즉 2번의 operation만으로 1이 되므로, 이러한 root값을 중심으로 반복횟수를 줄이게 되었다.
```yaml
int n;
scanf("%d",&n);
int root[10];
int tmp=n;
int num=1;
root[0]=n;
while(tmp!=2){
  tmp = (int)ceil(sqrt(tmp));  //root변수에는 n, sqrt(n), sqrt(sqrt(n)), ... 이런식으로 저장됨.
  root[num++]=tmp;
}
num--;
printf("%d\n",(n-1-2)-(num-1)+2*num);
for(int i=3;i<=n-1;i++){
  int signal=0;
  for(int j=0;j<num;j++){
    if(root[j]==i){
      signal=1;
      break;
    }
  }
  if(signal==1){
    ;
  }
  else{
    printf("%d %d\n",i,n);   //root변수를 제외한 나머지 수는 n으로 나눠서 1로 변환.
  }
}
for(int i=0;i<num;i++){
  printf("%d %d\n",root[i],root[i+1]);  //n을 sqrt(n)으로 나누는 행위를 2번씩 하면 1로 변환됨.
  printf("%d %d\n",root[i],root[i+1]);  //이를 통해 2를 제외한 나머지 모든 수를 n+5번 안에 1로 변환.
}
```

**Info Notice:** E,F문제 다른사람 정답 보면서 복습하고 정리할것.
{: .notice--info}
