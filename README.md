# 장면 기반 영상 인코딩 프로세스
영상을 장면별로 자른 후 각 장면의 퀄리티 측정 및 재인코딩 작업 수행

# 필수 라이브러리 (환경변수 설정 권장)
* [FFmpeg](https://www.gyan.dev/ffmpeg/builds/)
* [pySceneDetect](https://www.scenedetect.com/download/)
* [ab-av1](https://github.com/alexheretic/ab-av1)

# 선택 라이브러리
* [RIFE-ncnn-vulkan](https://github.com/TNTwise/rife-ncnn-vulkan)

# 사용법
.bat 파일을 동영상 경로로 이동시킨 후 다음을 실행한다.
shot-sw.bat <파일 이름> <프레임 보간 배율> <인코더> <프리셋> <VMAF>

예시)
* shot-nvenc.bat "test.mxf" 2 "libsvtav1" 5 95
* shot-sw.bat "foo.mkv" 1 "libx264" fastest 93
* shot-nvenc.bat "ipsum.mp4" 1 "hevc_nvenc" slow 96

# 유의사항
* 파일 이름과 인코더는 쌍따옴표로 감쌀 것
* 인코더와 프리셋 파라미터는 FFmpeg의 규칙에 따를 것
* VMAF 점수는 93~96을 권장함
* RIFE-ncnn-vulkan은 v4.18을 사용하므로, 지원되는지 확인 필요함
* 프로세스를 거친 결과물은 VMAF 측정오차가 매우 심하니 유의할 것

## 일반 인코딩 결과물과 비교
[reference video](https://www.youtube.com/watch?v=tbWugSQ7kCk)
* 
|:---:|:---:|:---:|:---:|
|분류|작업 흐름|용량|작업 시간|
|Original| -- | 64.2MB | -- |
|Whole-Encode| FlowFrames(프레임 2배 보간, scene-detection 미사용)<br/>ab-av1 | 69.5MB | 9분 35초 |
|Shot-Based-Encode| pySceneDetection<br/>rife-ncnn-vulkan<br/>ab-av1 | 37.3MB | 29분 32초 |