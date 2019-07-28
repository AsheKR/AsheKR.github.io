[Solid. A Bootstrap theme for Jekyll.](https://github.com/st4ple/solid-jekyll)

##### 블로그 포스팅

포스트를 추가하려면 `/_posts/blog/` 폴더 안에 `yyyy-mm-dd-name-of-post-like-this.markdown` 형식과 아래 내용을 포함하여 만든다.

```markdown
---
layout: post          #important: don't change this
title: "Name of post like this"
date: yyyy-mm-dd hh:mm:ss
author: Name
categories:
- blog                #important: leave this here
- category1
- category2
- ...
img: post01.jpg       #place image (850x450) with this name in /assets/img/blog/
thumb: thumb01.jpg    #place thumbnail (70x70) with this name in /assets/img/blog/thumbs/
---
아래 `<!--more-->`  태그가 오기전까지가 포스트 목록에서 보여줄 요약문이다.
<!--more-->
이곳은 Read More을 했을 때 포스트 상세내용이 포함되는 곳이다.
```
##### Project post

포트폴리오를 추가하려면 `/_posts/project/` 폴더 안에 `yyyy-mm-dd-name-of-the-project.markdown` 형식과 아래 내용을 포함하여 만든다.

```markdown
---
layout: project       #important: don't change this
title:  "Name of the project"
date: yyyy-mm-dd hh:mm:ss
author: Name
categories:
- project             #important: leave this here
img: portfolio_10.jpg #place image (600x450) with this name in /assets/img/project/
thumb: thumb02.jpg
carousel:
- single01.jpg        #place image (1280x600) with this name in /assets/img/project/carousel/
- single02.jpg  
- ...
client: Company XY
website: http://www.internet.com
---
#### This is a heading
This is a regular paragraph. Write as much as you like.
```
##### Question entry

페이지 메인의 자주묻는 질문에 포함될 내용은 `/_posts/question/` 폴더 안에 `yyyy-mm-dd-do-i-have-a-question.markdown` 형식과 아래 내용을 포함하여 만든다.

```markdown
---
layout: question
title:  "Do I have a question?"
date: yyyy-mm-dd hh:mm:ss
author: First Last
categories:
- question            #important: leave this here
---

## License
This theme is licensed under [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/).
