---
title:  "wireshark 에서 패킷 캡쳐 하는 방법"
date:   2019-10-31 22:00:00 +0900
tags: [windows, wireshark]
key: 20191031_01
---
## 패킷캡쳐

1.  ip로 찾고 싶을 때, 상단 display filter 에 `[ ip.dst == 'ip주소' ]`
2.  info 에 원하는 url 보낸 것 찾아서 우클릭 > follow > tcp stream > 인코딩 UTF8로 변경
3.  SSL 걸린것은 확인할 수 없음 ( 인증서 등록 따로하고 확인 가능하다고 하는데 확인해보지 않음 )

## 패킷캡쳐가 되지 않을 때 ( 2.6.8 기준 )

관리자 모드로 실행 후 이더넷 클릭