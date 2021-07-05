---
layout: post
title: About Autonomous Vehicle
description: |
  - 자율주행 자동차의 정의
  - 자율주행 자동차의 발전 동향
tags:
  - autonomous
published: true
---
# About Autonomous Vehicle

## 자율주행 자동차의 정의
주변 환경을 인식, 위험을 판단, 적절한 제어를 자동차 스스로 할 수 있어 운전자의 개입이 최소화 된 자동차를 말한다.
미국 자동차 공학회에서 제시한 자율주행 단계는 다음과 같다. 
![j3016-levels-of-driving-automation-12-10.jpg]({{site.baseurl}}/_posts/autonomous/j3016-levels-of-driving-automation-12-10.jpg)
복잡해 보이지만... 간단히 정리하면

|Level| 0 | 1 | 2 | 3 | 4 | 5 |
|:----|:--|:--|:--|:--|:--|:--|
|     |자율주행X|속도 및 제동 등을 **일부**제어|**속도와 방향**제어|**도로상황**에 맞게 스스로 제어|운전자는 **목적지 설정**만|**100% 완전자율주행**|

_0~2단계 : 사고시 운전자 책임_

_3~5단계 : 사고시 제조사 책임_

자율주행을 위해선 주변 물체를 인식하는 기술, 자차의 위치를 인식하는 기술, 경로를 계획하는 기술 등 다양한 기술이 필요하다. 

## 첨단 운전보조장치(Advanced Driver Assistance System, ADAS)
자율주행 관련 기술로, 운전자의 편의와 안전을 위해 자동차에 설치된 전자 장치를 말한다. 그러나 ADAS의 발달이 곧 자율주행으로 이어지지는 않는다. 두 분야가 요구하는 기술이 다르기 때문이다.
먼저 ADAS에 어떤 것들이 있는지 살펴본다.
### 적응형 순항제어 시스템(Adaptive Cruise Control,ACC)
- **운전자가 페달을 조작하지 않아도** 자동차가 원하는 **속도를 스스로 유지**한다.
- 앞 차의 속도를 인식해 스스로 감속하여 원하는 거리를 유지할 수 있다.

환경을 인식해서 스스로 이동할 수 있는 능력을 가진 자동차를 자율주행 자동차라 한다. 자율주행을 위해선 
This release makes including third party plugins easier.
Until now, the push state approach to loading new pages has been interfering with embedded `script` tags.
This version changes this by simulating the sequential loading of script tags on a fresh page load.

This approach should work in a majority of cases, but can still cause problems with scripts that can't be added more than once per page.
If an issue can't be resolved, there's now the option to disable push state by setting `disable_push_state: true` in `config.yml`.

## What's happening?
The problem is as follows:
When the browser encounters a `script` tag while parsing a HTML page it will stop (possibly to make a request to fetch
an external script) and then execute the code before continuing parsing the page
(it's easy to how this can make your page really slow, but that's a different topic).

In any case, due of this behavior you can do things like include jQuery,
then run code that depends on jQuery in the next script tag:

~~~html
<script src=".../jquery.js"></script>
<script>
  $('#tabs').someJQueryFunction(); // works
</script>
~~~

I'd consider this an anti-pattern for the reason mentioned above,
but it remains common and has the advantage of being easy to understand.

However, things break when Hydejack dynamically inserts new content into the page.
It works fine for standard markdown content like `p` tags,
but when inserting `script` tags the browser will execute them immediately and in parallel,
because in most cases this is what you'd want.
However, this means that `$('#tabs').someJQueryFunction();` will run while the HTTP request for jQuery is still
in progress --- and we get an error that `$` isn't defined, or similar.

From this description the solution should be obvious: Insert the `script` tags one-by-one,
to simulate how they would get executed if it was a fresh page request.
In fact this is how Hydejack is now handling things (and thanks to rxjs' `concatMap` it was easy to implement),
but unfortunately this is not a magic solution that can fix all problems:

* Some scripts may throw when running on the same page twice
* Some scripts rely on the document's `load` event, which has fired long before the script was inserted
* unkown-unkowns

But what will "magically" solve all third party script problems, is disabling dynamic page loading altogether,
for which there's now an option.
To make this a slightly less bitter pill to swallow,
there's now a CSS-only "intro" animation that looks similar to the dynamic one.
Maybe you won't even notice the difference.

## Patch Notes
### Minor
* Support embedding `script` tags in markdown content
* Add `disable_push_state` option to `_config.yml`
* Add `disable_drawer` option to `_config.yml`
* Rename syntax highlighting file to `syntax.scss`
* Added [chapter on third party scripts][scripts] to documentation

### Design
* Add subtle intro animation
* Rename "Check out X for more" to "See X for more" on welcome\* page
* Replace "»" with "→" in "read more"-type of links

### Fixes
* Fix default color in gem-based theme

[scripts]: ../docs/7.5.2/scripts.md
