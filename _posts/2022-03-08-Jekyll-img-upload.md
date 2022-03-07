---
layout: post
title:  "Jekyll 이미지 업로드 이슈 with 삽질"
date:   2022-03-08 23:09:00 +0900
categories: jekyll update
---

## Github 잔디 포스팅에서 일어난 이슈 

어제 private 잔디 심는 방법에 대한 글을 업로드 하였다.  
그런데 말입니다? 이미지가 다 깨져서 나오는 겁니다!😱  

![img.png](https://github.com/skj2366/skj2366.github.io/blob/main/_posts/Jekyll%20%EC%9D%B4%EB%AF%B8%EC%A7%80%20%EC%97%86%EC%9D%8C.png?raw=true) 
>이미지가 깨진 모습



하지만 VSCode 혹은 WebStorm으로 마크다운 파일을 열어보면 이미지가 잘 표출 되고 있다.


![img.png](https://github.com/skj2366/skj2366.github.io/blob/main/_posts/Jekyll%20IDE%20md미리보기%20잘%20나옴.png?raw=true)
>이미지가 IDE 마크다운 미리보기에서 잘 나오는 모습

아차.. 이럴수가 이미지가 깨져나오면 경로가 잘못된 것이거늘!  
기초적인 부분을 간과하고 그만 푸쉬를 해버려서 나타난 결과이다. 

---

## 삽질의 시작 

우선 급한대로 상대경로로 표기하기 위하여 https://github.com/skj2366/skj2366.github.io/blob/main/_posts/를 붙여주었다.
![임시조치.png](https://github.com/skj2366/skj2366.github.io/blob/main/_posts/Jekyll%20%EC%9D%B4%EB%AF%B8%EC%A7%80%20%EA%B2%BD%EB%A1%9C%20%EC%A1%B0%EC%B9%98.png?raw=true)
>상대경로 임시조치 모습


하지만 여전히 날 괴롭히는 이미지를 발견할 수 있었다 😭 
![머_멈춰.png](https://github.com/skj2366/skj2366.github.io/blob/main/_posts/%EC%97%AC%EC%A0%84%ED%9E%88%20%EB%82%A0%20%EA%B4%B4%EB%A1%AD%ED%9E%88%EB%8A%94%20%EC%9D%B4%EB%AF%B8%EC%A7%80.png?raw=true)
> 멈춰! 엑박 멈춰!!

  
  다음 방법을 찾기로 한다.  
  절대경로를 사용하자! 
  ![절대경로.png](https://github.com/skj2366/skj2366.github.io/blob/main/_posts/절대경로%20사용.png?raw=true)

결과는 바로 실패하여 Fail.

결국 삽질을 하다가 절대 실패할 수 없는 깃헙 리포지토리에 올라가는 이미지의 이미지 주소를 임시로 사용하기로 마음먹었다.

![깃허브 이미지 url](https://github.com/skj2366/skj2366.github.io/blob/main/_posts/%EA%B9%83%ED%97%88%EB%B8%8C%20%EC%9D%B4%EB%AF%B8%EC%A7%80%20url%20%EC%82%AC%EC%9A%A9.png)


대망의 업로드 & 테스트
![업로드완료](https://github.com/skj2366/skj2366.github.io/blob/main/_posts/%EC%9D%B4%EA%B1%B4%20%EC%95%88%EB%93%A4%EC%96%B4%EA%B0%88%EC%88%98%EA%B0%80%20%EC%97%86%EC%A7%80.png)
> 솔직히 이건 안들어갈 수가 없지 ..

드디어 성공했다 ! 

사실 원인도 알고 있었고 (프로젝트 명이 도메인과 동일해서 경로 상실) 
검색을 통하여 쉬운 방법을 찾는 방법도 있었지만 삽질로 해결하면 그만큼 기억에 잘남는다고 하지 않았는가? (는 핑계, 사실 빨리하고 얼른 자고 출근하려고 했다. 더 오래걸린건 함정)

이에 삽질을 한번 해보았다. 검색해 보니 편한 방법이 많더라
> 삽질 멈춰! 

마무리로 현재 상황에 생기는 문제점을 생각해보면
1. github repository에 이미지를 그대로 저장 
   1. 1GB 용량 제한이 무섭지 않더냐?
   2. 유료 repository를 사용하면 문제 해결 
2. _posts 경로 자체에 그냥 이미지 업로드
   1. 글이 몇개 없는데도 벌써 정신없다.
3. 깃허브 이미지 주소를 사용하기에는 글쓸때 마다 노가다가 심하다 
   1. 이미지 업로드 할 때마다 경로 복붙 해야한다.
   2. 자동화가 안된다.

현재 이 정도로 보이는데 종합해서 해결을 해볼 생각이다.
> ? 블로그 테마부터... 아니 세팅하고 아무것도 안바꿨잖아 ...