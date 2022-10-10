---
title: Jekyll 블로그를 Github Pages에 호스팅하기
date: 2022-10-10T00:36:56+09:00
last_modified_at: 2022-10-10T19:12:02+09:00
tags:
- todo
---


이 문서는 우분투 기준으로 작성되었음.

다른 OS의 경우 아래 링크 보고 하세요.

https://jekyllrb.com/docs/installation/

## requirements

https://jekyllrb.com/docs/installation/#requirements

Pop!\_os 기준임.
### Ruby

2.5.0 이상이어야 함.

```bash
ruby -v # 설치됐는지 확인
```

https://www.ruby-lang.org/en/documentation/installation/#apt

```bash
sudo apt install ruby-full
```

### RubyGems

```bash
gem -v # 설치됐는지 확인
```

루비를 설치했으면 gem 도 기본적으로 설치되어있을 것.

### GCC and Make

```bash
gcc -v
g++ -v
make -v # 각각 설치 됐는지 확인
```

## install dependencies

```bash
sudo apt-get install ruby-full build-essential zlib1g-dev
```

```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
gem install jekyll bundler
```

## 디렉토리 세팅 (안해도 됨)

```bash
mkdir jekyll # jekyll 이라는 디렉토리 생성
cd jekyll # 방금 만든 jekyll 디렉토리로 이동
jekyll new ./ # 새로운 jekyll 사이트 생성
bundle add webrick # webrick 젬 추가 (webrick이 기본이었으나 빠져서 추가 안하면 오류 생길 수 있음)
bundle install # 젬 설치
bundle exec jekyll serve # jekyll 로컬 서버 실행
```

```bash
Configuration file: /home/laptop/repos/jekyll/_config.yml
            Source: /home/laptop/repos/jekyll
       Destination: /home/laptop/repos/jekyll/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
       Jekyll Feed: Generating feed for posts
                    done in 0.254 seconds.
 Auto-regeneration: enabled for '/home/laptop/repos/jekyll'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

`Server address` 에 적힌 주소를 통해 로컬에서 테스트할 수 있음

![](attachments/Pasted%20image%2020221008215645.png)

테마를 그냥 포크해서 쓰면 그만이라 새로 init할 필요가 없었다.


## 테마 선택

https://github.com/ghosind/Jekyll-Paper-Github

깔끔해서 좋다. 딱 내가 원하던 모습.

## 겪은 문제

### 파일명
무조건 date가 앞에 들어가야 한다.

이에 대한 대안은 posts가 아니라 collections를 쓰는 것인데 paginator가 collections을 지원 안한다. ㅎㅎ

```bash
# _config.yml
collections:
  pages:
    output: true
    permalink: /:collection/:path.md
```

우선 collections 를 등록했다. `./_pages` 디렉토리에 대응한다.

```html
<div class="container-posts">
  {% for post in site["pages"] reversed %}
  <div class="posts-list-item">
    <span class="posts-list-item-name float-left">
      <a href="{{ post.url }}">{{ post.title }}</a>
    </span>
    <span class="posts-list-item-date float-right">
      {{ post.date | date: '%Y-%m-%d' }}
    </span>
  </div>
  {% endfor %}
</div>
```

`_includes` 디렉토리에 페이지네이터 레이아웃 역할을 하는 `posts.html` 이 있길래 긁어와서 `pages` 컬렉션 용으로 수정하고 `pages.html` 로 저장했다.

별도의 정렬을 안 해도 생성 일자 기준으로 가장 먼저 생성된 순으로 정렬되기에 그대로 가져다 써서 `reversed`만 붙여줬다. (가장 최근에 생성된 것을 우선해서 보여주기 위해...)

페이지네이터까지 구현하는건 너무 귀찮을 것 같아서 때려쳤다.

### RSS, Sitemap

테마에서 RSS, Sitemap 을 지원한다.

`/feed.xml` 과 `/sitemap.xml` 로 대응하는데, 문제는 RSS feed가 컬렉션을 출력을 안 한다.

https://github.com/jekyll/jekyll-feed#collections

feed 플러그인에서 관련 내용을 확인한다.

```yml
feed:
  collections:
    - pages
```

나는 pages 컬렉션을 사용하므로 `_config.yml` 에 해당 내용을 추가했다. rss 출력은 `/feed/pages.xml` 로 나온다. 통합 피드는 안 되는듯.

### 댓글

giscus를 사용하였다.

### 마크다운 엔진

jekyll이 기본적으로 사용하는 kramdown이 문제가 좀 많다. 난데없이 헤더를 리스트 안에 넣는다던가 링크가 테이블로 인식 되는 등...

다른 마크다운 엔진이 있나 찾아보았다.

https://jekyllrb.com/docs/configuration/markdown/

다른 옵션으로는 commonmark가 있고, 적어도 렌더링 결과가 깃헙 마크다운 프리뷰와는 일치해야 한다고 생각해서 commonmark ghpages를 선택.

https://github.com/github/jekyll-commonmark-ghpages

설치 방법은 위 주소의 readme에 적혀있고, 정상적으로 출력되는 것을 확인.

### double curly brace를 liquid template 으로 인식

`{{` 가 마크다운의 코드 블럭에 들어간 것만으로도 리퀴트 템플릿 언어로 인식하는 문제가 발생했다. 문서의 해당 부분들은 아예 렌더링되지 않고 증발한다.

해결법으로는 `{% raw %}`, `{% endraw %}` 로 감싸는 것인데, 아무리 봐도 현실적인 해결법이 아니다. 마크다운의 모든 코드 블럭을 저걸로 감싸는 것도 말이 안되고, 리퀴드 때문에 발생한 문젠데 표준 마크다운 언어로 작성된 문서의 틀을 깨는 것도  탐탁치 않다. 무엇보다 코드 블럭 쓸 때마다 저걸 할 생각하니 갑갑하다.

https://jekyllrb.com/docs/liquid/tags/

다행히 Jekyll 4.0부터 `render_with_liquid: false` 로 마크다운 문서를 liquid로 파싱하는 것을 비활성화할 수 있다. 대신 liquid 템플릿 언어로 인한 혜택도 못 보므로 조심해서 사용해야 한다.

깃헙 페이지가 기본으로 제공하는 루비 버전은 [2022년 10월 10일 기준 3.9.2](https://pages.github.com/versions/)이므로 이 기능을 지원하지 않는다.

### timezone

+00:00 기준으로 날짜가 표시되었음.

https://jekyllrb.com/docs/configuration/options/

타임존을 설정해줘야 했다. 한국은 `Asia/Seoul`

타임존 목록은 [위키피디아 참고.](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)


### baseurl, 페이지 링크

baseurl 이 `jekyll-blog` 로 설정되어있는데, 페이지네이터의 페이지는 baseurl을 무시하고 절대 주소를 사용하고 있었다.

페이지 주소가 꼬였고, 절대 주소를 쓰는 페이지네이터 템플릿을 상대 주소로 바꿔야 했음.

```html
<a href="{{ post.url | remove_first:'/'}}">{{ post.title }}</a>
```

`remove_first:'/'` 가 앞 부분의 `/` 를 제거해주는 템플릿.

