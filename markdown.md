# MarkDown
---
## Start
> 문서 가장 첫 컬럼에서 #과 공백을 입력하고 바로 제목을 작성한다. (github에서 이렇게 해야 잘 보임.)  
> \# 제목

## Editor
MarkDown 온라인 에디터 추천  
[stackedit.io](http://stackedit.io) ; 무료, 구글드라이브, github 연동됨.  
[marxi.co](http://marxi.co) ; 조금 쓰면 바로 유료임.  

에디터/뷰어 마다 지원되는 문법과 출력 결과가 다를 수있다.  
github : 줄 바꿈시 마지막에 스페이스 두 개를 추가해줘야 한다. 수식 미지원   


## 구조적 타이틀
> \# 큰 제목  
> \## 중간 제목  
> \### 작은 제목  
> \#### 더 작은 제목  
> \##### 아주 작은 제목  
위에는 아래와 같이 보인다. **#이 많을수록 글자가 작아진다.** #은 1~6개까지만 지원한다.  
> # 큰 제목  
> ## 중간 제목  
> ### 작은 제목  
> #### 더 작은 제목  
> ##### 아주 작은 제목  

## 인용문 / 들여쓰기  
> 블록인용문자를 쓰면 지금 이 블록처럼 된다.  
> 앞에 시작을 >로 하면 된다.  
\> 이렇게 작성하면  
> 이렇게 된다.  
> 줄 바꿈이 안 될때는 **문장 마지막에 스페이스 "두 개"를** 추가한다.  
> 시작을 >> 두개로 하면  
>> 이렇게 인용문안에 인용문이 들어가게 된다.    
>>> 단계 추가.  
>> 계속 추가  
빠져 나오려면 빈 줄 추가.  

>> 이 단으로  

> 일 단 으로  

> 문장 줄 바꿈 테스트
그냥 엔터 친 것 (뷰어에 따라 줄이 안 바뀐다.)  
문장 줄 바꿈 테스트  
스페이스 두 개 추가하여 엔터 친 것.(확실히 줄 바뀜)  


## 목록
> 목록은 -과 공백을 앞에 쓴다. 글머리있는 목록은 \* 또는 숫자.를 사용한다.  
> 여기서 사용하는 특수 문자들은 원래 문자로 표현하기 위해서는 이스케이프 문자가 필요한데 \\를 사용한다.   
> 목록의 깊이를 표현하려면 **공백 두 개** 를 앞에 계속 추가한다.  
<pre>
- 하나
  - 둘
    - 셋
* 하나
  * 둘
    * 셋
</pre>
위와 같이 하면 아래와 같이 나온다.  
- 하나
  - 둘
    - 셋
* 하나
  * 둘
    * 셋

## 특수 문자 / 그대로
> 내부 스트링을 그대로 쓰려면 \<pre> ... \</pre> 태그를 쓴다. 또는 code 태그를 쓴다.  
> 그러나 실제로 해 보면 특수 문자가 들어가면 아래처럼 안된다.  
<pre>
  #include <a.h>
  int main() {
    return 0 ;
  }
</pre>

## 소스 코드
> \`\`\` 보통 이렇게 **백쿼트(백틱) 세개를** 쓰고 언어명을 쓴다. 언어는 css, html, javascript, bash, python, cpp, java, php 등등 여러가지를 지원한다. 코드 입력을 마치려면 다시 백쿼트 세 개를 입력한다.  내부에서 백쿼트 세개를 쓰려면 어떻게 해야 되나??? 내부에서 백틱을 세개 쓰면 전체를 감싸는 백틱을 네 개로 대신 쓰면 된다. (더 많이 쓰면 된다.)
````
```python
import numpy as np
print(np.random.randn(4))
```
````
위와 같이 쓰면 아래처럼 보인다.
```python
import numpy as np
print(np.random.randn(4))
```

```javascript
function func() {
  var v1 = 'hello world';
  return v1 ;
}
```
```
언어명을 입력안하고 쓰면???
<html> hello </html>
입력한 그대로 다 출력된다.
~!@#$%^&*() 특수 문자도 입력 가능하다.
```





## 수평선
---
<pre>
이렇게 --- 세개를 입력하거나 < hr / > 태그를 쓴다.
</pre>

## 꾸미기
> 강조를 하려면 별 또는 언더바로 문자열을 특수 문자로 감싼다.  
> 먼저 \*로 감싸면 *테스트* 가 된다. <em>기울임</em> 이렇게 em태그를 써도 된다.  
> \_로 감싸면 _테스트_ 이렇게 italic이 된다.  
> \*\* 두 개로 감싸면 **테스트** 이렇게 bold로 된다.  
> \__ 두 개로 감싸면 __테스트__ 이렇게 위와 같다. <strong>강조</strong> 이렇게 strong 태그를 써도 동일하다.  
> \~ 물결 한 개로 감싸면 ~테스트~ 이렇게 바닥에 붙음.  
> \~~ 물결 두 개로 감싸면 ~~테스트~~ 이렇게 취소선이 된다.  

## 이미지/링크
### ImageLink
> **참조 링크**  
> a 태그는 대괄호, 소괄호를 차례로 사용하여 이렇게 표현한다.  

> [GOOGLE](google url "설명")   설명부는 생략 가능  
> [GOOGLE](https://google.com)  

> 동일 문서 내의 위치 이동은 링크 주소에  #과 이동위치제목을 붙여쓴다.   
> ```[처음으로](#Start) ```   
> [처음으로](#Start)   
> ```[이미지/링크로](#이미지/링크) 한글은 지원안되는 듯 하다.```    
> [이미지/링크로](#이미지/링크)   
> ```[ImageLink로](#ImageLink) ```   
> [ImageLink로](#ImageLink)   
 
> **이미지** 는 링크 방식과 동일한데 앞에 느낌표만 추가한다.  (이미지 크기 조정은 안됨)  
> ``` ! [ text ] ( link ) ```  
> ![google](https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_92x30dp.png)  

> 이미지 크기 조절을 하려면 html 태그를 사용한다. < img width="100" height="100" > 이렇게 이미지 태그를 쓴다.  뷰어에 따라서 안 보일 수 도 있다.    
> <img src="https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_92x30dp.png" width="60"/>   

> **이미지와 링크** 를 합치려면 위 두 개를 같이 쓴다. 참조 링크 문법의 텍스트부에 이미지 문법을 넣으면 된다.    
> ```[![google](https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_92x30dp.png)](https://google.com) ```  
>  [![google](https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_92x30dp.png)](https://google.com)  
>


## 테이블 / 표
> 테이블은 직관적으로 그리면 된다.   
> 헤더와 셀의 구분 가로선은 --- 이렇게 세 개 이상을 입력해야 한다.  
> 아래에서 가장 왼쪽과 가장 오른쪽의 |는 생략 가능하다.  
> 셀 내의 text alignment는 붙이고 싶은 위치에 가로선 앞, 뒤에 :을 추가한다. 아래 예에서는 key의 스트링을 오른쪽 정렬을 하기 위해 오른쪽에 붙였다.  
<pre>
|Key|Description|
|---:|----|
| Esc| 셀모드 진입 (헤드가 파란색) |
| a | 위에 셀 추가  |
| b | 아래에 셀 추가  |
</pre>
위와 같이 하면 이렇게 출력된다.  

|Key|Description|
|---:|----|
| Esc| 셀모드 진입 (헤드가 파란색) |
| a | 위에 셀 추가  |
| b | 아래에 셀 추가  |


## 수학 기호 / 수식 입력
> (https://ko.wikipedia.org/wiki/%EC%9C%84%ED%82%A4%EB%B0%B1%EA%B3%BC:TeX_%EB%AC%B8%EB%B2%95) 이 사이트를 참고한다.  
> 수식 입력은 \$\$ 두 개로 감싼다. 문법은 위 링크를 참고한다.  
> 수식을 \$ 한 개로 감싸는 것은 한 줄로 표현할 때 한다. (inline)  
```
$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}$$
```
달러 두 개로 표현하면 $$x = {-b \pm \sqrt{b^2-4ac} \over 2a}$$ 이렇게 크게 가운데에서만 출력된다.  

한 줄로 표현하면 $x = {-b \pm \sqrt{b^2-4ac} \over 2a}$ 이렇게 문장과 어울리고 작아진다.  

### 기본 기호
> 분수, 루트 , 제곱 , 절대값, 로그, 삼각함수
```
(${1}\over{2}$) +  $\frac{1}{3}$  ; 분수. 잘못된 괄호
$\left( \frac{1}{2} \right)$  ; 큰 괄호
$ \sqrt{ \frac{a}{b}} $  ; 루트
$\sqrt[3]{x^3+y^3 \over 2}$  ; 세제곱근, 지수
```
(${1}\over{2}$) + $\frac{1}{3}$
$\left( \frac{1}{2} \right)$
$ \sqrt{ \frac{a}{b}} $
$\sqrt[3]{x^3+y^3 \over 2}$

```
$\left\vert s \right\vert$  ; 절대값
$\lVert z \rVert$ ; 놈
$ 5^6 + \ln{r} + \log_2{10} $ ; 지수 로그
$ \sin {a} + \cos{b} $ ; 삼각함수
$$ \underset{y}{\overset{x}{_{c}^{a}Z_{d}^{b}}}	 $$
```

$\left\vert s \right\vert$
$\lVert z \rVert$
$ 5^6 + \ln{r} + \log_2{10} $
$ \sin {a} + \cos{b} $
$$ \underset{y}{\overset{x}{_{c}^{a}Z_{d}^{b}}}	 $$

> 문자 기타 기호
```
$ \pi+ \delta +\phi +\lambda $ $ \pm $
$\cap$ $\cup$  ; 집합 연산
$\approx$
${n \choose k}$
$ \Rightarrow $
$\rightarrow $

```
$ \pi+ \delta +\phi +\lambda $  $\pm$
$\cap$ $\cup$
$\approx$
${n \choose k}$
$ \Rightarrow $
$\rightarrow $


> 시그마 Sum, PI 곱
```
$ \sum_{i=1}^{10}{i}  $
$\prod_{i=1}^N x_i $
```
$ \sum_{i=1}^{10}{i}  $
$\prod_{i=1}^N x_i $

> 방정식
```
\begin{matrix}
f(n+1) &=& (n+1)^2 \\
       &=& n^2 + 2n + 1
\end{matrix}
```
$$
\begin{matrix}
f(n+1) &=& (n+1)^2 \\
       &=& n^2 + 2n + 1
\end{matrix}
$$




> 미분 / 적분
```
$$\lim_{n \to \infty}x_n$$ ; 달러 두개로 써야 한다. 인라인으로 하면 안 이쁨.
${dy \over dx}$ ; 이게 가장 심플하다  
${\operatorname{d}\!y\over\operatorname{d}\!x}$
${\partial^2\over\partial x_1\partial x_2}y$
$\int_{-N}^{N} e^x\, dx$
$$\int_{-N}^{N} e^x\, dx$$
```
$$\lim_{n \to \infty}x_n$$
${dy \over dx}$
${\operatorname{d}\!y\over\operatorname{d}\!x}$  
${\partial^2\over\partial x_1\partial x_2}y$
$\int_{-N}^{N} e^x\, dx$
$$\int_{-N}^{N} e^x\, dx$$

> 수식내에 \\ 기호가 있는 경우 \\\\ 이렇게 더블로 써줘야 한다.

```
$$
\begin{pmatrix}
x & y \\
 z & v
 \end{pmatrix}
$$
$$\begin{vmatrix} x & y \\ z & v \end{vmatrix}$$
```
$$
\begin{pmatrix}
x & y \\
 z & v
 \end{pmatrix}
$$

$$\begin{vmatrix} x & y \\ z & v \end{vmatrix}$$


```
$$
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\
\end{vmatrix}
$$
```
$$
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\
\end{vmatrix}
$$


<style= "color:red">
style tag:  hello 
</style>

<font color="red">
font tag color change: hello
</font>





<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ5NTgxODU3LC0zMjc1NDYyNDIsLTcyNz
Y0MTUyMCwxNTAxODgyNjYxXX0=
-->