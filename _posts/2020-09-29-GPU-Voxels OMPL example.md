---
title: "GPU-Voxels Open Motion Planning Library Example 생성"
tags:
  - Blog
  - MathJax
  - Jekyll
  - LaTeX
use_math: true
---

이 포스트에서는 뉴로메카사의 Indy7을 이용하여 GPU-Voxels 라이브러리를 활용한 Motion Planning 예제를 만든다.
예제를 실행하기 전 GPU-Voxels를 완전하게 설치해야 한다.
### Indy7-description을 다운로드
먼저 neuromeka github에서 indy7의 로봇 모델 파일을 받는다.
```bash
git clone https://github.com/neuromeka-robotics/indy-ros
```

### gvl_ompl_planning example build
gpu-voxles 폴더에 있는 gvl_ompl_planning을 build한다.
build를 성공하기 위해서는 먼저 ros가 설치되어 있으며 ros ompl을 설치해야 한다.
```bash
cmake-gui .
```

