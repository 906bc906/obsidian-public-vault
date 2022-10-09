---
title: Jekyll 블로그를 Github Pages에 호스팅하기
date: 2022-10-10T00:36:56+09:00
last_modified_at: 2022-10-10T00:36:56+09:00
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

utterances 와 giscus