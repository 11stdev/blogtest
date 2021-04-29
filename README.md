# 11STREET 기술 블로그

11번가 기술 블로그는 GitHub Pages와 Jekyll을 사용되었으며, 마크다운 문법을 사용하여 글을 작성합니다.


## 목차

* **환경구성**
	1. [프로젝트 Clone](#프로젝트-clone)
	2. [Jekyll 설치](#jekyll-설치)
	3. [로컬에서 블로그 실행하기](#로컬에서-블로그-실행하기) 

* **게시글 작성**
	1. [새 글 작성](#게시글-작성)
	2. [작성자 신규등록](#작성자-신규등록)
	3. [태그 신규등록](#태그-신규등록)



## 환경구성

### 프로젝트 Clone
	$ git clone http://bitbucket.11stcorp.com/scm/dev11st/11stdev.github.io.git
	
### Jekyll 설치
> https://wiki.11stcorp.com/pages/viewpage.action?pageId=364543247

	# MacOS 기준이며, M1 Apple Silicon 기반 추가설명 포함
	# M1 맥의 경우 - Rosetta를 이용하여 Terminal 프로그램을 오픈합니다. (Intel 기반으로 동작)
	# Native 확장기능 컴파일용 명령행 확장 도구 설치
	$ xcode-select --install

	# Homebrew설치 (반드시 Intel 기반의 Homebrew를 설치해야 합니다. - M1 사용자 주의)
	$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

	# Ruby 설치 (2.6.6, x86 버전 설치) - 일반 MacOS는 Ruby 2.6.3을 포함하고 있으므로 설치하지 않고 Pass해도 됩니다. (rvm -> ruby -> jekyll 설치)
	$ \curl -sSL https://get.rvm.io | bash -s stable
	$ rvm install ruby-2.6.6
	$ rvm use ruby-2.6.6
	$ rvm --default use 2.6.6

	# ruby 는 x86_64로 나와야 합니다.
	$ ruby -v
	ruby 2.6.6p146 (2020-03-31 revision 67876) [x86_64-darwin20]

	# jekyll, bundler 설치
	$ gem install jekyll bundler

### 로컬에서 블로그 실행하기

	# 프로젝트 경로로 이동
	$ cd 11stdev.github.io
	# 패키지 설치
	$ bundle install
	# 블로그 서버 실행
	$ jekyll serve (또는 bundle exec jekyll serve)
	# 브라우저 실행
	$ open http://localhost:4000

## 게시글 작성

### 새 글 작성 (초안)
1.  **`_drafts`** 디렉토리에 **`적당한이름.md`** 이름으로 파일을 만들고
2.  게시글 내용 마크다운으로 작성
	-   https://stackedit.io/ 활용 등
3. 로컬 확인
```bash
$ jekyll serve --drafts
```

### 새 글 작성 (발행)
1.  `_posts` 디렉토리에 `[yyyy-mm-dd-slug.md](http://yyyy-mm-dd-slug.md/)` 파일로 복사(or 이동).
    -   slug: 해당 포스트의 고유 키로 url의 일부로 사용. 왠만하면 특수문자없이 영문자,숫자,-(하이픈),.(점)...만 사용.
    -   yyyy,mm,dd: 발행 년,월,일.
    -   참고: 포스트의 URL(permalink):  [https://11stdev.github.io](https://11stdev.github.io/)/yyyy/dd/dd/slug
2.  파일 상단에 [front matter](https://jekyllrb.com/docs/frontmatter/) 작성
    -   layout: post # 레이아웃(필수). `page` 레이아웃을 사용하면 목록에 보이지 않는 글을 쓸 수 있음.
    -   title: '제목' # 제목(필수)
    -   author: `lastname.firstname` # 필자(필수). 왠만하면 회사 아이디(예: iolo.fitzowen) 사용
        -   autor 이름에 띄워쓰기 가능하도록 일부 수정 2021년 4월 10일
    -   tags: `[tag1,tag2,tag3,...]` # 태그 목록(선택). 왠만하면 특수문자없이 영소문자,숫자,-(하이픈),.(점)...만 사용.
    -   image: http://... # 커버이미지 url(선택)
    -   date: `YYYY-MM-DD HH:MM:SS` # 발행일(필수)
3.  처음 글을 쓰는 필자이라면 **작성자(author) 등록**(필수)
4.  주요 기술에 대한 태그가 없다면 **태그(tag) 등록**(선택)


### 작성자 신규등록
1.  `_authors` 디렉토리에 `[lastname.firstname.md](http://lastname.firstname.md/)` 이름으로 필자 정보 파일 추가
    -   사용자 포스트 목록 페이지의 URL:  [11stdev.github.io/author/lastname.firstname](http://11stdev.github.io/author/lastname.firstname)
2.  파일 상단에 [front matter](https://jekyllrb.com/docs/frontmatter/) 작성
    -   layout: author # 레이아웃(필수)
    -   name: `lastname.firstname` # post의 author와 매칭(필수). 왠만하면 회사 아이디(예: iolo.fitzowen) 사용. 왠만하면 특수문자없이 영소문자,숫자,-(하이픈),.(점)...만 사용.
    -   title: ... # 왠만하면 한글이름 사용( 필수)
    -   image: http://... # 프로필 이미지(필수)
    -   cover: http://... # 작성자 커버 이미지(선택)

### 태그 신규등록
1.  `_tags` 디렉토리에 `[tag-name.md](http://tag-name.md/)` 이름으로 태그 정보 파일 추가 (태글별 게시글을 모아볼 수 있음)
2.  파일 상단에 [front matter](https://jekyllrb.com/docs/frontmatter/) 작성
    -   layout: tag # 레이아웃(필수)
    -   name: `tag-name` # post의 tags 배열의 항목과 매칭(필수). 왠만하면 특수문자없이 영소문자,숫자,-(하이픈),.(점)...만 사용.
    -   title: ... # 좀 더 길고 구체적인 설명(필수)