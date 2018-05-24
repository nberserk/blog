---
layout: post
title:  "redirecting www.domain to domain"
tags: aws s3 route53
published: true
---

www.domain.com은 domain.com의 서브 도메인이라고 할 수 있다.
그러니 route53에서 www.domain.com의 A 항목을 만들어서 s3 web으로 보내고, s3 web은 domain.com으로 리다이렉션 시키면 된다.

- 이미 domain.com의 route53 설정이 되어 있다고 가정
- www.domain.com 과 domain.com 2개의 s3 bucket 생성. 이름은 꼭 도메인 네임과 같아야 함.
- route53에서 www.domain.com의 record set 생성. type은 'A'로 하고, www.domain.com의 s3 bucket으로 alias를 건다. 아래 스샷 참조.
![image](https://user-images.githubusercontent.com/900639/30008944-36364c9a-915f-11e7-93bc-1c4d3a9066fe.png)

- aws>s3> www.domain.com으로 가서 properties tab의 static web hosting 을 인에이블하고 redirection을 domain.com으로 해준다.

![image](https://user-images.githubusercontent.com/900639/30008999-b6a9f0de-915f-11e7-8ae9-4ffde5898281.png)

- 시간이 얼마 지난후, www.domain.com을 들어가면 domain.com으로 redirect되는 것을 확인 할 수 있다.



