---
title: "libpointmatcher 설치방법"
tags:
  - Blog
  - MathJax
  - Jekyll
  - LaTeX
use_math: true
Shell:      console, shell
sitemap :
  changefreq : daily
  priority : 1.0
---

### 수정사항

```bash
gedit build/examples/CMakeFiles/icp_simple.dir/link.txt
```
```bash
 -o icp_simple
 ```
 를 지우고 아래 코드로 변경
 ```bash
-shared -o icp_simple.so
```


```bash
gedit build/examples/CMakeFiles/icp_simple.dir/flags.make
```
```bash
CXX_FLAGS = -O3 -DNDEBUG   -fPIC -O3 -Wall -std=gnu++11
```

위의 부분을 다음과 같이 변경
```bash
CXX_FLAGS = -O3 -DNDEBUG   -O3 -Wall -std=gnu++11
```
빌드 후 example/icp_simple.so가 생성되었는지 확인


