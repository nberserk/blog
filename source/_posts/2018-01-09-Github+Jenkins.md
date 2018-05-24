---
layout: post
title:  "Github + Jenkins"
tags: spring
published: true
---

github PR을 테스트 할 수 있는 Jenkins job을 만들어 보자. 비교적 간단하지만 빈약한 문서, 여러가지 조합시 버그로 인한 exception등으로 결코 쉽지 않은 작업이었다.
그 중 제일은 역시나 회사 proxy때문 이였다.

## Jenkins

내가 테스트 했던 버전은 아래와 같다.

- Jenkins version : 2.102
- plugin> github pull request builder  : 1.39.0

이 플로그인을 사용하면 아래처럼 PR에 테스트 결과 등을 표시할 수 있다. 일종의 proof build라고 봐도 됨. TC 뿐만이 아니고 여러개의 status를 추가할 수도 있다.

![pr check]({{site.url}}/assets/pr-check.png)


### GitHub Pull Request Builder

설정 순서

1. Jenkins>Manage Jenkins>Manage Plugins 에서
    - `github pull request builder` 를 설치.
    - `github plugin`을 1.29 버전 이상으로 설치(1.28버전이 proxy지원 못하는 버그가 있음.)
1. Jenkins>Configure System> Github에서
    - fill `https://<enterprise github url>/api/v3` 입력
    - credential 추가.
        - github>Settings>Personal access tokens 에서 생성.
        - Jenkins>Credential>System>Global credentials> add credential
            -  Kind: `secret text` 선택
            - secret에 github의 토큰을 기입
            - ID에 적당한 이름을 적어주고

1. Jenkins>Configure System>github pull request build 에서
    - github server api url 채워주고
    - auto-manage webhooks 체크. 이걸 체크하면 웹훅을 자동으로 걸어준다.
1. Job 생성
    - select New Item > FreeStyle
    - source code Management
        - fille repository url,
        - name as `origin`,
        - refspec as `+refs/pull/${ghprbPullId}/*:refs/remotes/origin/pr/${ghprbPullId}/*`,
        - branch specifier `${sha1}`
    - Build Triggers
        - check `github pull request builder`
        - check `user github hooks for build triggering`
        - Advanced>List of organizations. Their members will be whitelisted. > 에 github orginization 이름을 써 준다.


### 기타

- log 파일을 tail로 잡으면서 설정하기, 문서도 잘 없고, 로그로 유추해서 설정하는게 시간을 단축할 수 있는 길임.
- freestyle project로 job을 만들고 Build>Invoke top-level maven targets으로 해야 에러가 안남. maven project로 빌드를 돌리면  [이런 에러가.](https://github.com/jenkinsci/ghprb-plugin/issues/618)


## References

- [이게 가장 잘 되어 있는 reference 였음. 그나마..](http://yaks-all-the-way-down.com/2016/04/22/Presenting-Git-Hub-Pull-Request-Builder.html#Organization )
- <https://jakubstas.com/github-and-jenkins-pull-request-checking/#.WlVLMZP1Vrx>
- <https://gist.github.com/ostinelli/972cfdb4bce51d428d3b>




