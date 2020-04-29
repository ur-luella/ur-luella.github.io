---
layout: post
title: "Jekyll을 이용한 github 블로그 생성"
excerpt: "간단한 방법으로 블로그 만들기"
categories: [Blog]
tags: [jekyll]
comments: true
---

코드 관리도 하고 싶고 블로그도 하고 싶을 때 둘다 관리할 수 있는 방법으로 github의 블로그를 만드는 것이 아닐까 싶다. 간단하게 만들 수 있겠지하고 우습게 봤다가 git 공부하고 html 공부하고 markdown 공부하고..

블로그를 만들기 위해 찾다보니 아주 유용한 Jekyll을 알게 되었다. 테마도 예쁜게 많고 잘 만든 테마를 수정할 수 있다는 장점이 아주 매력적이다. 아직은 서툴지만 하나하나씩 차근차근 더 깊게 공부하려 한다.

## Git과 Jekyll 설치
* Git 설치 : Git 설치는 [이곳](https://git-scm.com/)에서 Git에 대한 간단한 설명은 [이곳](https://backlog.com/git-tutorial/kr/)에서 확인할 수 있다.
* Jekyll 설치: 터미널에 아래와 같이 입력한다.

		{% raw %}
		sudo gem install jekyll
		{% endraw %}


## Jekyll 테마 고르기
이 부분에서 시간을 가장 많이 쏟았던 것 같다. 테마를 골라서 사용하다가 마음에 들지않아서 갈아엎고 다시 만들었다. Jekyll 테마는 [이곳](https://github.com/topics/jekyll-theme)에서 골라도 되고 구글링을 통해서도 마음에 드는 테마를 찾을 수 있다. 나는 enyuanz의 [leonids  테마](https://renyuanz.github.io/leonids)로 만들었다. 블로그이 용도에 따라서도 테마를 다르게 선택할 수 있다. 블로그의 주 용도를 논문내용이나 프로젝트의 생각들을 정리하기 위해서 카테고리로 나눌 수 있는 블로그로 선택하였다.

![leonids](https://user-images.githubusercontent.com/41414127/60320467-c46ab000-99b4-11e9-84a2-df22f5f0cd31.png)

테마를 고른 후 해당 테마의 github repository에 가면 오른쪽 위에 fork가 있다. 누르면 자신의 github에 repository 그대로 복사되는데 그 상태에서 setting에 들어가 repository 이름을 수정하면 된다. 보통 github아이디 + .github.io를 붙인다.

![fork](https://user-images.githubusercontent.com/41414127/60320484-d0567200-99b4-11e9-9d87-3b441e0eb89c.png)

## 원격 저장소 만들고 파일 수정하기
로컬에서 수정하기 위해 맥에 블로그용 폴더를 하나 만들 곳을 지정한 후 터미널을 열어 해당 폴더로 위치를 변경한 후 git을 통해 연결한다. 나같은 경우 블로그용 파일을 저장하는 폴더 이름이 ur-luella이다. 후에 글을 쓰거나 로컬에 있는 파일들을 수정할때 사용하게 되는 code들이 git add . / git commit -m " " / git push origin master이다. 순서대로 설명하자면, 로컬 변경사항을 git에게 알리기 / 변경사항에 대한 설명을 "설명"에 적어 커밋하기 / github에 push하여 내용 변경하기로 이해할 수 있다.

	{% raw %}
	git clone http://github.com/ur-luella/ur-luella.github.io.git #폴더이름 변경함 -> ur-luella
	cd ur-luella # 폴더로 이동
	git init 
	git remote add origin http://github.com/ur-luella.github.io.git
	git add .
	git commit -m "저장소 연결"
	git push orgin master
	{% endraw %}

수정해야하는 폴더, 파일은 테마마다 다르기 때문에 참고용으로만 볼 것. 내 테마의 경우는 아래와 같다.
* config.yml: 블로그의 구성 파일
* posts: post(글)들을 저장하는 폴더
* layouts: 페이지의 레이아웃을 저장하는 폴더(post, home 등)


파일을 수정할 땐 Sublime Text를 사용하였다. 먼저, config 파일의 정보를 변경한다. 이름과 이메일 등의 정보를 수정하고 favicon을 수정한다. 이미지를 넣고 싶을 경우 github의 issue 탭에 이미지를 업로드하였을 때 이미지의 주소가 새롭게 생겨난다. 그 주소를 복사하여 붙이면 된다.

두번째로는 글을 쓰기 위해 post폴더에 들어간다. leonids 테마는 YYYY-MM-DD-postname으로 형식이 되어있고 대부분 테마마다 예시 post가 있어 참고하면 된다. 코드를 입력하고 프로그래밍 언어를 입력하면 자동으로 하이라이트를 표시해준다는 것도 굉장한 장점인 듯 하다. 

{% highlight python %}
import numpy as np
a = np.arange(20).reshape([4,5])
print(a)
def test(num):
	return num
{% endhighlight %}


## 로컬 변경 파일 push하기
로컬에서 파일을 변경하고 블로그에 변경된 내용을 반영하기 위해 터미널에서 코드를 입력한다. 먼저 블로그 파일들을 저장한 폴더로 변경한 후 로컬 파일 변경된 것을 git으로 체크한 후 push한다. 블로그에 업데이트가 되었는지 확인하면 끝! 블로그가 업데이트 되는데는 다소 시간이 걸릴 때가 있다.

	{% raw %}
	cd ur-luella
	git add .
	git commit -m "새글 추가" # "" 안에 수정된 내용 작성
	git push orgin master 
	{% endraw %}


## 마치며
Jekyll 테마를 이용한 github 블로그를 만들어보았다. 블로그를 좋아하는데 마음에 드는 블로그 사이트가 없어서 이리저리 방황했었는데 직접 테마부터 구성까지 수정할 수 있는 github 블로그를 만드니 이제 다른 블로그는 안 만들어도 될 것 같다. 또, 코드를 작성하여 블로그를 수정한다는 게 번거로울 수는 있지만 나만의 블로그가 만들어진다고 생각하니 좋은 점이 더 많다. 

