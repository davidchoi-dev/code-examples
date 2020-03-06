# 함수 스코프에 의한 **var**의 문제점

영상에서 다 다루지 못했지만, let이 만들어지게 된 중요한 이유 중 하나를  
두 예시 코드를 통해 설명드릴게요.  

ES6 이전의 자바스크립트를 다루다 보면, 웬만한 다른 언어들에서는 마주치기 힘든  
*정말 독특한 문제*를 마주치게 될 때가 있어요.  

아래 코드를 살펴보죠.  

```javascript
function countWithLet () {

  //0에서 4까지 for loop을 돌면서
  for (let i = 0; i < 5; i++) {

    //'1초 뒤 i를 출력하라'는 지시를 내린다.
    setTimeout(function () {

      //i를 출력
      console.log(i)
    }, 1000)
  }
}

countWithLet(); //위의 함수를 호출
```

이 함수를 실행하면 어떤 결과가 나올까요?  
예상하기 어렵지 않죠.  1초 뒤에  

```
0
1
2
3
4
```

위와 같은 결과물이 한 번에  나올거에요.  
매우 상식적이고 정상적인 결과죠.  

![playcode.io](https://raw.githubusercontent.com/junseol86/yalcodic/master/26_scope/js_code/playcode.png?token=AB5VY2SMJ2A4GI32MWJVPI26MBDOS)

<a href="https://playcode.io/" target="_blank">playcode.io</a>에 방문하면 위 캡쳐화면에서처럼 간단한 자바스크립트 코드들을 붙여넣어서 바로 실행할 수 있어요!  

```
'1초 간격'으로 0, 1, 2... 이렇게 나오는 게 아니에요.
for문은 한 번에 '쫘라락' 돌고 그 순간으로부터 1초를 재는거니까
함수 호출 뒤 1초 후에 한꺼번에 '쫘라락' 나오는거죠.
```

### 그런데 이건 **let**을 사용했기 때문에 볼 수 있는 모습이에요.  

***

**var**를 사용하면 뭐가 어떻게 달라지냐구요?  

```javascript
function countWithVar () {

  //0에서 4까지 for loop을 돌면서
  for (var i = 0; i < 5; i++) {

    //'1초 뒤 i를 출력하라'는 지시를 내린다.
    setTimeout(function () {

      //i를 출력
      console.log(i)
    }, 1000)
  }
}

countWithVar();
```

직접 코딩을 해보고 결과를 보죠.  

```
5
5
5
5
5
```

### ?????

이건 무슨...  **어째서** 이런 결과가 나왔을까요?  

자, 영상에서 말씀드렸던 **var**의 특성 중  
```
함수 스코프를 갖는다
```
기억하시죠?

**let**으로 선언된 i는 여느 언어의 상/변수들이 그렇듯 for문으로 지정된 블록 영역, 즉
```javascript
  for (let i = 0; i < 5; i++) {
    ...
    // 바로 이 이 영역
    ...
  }
```
주석으로 나타낸 위의 활동영역을 갖게 되죠.

반면 **함수 스코프**를 갖는 **var** 변수 i는

```javascript
function countWithVar () {
  // 여기부터~
  for (var i = 0; i < 5; i++) {
    ...
    ...
  }
  // 여기까지!!
}
```
주석으로 표시한 위의 공간에서 활동하게 되는거에요.

그러니까 **var**를 사용했던 위의 코드는 사실상

```javascript
function countWithVar () {

  var i; // ◀ ◀ ◀︎︎︎︎︎︎︎︎ i가 여기 선언된 것과 같아요.

  //0에서 4까지 for loop을 돌면서
  for (i = 0; i < 5; i++) {

    //'1초 뒤 i를 출력하라'는 지시를 내린다.
    setTimeout(function () {

      //i를 출력
      console.log(i)
    }, 1000)
  }
}

countWithVar();
```
위의 코드와 다름없게 되어버린거죠!!!

for문이 돌면서 i는 5까지 증가되어 있을거에요.  

그 1초 뒤부터 연속으로 실행될 console.log들은  
**let**을 썼을때처럼 for문 블록 안에 있는,  
즉 루프가 돌 때마다 주어진 각자만의 i를 출력하는 게 아니라  

함수 내에 공유되는 i,
이미 5가 되어버린 i를 다 같이 출력하게 되는거에요.

<br>

### 알~겠나요?

<br>

이를 이해하고 나면 앞으로 웹을 개발할 때 마주칠 이러한 문제들이 어째서 발생하는건지,  
그리고 Node.js 등에서는 왜 **var** 대신 **let**을 사용하는것이 권장되는지 알 수 있을거에요.

설명이 부족했던 부분이나 잘못된 점이 있다면 댓글 등으로 알려주시면 반영하도록 하겠습니다.

### 즐코하세요!!
