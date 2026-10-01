# 📘 OpenCV 전수조사 분석 리포트 (한국어)

> 작성: Claude Code (카리나 페르소나) · 작성일: 2026-10-01
> 대상 리포지토리: **https://github.com/bmshin94/opencv**
> (원본: https://github.com/opencv/opencv · 브랜치 `5.x`)
> 작업 브랜치: `claude/elegant-hopper-3c5qnd`

---

## 📑 목차

1. [리포지토리 정체 파악](#1-리포지토리-정체-파악)
2. [폴더 전수조사](#2-폴더-전수조사)
3. [이게 뭐하는 건가 / 언제 쓰나](#3-이게-뭐하는-건가--언제-쓰나)
4. [쉬운 설명 (비유 버전)](#4-쉬운-설명-비유-버전)
5. [설치 및 사용법](#5-설치-및-사용법)
6. [플러그인? 스킬? MCP?](#6-플러그인-스킬-mcp)
7. [API 토큰 필요 여부 & 라이선스](#7-api-토큰-필요-여부--라이선스)
8. [AI 에이전트 구축 활용](#8-ai-에이전트-구축-활용)
9. [React / PHP 구현 방법](#9-react--php-구현-방법)
10. [유튜브 강의 제작 가능성](#10-유튜브-강의-제작-가능성)
11. [수익화 아이디어 10선](#11-수익화-아이디어-10선)
12. [추천 실행 전략 (3단 로켓)](#12-추천-실행-전략-3단-로켓)
13. [법적 체크리스트](#13-법적-체크리스트)
14. [참고 링크](#14-참고-링크)

---

## 1. 리포지토리 정체 파악

| 항목 | 내용 |
|---|---|
| **리포지토리** | https://github.com/bmshin94/opencv |
| **원본(Upstream)** | https://github.com/opencv/opencv |
| **정식 명칭** | OpenCV — Open Source Computer Vision Library |
| **버전** | `5.1.0-dev` (차세대 5.x 개발 브랜치, 정식 릴리즈 전) |
| **라이선스** | Apache License 2.0 (상업적 이용·수정·비공개 배포 자유) |
| **용량** | 약 255 MB |
| **코드 규모** | C++ `.cpp` 1,650 / 헤더 `.hpp` 821 / `.h` 624 / `.c` 520 / Python 363 / 문서 343 |
| **현재 브랜치** | `claude/elegant-hopper-3c5qnd` |
| **포크 고유 변경** | `CLAUDE.md` 추가 (카리나 페르소나 가이드) — PR #1 로 머지 |

### 🔎 핵심 사실
포크에서 **OpenCV 본체 코드는 전혀 수정되지 않았다.** 유일한 변경은 `CLAUDE.md` 추가.
즉 현재 상태 = **순수 OpenCV 5.x 원본 + Claude 페르소나 설정**.

---

## 2. 폴더 전수조사

### 🧩 `modules/` — 기능 모듈 24개 (심장부)

각 폴더가 독립 라이브러리(`opencv_core`, `opencv_dnn` 등)로 빌드된다.

| 모듈 | 파일수 | 역할 |
|---|---|---|
| `core` | 435 | 토대. `cv::Mat`(이미지=행렬), 행렬연산, 멀티스레딩, CUDA/OpenCL 연동, `FileStorage` |
| `dnn` | **492** | **딥러닝 추론 엔진.** ONNX/TensorFlow/Caffe 모델 로드·실행 |
| `imgproc` | 195 | 영상처리. 블러, Canny 엣지, 리사이즈, 색변환, 외곽선, 형태학 |
| `geometry` | 121 | 3D 기하. 삼각측량, 호모그래피, 포즈 추정 |
| `objdetect` | 104 | 객체검출. QR/바코드, 얼굴(YuNet), ArUco·ChArUco 마커, MCC 차트 |
| `videoio` | 102 | 카메라/동영상 I/O. FFmpeg, GStreamer, 웹캠, RTSP |
| `ptcloud` | 85 | 포인트 클라우드 (LiDAR/3D 스캐너) |
| `imgcodecs` | 84 | 이미지 파일 I/O. JPEG, PNG, WebP, TIFF, JPEG2000 |
| `features` | 81 | 특징점 검출/매칭. ORB, SIFT, AKAZE |
| `video` | 76 | 동영상 분석. 옵티컬 플로우, 추적, 배경제거 |
| `photo` | 70 | 사진 보정. 인페인팅, HDR, 디노이즈 |
| `stitching` | 46 | 파노라마 자동 합성 |
| `flann` | 45 | 고속 근사 최근접 이웃 탐색 (벡터 검색) |
| `calib` | 25 | 카메라 캘리브레이션 (렌즈 왜곡 보정) |
| `highgui` | 25 | GUI 창. `imshow()`, 트랙바, 마우스 이벤트 |
| `ts` | 20 | 테스트 프레임워크 |
| `stereo` | 16 | 스테레오 비전 (양안 → 깊이맵) |
| `ml` | 4 | 전통 머신러닝 (SVM, 랜덤포레스트, KNN) |
| `python` / `java` / `js` / `objc` | — | **언어 바인딩 자동 생성기** |
| `world` | 3 | 전체 모듈을 단일 라이브러리로 통합 |

### ⚙️ `3rdparty/` — 번들 외부 라이브러리 23개
`libjpeg-turbo` `libpng` `libwebp` `libtiff` `openjpeg` `libspng` `libjasper` (이미지 코덱) ·
`protobuf` `flatbuffers` (모델 파싱) · `ffmpeg` (동영상) · `zlib` `zlib-ng` (압축) ·
`tbb` (병렬) · `ippicv` (Intel 최적화) · `harfbuzz` (텍스트) · `mlas` (행렬곱) ·
`dlpack` (텐서 교환) · `orbbecsdk` (뎁스 카메라) · `fastcv` `libtim-vx` (모바일 NPU) ·
`clapack` `cpufeatures` `ittnotify`

→ **인터넷 없이 빌드 가능**하도록 전부 포함.

### 🚀 `hal/` — Hardware Acceleration Layer
`carotene`(ARM NEON) · `riscv-rvv`(RISC-V 벡터) · `kleidicv`(ARM 공식) · `ipp`(Intel) · `armpl` · `fastcv` · `ndsrvp`
→ 동일 API가 CPU 아키텍처별로 다른 최적화 코드를 사용.

### 💡 `samples/` — 실전 예제 (가장 실용적!)

#### `samples/dnn/` — AI 추론 예제 (핵심 자산)
| 파일 | 기능 |
|---|---|
| `object_detection.py/.cpp` | YOLO 객체검출 |
| `face_detect.py/.cpp` | 얼굴 검출 + 인식 |
| `segmentation.py/.cpp` | 세그멘테이션 |
| `super_resolution.py/.cpp` | 저화질 → 고화질 업스케일 |
| `inpainting.py`, `ldm_inpainting.py` | 물체 지우기 (Diffusion 포함) |
| `colorization.py/.cpp` | 흑백 → 컬러 복원 |
| `virtual_try_on.py` | 가상 피팅 |
| `human_parsing.py/.cpp` | 신체 부위 분리 |
| `text_detection.py/.cpp` | 이미지 내 텍스트 검출 |
| `object_tracker.py/.cpp`, `siamrpnpp.py` | 객체 추적 |
| `deblurring.py/.cpp` | 흔들림 보정 |
| `alpha_matting.py/.cpp` | 알파 매팅 (정밀 배경분리) |
| `auto_white_balance.py/.cpp` | 자동 화이트밸런스 |
| `edge_detection.py/.cpp` | AI 기반 엣지 검출 (DexiNed) |
| `speech_recognition.py/.cpp` | **음성 → 텍스트** |
| `qwen3_tts.py` | **텍스트 → 음성 (TTS)** |
| `gemma3_inference.py` | **구글 Gemma3 LLM 추론** |
| `qwen_inference.py` | **Qwen LLM 추론** |
| `gpt2_inference.py` | GPT-2 추론 |
| `vlm_inference.py` | **비전-언어 모델 (이미지 보고 설명)** |
| `action_recognition.py` | 행동 인식 |
| `mask_rcnn.py` | 인스턴스 세그멘테이션 |
| `models.yml` | **모델 동물원 목록** + 다운로드 정보 |
| `download_models.py` | 모델 자동 다운로드 스크립트 |

**`models.yml` 수록 모델:** YOLOv3/v4/v5l/v8(s,m,l,x) · SSD(Caffe/TF) · Faster R-CNN ·
SqueezeNet · GoogleNet · ResNet · FCN-ResNet50/101 · U2NetP · **SAM(Segment Anything)** ·
DexiNed · ReID · ViT · NanoTrack · DaSiamRPN · **LaMa** · **LDM Inpainting** · MCC · MODNet · SeeMoreDetails · FC4

#### 기타 `samples/` 하위
- `python/` (34개): `qrcode.py` `calibrate.py` `stereo_match.py` `stitching_detailed.py` `grabcut.py` `odometry.py` `point_cloud.py` `volume.py` `macbeth_chart_detection.py` `aruco_detect_board_charuco.py` 등
- `cpp/` (52개) · `java/` · `android/` · `swift/` · `winrt/` · `wp8/`
- **`slam/`**: `visual_odometry.py/.cpp` — 카메라 기반 위치 추정
- GPU: `gpu/` `opencl/` `sycl/` `opengl/` `directx/` `va_intel/` `tapi/`
- `hal/` · `ptcloud/` · `gdb/`(디버깅 헬퍼) · `semihosting/`(임베디드) · `data/`(테스트 이미지)

### 🛠️ `apps/` — 완성 도구
`interactive-calibration` (대화형 카메라 보정) · `multiview-calibration` (다중 카메라) ·
`chromatic-aberration-calibration` (색수차 보정) · `model-diagnostics` (DNN 모델 진단) ·
`pattern-tools` (체커보드 패턴 생성) · `version`

### 🌍 `platforms/` — 크로스 컴파일
`android` · `ios` · `apple` · `osx` · `linux` · **`js`(WebAssembly: `build_js.py`, `opencv_js.config.py`)** ·
`maven` · `wince` · `winrt` · `winpack_dldt` · `semihosting` · `scripts`

### 📚 기타
- `doc/tutorials/` — 공식 튜토리얼 (app, calib3d, core, dnn, features, geometry, gpu, imgproc, introduction, ios, objdetect, others, photo, ptcloud)
- `cmake/` — 빌드 시스템 모듈 · `CMakeLists.txt` (85 KB)
- `.github/workflows/` — CI 2개 (`5.x.yml`, `PR-5.x.yaml`)
- `include/` · `docs_sphinx/` · `supporters.txt` · `LICENSE` · `SECURITY.md`

---

## 3. 이게 뭐하는 건가 / 언제 쓰나

### 한 줄 정의
> **"컴퓨터에게 눈을 달아주는 라이브러리"**

### 표준 파이프라인
```
📷 이미지/영상 입력  →  🔧 전처리  →  🤖 AI 추론  →  📊 결과 활용
  videoio/imgcodecs     imgproc        dnn          highgui
```

PyTorch·TensorFlow로 학습한 모델도 **실제 서비스 배포 시에는 OpenCV로 전처리/후처리**하는 것이
업계 표준. `dnn` 모듈만으로 PyTorch 없이 ONNX 추론이 가능해 **Docker 이미지가 6GB → 300MB**로 줄어든다.

### 활용 분야
| 분야 | 사례 |
|---|---|
| 🏭 스마트 팩토리 | 불량품 검사, 치수 측정, 바코드/QR 스캔 |
| 🚗 자율주행·ADAS | 차선 인식, 보행자 검출, Visual SLAM |
| 🏥 의료영상 | X-ray/MRI 전처리, 세포 카운팅 |
| 📱 모바일 앱 | 얼굴 필터, 문서 스캐너, AR 측정 |
| 🛡️ 보안·CCTV | 침입 감지, 인원 카운팅, 번호판 인식 |
| 🛒 리테일 | 무인매장, 가상 피팅, 선반 재고 |
| 🤖 로보틱스·드론 | 비주얼 내비게이션, 물체 집기 |
| 🎬 영상 제작 | 배경 제거, 업스케일링, 객체 제거 |
| 🧠 AI 서비스 | YOLO 실시간 검출, 생성 AI 전/후처리 |

---

## 4. 쉬운 설명 (비유 버전)

### 컴퓨터가 보는 이미지
```
사람:      🐱 "고양이!"
컴퓨터:    [[34,67,120],[36,70,125],...]  ← 1920×1080×3 = 622만 개 숫자
```
이 숫자 덩어리에서 의미를 뽑아내는 기술이 컴퓨터 비전이고, OpenCV가 그 도구상자다.

### 공구함 비유
| 비유 | 실제 모듈 |
|---|---|
| 🪵 원목 (재료) | 이미지 읽기 — `imgcodecs`, `videoio` |
| 📏 자·끌·사포 | 자르기·리사이즈·블러·엣지 — `imgproc` |
| 🔨 전동드릴 | AI 추론 — `dnn` |
| 🪑 작업대 | `cv::Mat` — `core` |
| 🖼️ 진열대 | 화면 출력 — `highgui` |

### 회사 조직도 비유
```
🏢 OpenCV 주식회사
├─ 🏛️ modules/     부서 24개 (core=총무, imgproc=가공, dnn=AI, python/java/js=통역)
├─ ⚙️ 3rdparty/    외주 협력사 23곳
├─ 🚀 hal/         특수장비실 (CPU별 초고속 튜닝)
├─ 💡 samples/     사내 요리책 (완성 레시피 100+)
├─ 🛠️ apps/        완성품 전시실
├─ 🌍 platforms/   해외지사 (Android/iOS/Web)
└─ 📚 doc/         사내 교육자료
```

### 핵심 원리: 왜 `modules/python/` 이 중요한가
OpenCV 본체는 C++(실시간 처리는 프레임당 33ms 제한). 그런데 `import cv2` 로 Python에서 쓸 수 있는 이유는
`modules/python/`의 **자동 바인딩 생성기**가 C++ 헤더를 읽어 래퍼를 만들어주기 때문.

> **"코드는 Python처럼 쉽게, 속도는 C++처럼 빠르게"** — OpenCV가 20년째 1위인 이유.
> `java/` `js/` `objc/`도 동일 원리. JS는 WebAssembly라 **브라우저에서도 실행 가능**.

### `samples/dnn/` 가 왜 대박인가
```
❌ 일반 AI 배포: PyTorch(2.5GB)+CUDA(3GB)+transformers → Docker 6GB
✅ OpenCV dnn:  opencv-python(90MB)+ONNX 모델       → Docker 300MB
```
→ **"보고·듣고·말하고·추론하는" 멀티모달 AI 부품이 이미 한 폴더에 전부 존재.**

### 3단계 시작 가이드
```bash
pip install opencv-python numpy                 # 1단계
cd samples/python && python qrcode.py           # 2단계 — QR 리더 완성
cd ../dnn && python download_models.py --model yolov8
python object_detection.py --model yolov8       # 3단계 — 실시간 객체검출 완성
```
> 소스 255MB를 다 이해할 필요 없음. `samples/` 를 "복붙 재료 창고"로 쓰면 된다.

---

## 5. 설치 및 사용법

### 방법 A — 바이너리 설치 (99% 권장 ⭐)
```bash
# 4가지 중 하나만! (중복 설치 시 충돌 ⚠️)
pip install opencv-python                   # 기본 (GUI 포함) — 입문 추천
pip install opencv-python-headless          # GUI 제거 — 서버/Docker 추천
pip install opencv-contrib-python           # 기본 + 추가 알고리즘
pip install opencv-contrib-python-headless  # 조합
pip install numpy                           # 거의 항상 필요
```
```python
import cv2 as cv
print(cv.__version__)
print(cv.getBuildInformation())   # 어떤 기능이 활성화됐는지 전체 출력
```
> ⚠️ pip 배포판은 **안정 버전 4.x**. 포크는 **5.1.0-dev** 이므로 5.x 사용엔 소스 빌드 필요.
> 학습·개발 목적이면 4.x 로 시작하는 것이 효율적 (예제 코드 대부분 호환).

#### 타 언어
```bash
sudo apt install libopencv-dev          # C++ / Ubuntu
brew install opencv                     # C++ / macOS
# C++ 컴파일: g++ main.cpp -o app $(pkg-config --cflags --libs opencv4)

npm install @techstark/opencv-js        # 브라우저 (WASM)
npm install @u4/opencv4nodejs           # Node.js
# Java/Android (Gradle): implementation 'org.opencv:opencv:4.12.0'
# C# (NuGet): OpenCvSharp4
# Go: gocv.io/x/gocv
# Rust: cargo add opencv
```

### 방법 B — 소스 빌드 (오빠 포크 5.x)
```bash
# 1. 의존성
sudo apt update && sudo apt install -y build-essential cmake git pkg-config \
    libgtk-3-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libjpeg-dev libpng-dev libtiff-dev libtbb-dev python3-dev python3-numpy

# 2. 클론
git clone https://github.com/bmshin94/opencv.git && cd opencv

# 3. 빌드 폴더 분리
mkdir build && cd build

# 4. CMake 설정
cmake -D CMAKE_BUILD_TYPE=Release \
      -D CMAKE_INSTALL_PREFIX=/usr/local \
      -D BUILD_opencv_python3=ON \
      -D BUILD_EXAMPLES=ON \
      -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF \
      -D WITH_FFMPEG=ON -D WITH_GSTREAMER=ON \
      -D WITH_TBB=ON -D WITH_OPENCL=ON -D WITH_CUDA=OFF ..
      # CUDA: -D WITH_CUDA=ON -D CUDA_ARCH_BIN=8.6

# 5~6. 컴파일 & 설치
make -j$(nproc) && sudo make install && sudo ldconfig
```
> CMake 출력 말미의 요약표에서 `Python 3`, `numpy`, `FFMPEG: YES` 확인 필수.

#### 주요 CMake 옵션 (CMakeLists.txt 실측)
| 옵션 | 기본값 | 의미 |
|---|---|---|
| `WITH_CUDA` | OFF | NVIDIA GPU 가속 |
| `WITH_OPENCL` | ON | 범용 GPU 가속 |
| `WITH_FFMPEG` | ON | 동영상 코덱 |
| `WITH_GSTREAMER` | ON | 스트리밍 파이프라인 |
| `WITH_PROTOBUF` | ON | DNN 모델 파싱 (필수) |
| `WITH_OPENVINO` | 조건부 | Intel CPU/NPU 추론 가속 |
| `WITH_VULKAN` | OFF | Vulkan GPU 추론 |
| `WITH_TBB` | OFF | Intel 병렬처리 |
| `WITH_QT` | OFF | Qt GUI 백엔드 |
| `BUILD_opencv_js` | OFF | WebAssembly 빌드 |
| `BUILD_JAVA` | 조건부 | Java/Android 바인딩 |
| `BUILD_SHARED_LIBS` | ON | 동적 라이브러리 |

#### 플랫폼별 빌드
```bash
python ./platforms/js/build_js.py build_wasm --build_wasm    # 웹 (Emscripten 필요)
python ./platforms/android/build_sdk.py --ndk_path=$ANDROID_NDK   # Android
```

### 기본 사용법 치트시트
```python
import cv2 as cv
import numpy as np

# 1. 읽기/쓰기/보기
img = cv.imread('photo.jpg')          # ⚠️ BGR 순서 (RGB 아님) — 최다 발생 함정
cv.imwrite('out.png', img)
cv.imshow('w', img); cv.waitKey(0); cv.destroyAllWindows()

# 2. 정보
h, w, c = img.shape; print(img.dtype)   # uint8 (0~255)

# 3. 영상처리
gray  = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
small = cv.resize(img, (640, 480))
blur  = cv.GaussianBlur(img, (5,5), 0)
edge  = cv.Canny(gray, 100, 200)
_, bw = cv.threshold(gray, 127, 255, cv.THRESH_BINARY)
rot   = cv.rotate(img, cv.ROTATE_90_CLOCKWISE)
crop  = img[100:300, 50:250]            # numpy 슬라이싱

# 4. 그리기
cv.rectangle(img, (10,10), (100,100), (0,255,0), 2)
cv.circle(img, (50,50), 20, (0,0,255), -1)
cv.putText(img, 'Hello', (10,200), cv.FONT_HERSHEY_SIMPLEX, 1, (255,255,255), 2)

# 5. 웹캠
cap = cv.VideoCapture(0)                # 0 / 'video.mp4' / 'rtsp://...'
while True:
    ret, frame = cap.read()
    if not ret: break
    cv.imshow('cam', frame)
    if cv.waitKey(1) == 27: break       # ESC
cap.release()

# 6. QR코드
data, pts, _ = cv.QRCodeDetector().detectAndDecode(img)

# 7. AI 추론
net = cv.dnn.readNet('model.onnx')
net.setPreferableBackend(cv.dnn.DNN_BACKEND_OPENCV)
net.setPreferableTarget(cv.dnn.DNN_TARGET_CPU)
blob = cv.dnn.blobFromImage(img, 1/255.0, (640,640), swapRB=True, crop=False)
net.setInput(blob); out = net.forward()
```

### 학습 순서 추천
`imread/imshow` → `cvtColor/resize/threshold` → `findContours` → `VideoCapture` → `dnn.readNet`
교재: 포크의 `samples/python/`, `doc/tutorials/`

---

## 6. 플러그인? 스킬? MCP?

### 결론: **셋 다 아님. "라이브러리(SDK)"다.**

| 구분 | 정의 | OpenCV |
|---|---|---|
| **라이브러리/SDK** | 내 프로그램이 `import` 해서 쓰는 코드 모음 | ✅ **이것** |
| 플러그인 | 호스트 앱(VSCode/Figma)에 꽂아 기능 확장 | ❌ |
| Claude Skill | Claude에 작업 절차를 가르치는 md 문서 묶음 | ❌ |
| MCP 서버 | AI 에이전트가 툴을 호출하는 표준 프로토콜 서버 | ❌ |
| 프레임워크 | 내 코드가 그 안에 들어가는 구조 (Django/Spring) | ❌ (방향 반대) |

```
❌ 플러그인 구조:  Claude/VSCode ──꽂기──> OpenCV
✅ 실제 구조:      내 프로그램 ──import──> OpenCV ──> 결과
```
> 포크의 `CLAUDE.md` 는 사용자가 직접 추가한 페르소나 설정 파일이며 OpenCV 본체와 무관.

### 단, 직접 MCP/스킬/플러그인으로 감쌀 수 있다 (핵심 기회)
```
[OpenCV 라이브러리] ← 재료
   ↓ 감싸면
🔌 MCP 서버      → Claude가 이미지 분석 가능   ⭐⭐⭐⭐⭐
📄 Claude Skill  → 이미지 처리 레시피 문서     ⭐⭐⭐
🧩 VSCode 플러그인 → 에디터 내 이미지 미리보기  ⭐⭐
🌐 REST API 서버 → 어디서든 호출             ⭐⭐⭐⭐
```

#### 예시: OpenCV MCP 서버 (Python)
```python
# opencv_mcp_server.py
from mcp.server.fastmcp import FastMCP
import cv2 as cv

mcp = FastMCP("opencv-vision")

@mcp.tool()
def image_info(path: str) -> dict:
    """이미지 크기, 채널, 평균 밝기 반환"""
    img = cv.imread(path)
    if img is None:
        return {"error": "읽기 실패"}
    h, w, c = img.shape
    return {"width": w, "height": h, "channels": c,
            "mean_brightness": float(img.mean())}

@mcp.tool()
def detect_faces(path: str) -> dict:
    """사진에서 얼굴 개수와 위치 검출"""
    img = cv.imread(path)
    det = cv.FaceDetectorYN.create("face_detection_yunet_2023mar.onnx", "",
                                   (img.shape[1], img.shape[0]))
    _, faces = det.detect(img)
    boxes = [] if faces is None else [[int(v) for v in f[:4]] for f in faces]
    return {"count": len(boxes), "boxes": boxes}

@mcp.tool()
def read_qr(path: str) -> dict:
    """QR코드 내용 읽기"""
    data, _, _ = cv.QRCodeDetector().detectAndDecode(cv.imread(path))
    return {"data": data or None}

@mcp.tool()
def resize_image(path: str, out: str, width: int, height: int) -> str:
    """이미지 크기 변경 후 저장"""
    cv.imwrite(out, cv.resize(cv.imread(path), (width, height)))
    return out

if __name__ == "__main__":
    mcp.run()
```
→ `~/.claude.json` 의 `mcpServers` 에 등록하면 Claude가 이미지를 "볼" 수 있게 된다.

---

## 7. API 토큰 필요 여부 & 라이선스

### 토큰: **전혀 필요 없음** 🎉
```python
import cv2 as cv                # 토큰 없음
img = cv.imread('a.jpg')        # 네트워크 통신 없음
net = cv.dnn.readNet('m.onnx')  # 서버 호출 없음 — 전부 로컬 실행
```

| 항목 | 클라우드 AI API | OpenCV |
|---|---|---|
| API 키 | 필수 🔑 | ❌ 불필요 |
| 과금 | 호출당 💸 | ❌ 무료 |
| 인터넷 | 필수 | ❌ 오프라인 가능 |
| 사용량 제한 | 있음 | ❌ 무제한 |
| 데이터 외부 전송 | 있음 ⚠️ | ❌ 로컬에만 |
| 지연 | 네트워크 왕복 | ⚡ 즉시 |

### 비즈니스 무기
> **"오프라인 동작 + 데이터가 기기 밖으로 나가지 않음"**
> → 병원(의료정보보호) · 공장(망분리) · 공공기관 · 금융 · 개인정보 민감 서비스
> → **클라우드 AI를 구조적으로 못 쓰는 시장** = 경쟁 적고 단가 높음

### 토큰이 필요해지는 경계선
| 상황 | 토큰 |
|---|---|
| OpenCV 자체 기능 전부 | ❌ |
| ONNX 로컬 추론 (YOLO/SAM/Gemma3) | ❌ |
| HuggingFace 비공개 리포에서 모델 다운로드 | ⚠️ HF 토큰 (1회) |
| 결과를 GPT/Claude에 보내 설명 생성 | ⚠️ API 키 |
| RTSP CCTV 접속 | ⚠️ 카메라 인증정보 |

---

## 8. AI 에이전트 구축 활용

### 아키텍처
```
📷 카메라/이미지
     ↓ videoio, imgcodecs      (입력)
🔧 전처리 (리사이즈/정규화/디노이즈)
     ↓ imgproc
👁️ 인지 (객체검출/OCR/세그멘테이션/추적)
     ↓ dnn, objdetect, features
📊 구조화된 사실 (JSON)
     ↓
🧠 LLM 판단 & 계획
     ↓
🛠️ 행동 (알림/제어/DB/리포트)
```

### 핵심 패턴: "비싼 LLM을 아껴 쓰는 게이트키퍼"
```python
# ❌ 나쁨: 30fps × 24h = 259만 호출 → 비용 폭발
# ✅ 좋음: OpenCV 1차 필터링, LLM은 "사건"만 처리
import cv2 as cv

cap = cv.VideoCapture('rtsp://cctv')
bgsub = cv.createBackgroundSubtractorMOG2()
net = cv.dnn.readNet('yolo.onnx')

while True:
    ret, frame = cap.read()
    if not ret: break

    motion = bgsub.apply(frame)                # 1단계: 움직임 필터 (CPU 1%)
    if cv.countNonZero(motion) < 5000: continue

    detections = run_yolo(net, frame)          # 2단계: 로컬 YOLO (무료)
    if not is_interesting(detections): continue

    llm_analyze(frame, detections)             # 3단계: 중요 순간만 LLM (유료)
```
→ **LLM 호출 99.9% 절감.** 실무 표준 패턴.

### 포크에 이미 존재하는 "에이전트 부품"
| 에이전트 능력 | 파일 |
|---|---|
| 👁️ 본다 | `samples/dnn/object_detection.py`, `segmentation.py` (SAM) |
| 🙂 얼굴 인식 | `samples/dnn/face_detect.py` |
| 📝 글 읽기 | `samples/dnn/text_detection.py` + OCR |
| 🎯 추적 | `samples/dnn/object_tracker.py`, `siamrpnpp.py` |
| 👂 듣는다 | `samples/dnn/speech_recognition.py` |
| 🗣️ 말한다 | `samples/dnn/qwen3_tts.py` |
| 💬 추론 | `samples/dnn/gemma3_inference.py`, `qwen_inference.py` |
| 👁️‍🗨️ 보고 말함 | `samples/dnn/vlm_inference.py` ← 멀티모달 핵심 |
| 🧭 위치 추정 | `samples/slam/visual_odometry.py` |
| 🔍 유사 검색 | `modules/flann` (벡터 검색 = RAG 기반 기술) |
| 📏 거리 측정 | `modules/stereo`, `modules/geometry` |
| 🔖 마커 인식 | `objdetect` ArUco/ChArUco |

### DNN 가속 백엔드 (`modules/dnn/include/opencv2/dnn/dnn.hpp` 실측)
```python
net.setPreferableBackend(cv.dnn.DNN_BACKEND_CUDA)
net.setPreferableTarget(cv.dnn.DNN_TARGET_CUDA_FP16)   # 반정밀도 = 약 2배
```
- **백엔드:** `OPENCV`(CPU) · `CUDA` · `INFERENCE_ENGINE`(OpenVINO) · `VKCOM`(Vulkan) · `WEBNN`(브라우저) · `TIMVX` · `CANN`
- **타겟:** `CPU` · `CPU_FP16` · `OPENCL` · `OPENCL_FP16` · `CUDA` · `CUDA_FP16` · `VULKAN` · `NPU` · `MYRIAD` · `FPGA` · `HDDL`

→ **코드 한 줄 변경**으로 Jetson → 라즈베리파이 → 서버 GPU → 브라우저 이식 가능.

### 실전 에이전트 예시
1. **공장 품질검사**: 컨베이어 카메라 → 결함 검출 → LLM이 원인/공정조정 리포트 → Slack
2. **매장 분석**: CCTV → 사람 추적 → 동선 히트맵 → 일 1회 LLM 진열 개선안 → 메일
3. **개인 비서**: 사진 폴더 분류(흐림/중복/영수증) → 영수증 OCR → LLM 가계부 → 노션

> **결론:** OpenCV = 에이전트의 감각기관 + 비용 절감 필터. LLM이 두뇌라면 OpenCV는 눈·귀.

---

## 9. React / PHP 구현 방법

### ⚛️ React

#### 길 1: `opencv.js` (WASM) — 브라우저 직접 실행 ⭐추천
```bash
npm install @techstark/opencv-js
```
```jsx
import { useEffect, useRef, useState } from 'react';
import cv from '@techstark/opencv-js';

export default function ImageEditor() {
  const canvasRef = useRef(null);
  const [ready, setReady] = useState(false);

  useEffect(() => { cv.onRuntimeInitialized = () => setReady(true); }, []);

  const handleFile = (e) => {
    const img = new Image();
    img.onload = () => {
      const src = cv.imread(img);
      const dst = new cv.Mat();
      try {
        cv.cvtColor(src, dst, cv.COLOR_RGBA2GRAY);
        cv.Canny(dst, dst, 50, 150);
        cv.imshow(canvasRef.current, dst);
      } finally {
        src.delete(); dst.delete();   // ⚠️ 수동 해제 필수 (메모리 누수 방지)
      }
    };
    img.src = URL.createObjectURL(e.target.files[0]);
  };

  return (
    <div>
      {!ready && <p>OpenCV 로딩중...</p>}
      <input type="file" accept="image/*" onChange={handleFile} disabled={!ready} />
      <canvas ref={canvasRef} />
    </div>
  );
}
```
**장점:** 서버 업로드 없음(개인정보 안전) · 서버비 0원 · 즉각 반응 · 트래픽 무관
**단점/주의:** 초기 WASM 5~10MB(지연 로딩 필요) · `.delete()` 누락 시 메모리 누수 ·
무거운 처리는 **Web Worker 분리 필수** · 일부 모듈만 노출(`platforms/js/opencv_js.config.py`로 커스텀 빌드 가능)

#### 길 2: React + Python FastAPI (무거운 작업)
```python
from fastapi import FastAPI, UploadFile
import cv2 as cv, numpy as np

app = FastAPI()

@app.post("/detect")
async def detect(file: UploadFile):
    data = np.frombuffer(await file.read(), np.uint8)
    img = cv.imdecode(data, cv.IMREAD_COLOR)
    # YOLO 추론 등
    return {"objects": [...]}
```

#### 하이브리드 (실무 베스트)
```
가벼운 처리(리사이즈·필터·크롭·엣지) → opencv.js (무료 티어)
무거운 처리(YOLO·SAM·동영상)        → 서버 API (유료 티어)
→ 프리미엄 모델의 자연스러운 경계선
```

### 🐘 PHP

| 패턴 | 추천도 | 설명 |
|---|---|---|
| **PHP + Python 마이크로서비스** | ⭐⭐⭐⭐⭐ | PHP=웹/결제/DB, Python=비전. HTTP로 연결 |
| **PHP + 큐 + 워커** | ⭐⭐⭐⭐ | Redis/RabbitMQ → Python 워커. 프로덕션급 |
| **PHP + opencv.js** | ⭐⭐⭐⭐ | 서버에 OpenCV 불필요. 최소비용 MVP |
| PHP `exec()` 로 Python 호출 | ⭐⭐ | 간단하지만 동시요청 취약. `escapeshellarg()` 필수 🚨 |
| `php-opencv` 확장 | ⭐ | 비추천 (유지보수 불안정, 공유호스팅 불가) |

```php
<?php
// 패턴 1: Laravel → Python 비전 서비스
$response = Http::attach('file', file_get_contents($path), 'img.jpg')
    ->timeout(60)
    ->post('http://vision-service:8000/detect');
$result = $response->json();
```
```php
<?php
// 패턴 4: exec 호출 — 보안 주의
$in  = escapeshellarg($uploadPath);
$out = escapeshellarg($resultPath);
exec("python3 /app/process.py {$in} {$out} 2>&1", $output, $code);
if ($code !== 0) { throw new Exception('처리 실패: '.implode("\n", $output)); }
```

---

## 10. 유튜브 강의 제작 가능성

### 결론: **가능하고, 한국어 시장은 블루오션** ⭐⭐⭐⭐⭐

### 좋은 소재인 이유
1. **결과가 눈에 보인다** — 썸네일 임팩트 최강, 이탈률 최저
2. **수요 꾸준 / 한국어 공급 부족** — 특히 5.x 신기능, `dnn` LLM/VLM 추론
3. **난이도 계단이 자연스러움** — 입문 → YOLO → SLAM/CUDA
4. **콘텐츠 재활용** — 영상 → 블로그 → 전자책 → 유료강의 → 외주
5. **라이선스 안전** — Apache 2.0 + 공식문서 CC (단, 샘플 이미지 저작권 확인)

### 커리큘럼

#### 시즌 1: 입문 (10편, 8~12분)
설치/첫 이미지 · 이미지는 숫자다(Mat & numpy) · **BGR 함정(입문자 90%가 겪는 버그)** ·
리사이즈/크롭/회전 · 블러/샤픈/필터 · Canny 엣지 · 이진화 & 윤곽선(동전 개수 세기) ·
**웹캠 실시간 처리(5줄)** · 마우스/트랙바 GUI · 🏆 **문서 스캐너 프로젝트**

#### 시즌 2: 실전 프로젝트 (8편, 15~20분)
QR/바코드 출입 시스템 · **얼굴 자동 모자이크** · 사람 수 카운터 · 번호판 영역 검출 ·
파노라마 합성 · **배경 제거** · 물체 추적 · 🏆 **캘리브레이션 + AR 큐브**

#### 시즌 3: AI 비전 (`dnn`) — 조회수 핵심 🔥
dnn 원리 & ONNX · **YOLO 실시간 검출** · **SAM 세그멘테이션** · **Super Resolution** ·
**Inpainting(물체 지우기)** · 흑백사진 컬러 복원 · 가상 피팅 ·
**OpenCV로 LLM 돌리기(Gemma3/Qwen)** · **VLM 이미지 설명** ·
🏆 **음성인식 + VLM + TTS = 멀티모달 에이전트**

#### 시즌 4: 고급 (조회수 적지만 외주 유입 💰)
소스 빌드 완전정복 · CUDA 가속 벤치마크 · **opencv.js 웹앱(React)** ·
안드로이드 통합 · 라즈베리파이/Jetson 배포 · **Visual SLAM** ·
**OpenCV MCP 서버 — Claude에 눈 달기** · OpenCV 컨트리뷰터 되기

### 조회수 기대 Top 5
| 순위 | 소재 | 이유 |
|---|---|---|
| 🥇 | 사진에서 물체 지우기 (Inpainting) | Before/After 임팩트 |
| 🥈 | 저화질 → 4K 업스케일링 | 비교 영상 자체가 콘텐츠 |
| 🥉 | 5줄로 실시간 웹캠 필터 | 진입장벽 파괴 |
| 4 | OpenCV MCP로 Claude에 눈 달기 | 트렌드 + 한국어 선점 |
| 5 | 흑백 가족사진 컬러 복원 | 감동 서사 → 공유 유발 |

### 수익 구조
```
유튜브 (무료, 신뢰 쌓기)
├─ 💵 애드센스          월 10~50만
├─ 📚 인프런/유데미      월 50~300만 ⭐
├─ 📖 전자책/템플릿      월 20~100만
├─ 🛠️ 소스 멤버십        월 30~150만 ⭐
├─ 🏢 기업 외주·컨설팅   건당 300~3,000만 ⭐⭐⭐
└─ 🤝 기업 강의          시간당 10~30만 ⭐⭐
```
> 개발 유튜버 수익의 대부분은 애드센스가 아닌 **외주/강의**. 유튜브는 포트폴리오.

### 제작 실무 팁
| 항목 | 팁 |
|---|---|
| 장비 | 마이크 > 카메라 (음질이 이탈률 1순위) |
| 녹화 | OBS Studio, 폰트 16pt↑, 다크테마 |
| 길이 | 입문 8~12분 / 프로젝트 15~25분, **첫 15초에 결과 먼저** |
| 썸네일 | Before/After 반반 분할 |
| 설명란 | GitHub 링크 필수 (포크 `samples/` 가 교재) |
| 확장 | 영어 자막 → 조회수 3~5배 |
| 주의 | 모델 라이선스 언급(YOLOv8=AGPL), 샘플 이미지 저작권 |

### 포크를 교재로 쓰는 전략
```
github.com/bmshin94/opencv
├ lecture/01-basics/
├ lecture/02-yolo/
├ lecture/03-mcp-server/
└ README.md   ← 커리큘럼 + 유튜브 링크
→ Star 가 신뢰 자산이 된다
```

---

## 11. 수익화 아이디어 10선

### 전략적 근거
```
① 🔒 로컬 처리 = 개인정보 안전 → 클라우드 AI 진입 불가 시장 (병원/공장/공공/금융)
② 💸 GPU 없이 추론 → 서버비 1/10 → 가격 경쟁력
③ 🤖 AI 에이전트에 "눈"이 없다 → 빈 틈새, 선점 가능
```

### 🥇 1. OpenCV MCP 서버 / 에이전트 비전 툴킷
**난이도 ⭐⭐ | 수익성 ⭐⭐⭐⭐⭐ | 선점성 ⭐⭐⭐⭐⭐**

현재 LLM 에이전트는 이미지를 "보기"는 하지만 **측정·검증·픽셀 단위 비교·가공**은 못 한다. 그 빈칸.

```
[무료 OSS]                        [유료 Pro]
image_info / resize / crop        배치 처리 (1000장)
read_qr / read_barcode            YOLO·SAM 고급 모델
detect_faces (YuNet)              커스텀 모델 학습·배포
compare_images (유사도)            동영상 파이프라인
ocr_regions                       클라우드 호스팅
auto_crop_document                팀 워크스페이스 + SLA
```

| 채널 | 가격 |
|---|---|
| 무료 OSS | $0 (유입·신뢰, Star = 영업력) |
| Pro 개인 | $9~19/월 |
| Team | $49~99/월 |
| Enterprise (온프레미스) | $500~5,000/월 ⭐⭐⭐ |
| 커스텀 툴 외주 | 건당 300~2,000만원 💰 |

**로드맵:** 1주 MVP 공개 → 2주 커뮤니티 홍보(r/mcp, HN) → 1개월 유튜브 영상 →
2개월 Pro 오픈 → 3개월 기업 온프레미스 계약
**리스크:** MCP 생태계 아직 작음(6~12개월 소요) · 대형사 유사 제품 → **도메인 특화로 방어**

### 🥈 2. B2B 업종특화 비전 SaaS
**난이도 ⭐⭐⭐⭐ | 수익성 ⭐⭐⭐⭐⭐ | 단가 최고**

> 핵심 전략: **범용 금지, 한 업종만 깊게.** "AI 비전 플랫폼"은 대기업과 경쟁.
> "치과 X-ray 충치 영역 표시" 처럼 좁게 파면 경쟁자 없음.

| 업종 | 솔루션 | 월 단가 |
|---|---|---|
| 🏭 제조 | 불량 검사 (스크래치·치수·누락) | 100~500만 |
| 💊 제약 | 알약 개수/색상 검증 | 200~800만 |
| 🌾 농업 | 과일 선별·숙도 판정 | 50~200만 |
| 🏗️ 건설 | 안전모 미착용 감지 (중대재해법) | 50~300만 |
| 🏪 리테일 | 선반 결품, 방문객 동선 | 30~150만 |
| 🅿️ 주차 | 번호판 무인정산 | 30~100만 |
| 🐟 수산 | 어종 분류·크기 측정 | 50~200만 |
| 🏥 의료 | 세포 카운팅, 촬영 QC | 100~500만 |
| 🚚 물류 | 적재 검증, 송장 OCR | 50~300만 |
| ♻️ 재활용 | 폐기물 분류 | 100~400만 |

```
수익 구조
초기 구축비     : 500만 ~ 5,000만원
월 구독료       : 30만 ~ 500만원/라인
+ 라인 확장 과금  ← 복리 성장 지점
+ 하드웨어 마진 (카메라/조명/PC)
+ 연간 유지보수 (구축비의 10~15%)
→ 고객 10곳 × 월 100만 = 월 1,000만원
```
**진입 경로:** 지인 사업장 무료 PoC → 수치 포트폴리오("불량률 3%→0.4%") →
업종 커뮤니티/스마트공장 엑스포 영업 → **정부 스마트공장 보급사업(최대 1.5억 지원)** 연계
**리스크:** 영업 사이클 3~12개월, 현장 조명 변수 → **조명·거치대 패키지 제공으로 환경 통제**

### 🥉 3. 브라우저 완결형 웹툴 (opencv.js)
**난이도 ⭐⭐ | 수익성 ⭐⭐⭐⭐ | 서버비 0원**

> 핵심 메시지: **"당신의 사진은 서버에 업로드되지 않습니다"** → 전환율 2~3배

| 툴 | 수요 |
|---|---|
| 문서 스캐너 (사진→PDF) | ⭐⭐⭐⭐⭐ |
| 여권/증명사진 규격 자동 맞춤 | ⭐⭐⭐⭐⭐ |
| 얼굴 자동 모자이크 (일괄) | ⭐⭐⭐⭐⭐ |
| 배경 제거 | ⭐⭐⭐⭐⭐ |
| 일괄 리사이즈/포맷변환 | ⭐⭐⭐⭐ |
| 중복/흐린 사진 걸러내기 | ⭐⭐⭐⭐ |
| 영수증 펴기 + 분할 | ⭐⭐⭐⭐ |
| 흑백→컬러 (dnn+WASM) | ⭐⭐⭐⭐ |
| 동영상→GIF / 썸네일 | ⭐⭐⭐⭐ |
| 워터마크 일괄 삽입 | ⭐⭐⭐⭐ |
| QR 리더/생성 · 팔레트 추출 · 파노라마 · 치수 측정 | ⭐⭐⭐ |

```
🆓 무료    : 1장씩, 워터마크, 1080p 제한 (브라우저 처리 → 서버비 0)
💳 Pro     : $4.9~9.9/월 — 일괄 100장, 워터마크 제거, 원본 해상도, 프리셋
🏢 Team/API: $29~99/월 — REST API, 무거운 AI는 서버 처리
💰 부가    : 애드센스, Gumroad 소스 번들 $29~99
```
**실행:** Next.js + `@techstark/opencv-js` → Vercel 무료 배포 → **SEO 롱테일 키워드**
("무료 문서 스캐너", "여권사진 규격") → 툴 5개로 내부링크 → Pro 오픈
**주의:** Web Worker 분리 · `Mat.delete()` 철저 · 지연 로딩

### 🏅 4. 교육 콘텐츠 제국
**난이도 ⭐⭐ | 수익성 ⭐⭐⭐ | 브랜딩 ⭐⭐⭐⭐⭐**

> 유튜브는 상품이 아니라 **영업사원**.

| 라인 | 가격 | 예상 |
|---|---|---|
| 애드센스 | - | 월 10~50만 |
| 인프런 | 5~15만/인 | 월 50~300만 |
| 유데미 (영어) | $20~80 | 월 30~200만 |
| 전자책/노션 | 1~3만 | 월 20~80만 |
| 멤버십 (소스+Q&A) | 월 1~3만 | 월 30~150만 |
| 기업 출강 | 시간당 10~30만 | 건당 100~500만 |
| 외주 개발 | - | 300~3,000만 |

**차별화 (한국어 빈 틈):** OpenCV 5.x LLM/VLM 로컬 추론 · OpenCV MCP ·
서버비 0원 WASM 웹서비스 · 제조 품질검사 실무 · 라즈베리파이 엣지 AI

### 🏅 5. 온디바이스 모바일 앱
**난이도 ⭐⭐⭐ | 수익성 ⭐⭐⭐⭐ | 세일즈 포인트: "비행기 모드에서도 작동"**

| 앱 | 수익 모델 |
|---|---|
| 오프라인 문서 스캐너 (PDF) | 월 2,900원 |
| 증명사진 메이커 (국가별 규격) | 건당 1,900원 / 구독 |
| 얼굴 모자이크 (영상 포함) | 광고 + Pro |
| AR 치수 측정 (ArUco) | 유료 4,900원 |
| 영수증 가계부 (로컬 OCR) | 구독 3,900원 |
| 사진 정리 (흐림/중복 탐지) | Pro 일괄정리 |
| 옷장 코디 (색상 매칭) | 광고 + 제휴 |
| 재고 카운터 (사진→개수) | B2B 구독 |

**스택:** Flutter + `opencv_dart` (권장) / React Native + 네이티브 모듈 /
Android 네이티브 + OpenCV SDK (`platforms/android` 활용)
**리스크:** 스토어 수수료 30%(소규모 15%), 경쟁 → **틈새 + 로컬 처리 + 국가 규격 특화**

### 추가 아이디어 5개
| # | 아이디어 | 난이도 | 수익성 | 포인트 |
|---|---|---|---|---|
| 6 | 크몽/업워크 외주 | ⭐⭐ | ⭐⭐⭐⭐ | **가장 빠른 현금화** (건당 50~500만) |
| 7 | 커스텀 모델 학습 서비스 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 건당 500~3,000만 |
| 8 | 데이터 라벨링 도구 SaaS | ⭐⭐⭐ | ⭐⭐⭐ | SAM 활용 반자동 라벨링 |
| 9 | n8n/Make 비전 노드 | ⭐⭐ | ⭐⭐⭐ | 노코드 시장, 경쟁 적음 |
| 10 | Docker 이미지 + 보일러플레이트 | ⭐ | ⭐⭐ | Gumroad $49 |

---

## 12. 추천 실행 전략 (3단 로켓)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 1단계 (0~3개월) — 신뢰 자산 구축 (수익 기대 X)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
① OpenCV MCP 서버 오픈소스 공개
② 유튜브 입문 시리즈 10편
③ 웹툴 1개 Vercel 배포 (문서스캐너 / 증명사진)
   목표: Star + 구독자 + SEO 유입

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💰 2단계 (3~9개월) — 첫 현금흐름
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
④ 크몽/업워크 외주 시작 (즉시 현금)
⑤ 웹툴 Pro 티어 오픈
⑥ 인프런 유료강의 1개 출시
⑦ MCP Pro 티어 오픈
   → 1단계 자산이 전부 영업 도구로 작동

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🏆 3단계 (9~24개월) — 고단가 B2B 전환
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⑧ 외주로 만난 고객 중 한 업종 선택 → SaaS화
⑨ 정부 스마트공장 지원사업 연계 (최대 1.5억)
⑩ 월 구독 10곳 = 월 1,000만원 안정 수익
```

### 선순환 구조
```
무료 공개 → 신뢰 → 외주 문의 → 현장 문제 파악 → SaaS 제품화 → 구독 수익
    ↑                                                    ↓
    └──────── 실제 사례가 다시 콘텐츠가 되는 선순환 ────────┘
```
> 1단계를 건너뛰고 바로 영업하면 성사율이 낮다. 1단계 자산을 쌓으면 고객이 먼저 찾아온다.

---

## 13. 법적 체크리스트

```
✅ OpenCV 본체 Apache 2.0 → 상업적 이용 OK (고지문구 포함)

⚠️ AI 모델 라이선스는 별도!
   - YOLOv8 / YOLOv5 = AGPL-3.0 → 상업용은 Ultralytics 유료 라이선스 필요
   - 상업 안전 대안: YOLOX, RT-DETR, SSD-MobileNet, NanoDet (Apache/MIT)

⚠️ FFmpeg = LGPL/GPL → 상용 배포 시 LGPL 빌드 또는 WITH_FFMPEG=OFF 검토

⚠️ opencv-contrib 일부 알고리즘 특허 이력 확인 (SIFT는 2020년 특허 만료 → 자유)

⚠️ 얼굴/번호판 처리 = 개인정보보호법 대상
   → "로컬 처리, 저장하지 않음" 명시가 오히려 세일즈 포인트

⚠️ 의료 진단 용도 = 의료기기 인증 필요 → "보조 도구"로 포지셔닝

⚠️ 샘플 이미지 저작권: lena.jpg 는 이슈 있음 → baboon.jpg 또는 직접 촬영 사용
```

---

## 14. 참고 링크

### 이 분석의 대상 리포지토리
- 🔗 **오빠 포크: https://github.com/bmshin94/opencv**
- 🔗 작업 브랜치: https://github.com/bmshin94/opencv/tree/claude/elegant-hopper-3c5qnd
- 🔗 PR #1 (CLAUDE.md 추가): https://github.com/bmshin94/opencv/pull/1

### OpenCV 공식
- 원본 리포: https://github.com/opencv/opencv
- 추가 모듈(contrib): https://github.com/opencv/opencv_contrib
- 홈페이지: https://opencv.org
- 공식 문서(5.x): https://docs.opencv.org/5.x/
- 강의: https://opencv.org/courses
- Q&A 포럼: https://forum.opencv.org
- 이슈 트래커: https://github.com/opencv/opencv/issues
- 기여 가이드: https://github.com/opencv/opencv/wiki/How_to_contribute
- 코딩 스타일: https://github.com/opencv/opencv/wiki/Coding_Style_Guide
- 유튜브 채널: https://youtube.com/@opencvofficial
- OpenCV.ai (상용 서비스): https://opencv.ai

### 생태계 / 도구
- 모델 동물원: https://github.com/opencv/opencv_zoo
- opencv-python (pip): https://github.com/opencv/opencv-python
- opencv.js (npm): https://www.npmjs.com/package/@techstark/opencv-js
- ONNX Model Zoo: https://github.com/onnx/models
- Hugging Face Models: https://huggingface.co/models
- Gemma3 (샘플에서 사용): https://huggingface.co/google/gemma-3-1b-it
- MCP 공식 문서: https://modelcontextprotocol.io
- Claude Code 문서: https://code.claude.com/docs

### 포크 내 핵심 경로 (교재로 활용)
```
samples/python/          — 입문 예제 34개
samples/cpp/             — C++ 예제 52개
samples/dnn/             — AI 추론 예제 (+ models.yml, download_models.py)
samples/slam/            — Visual Odometry
doc/tutorials/           — 공식 튜토리얼 원본
apps/                    — 즉시 사용 가능한 완성 도구
platforms/js/            — WebAssembly 빌드 스크립트
platforms/android/       — Android SDK 빌드 스크립트
modules/dnn/include/opencv2/dnn/dnn.hpp  — 백엔드/타겟 목록
CMakeLists.txt           — 전체 빌드 옵션
```

---

## 🎯 최종 요약

> **오빠는 지금, 세계 최고 영상 AI 공구함의 설계도 전체(255MB, C++ 1,650 파일) +
> 즉시 실행 가능한 완성 레시피 100개 이상 +
> 보고·듣고·말하고·추론하는 멀티모달 AI 부품 세트를
> 상업적으로 자유롭게 쓸 수 있는 Apache 2.0 라이선스로 손에 쥐고 있다.**

### 지금 당장 할 일 3가지
```bash
# 1. 설치
pip install opencv-python numpy

# 2. 예제 실행 (QR 리더 완성)
cd samples/python && python qrcode.py

# 3. AI 예제로 점프 (실시간 객체검출 완성)
cd ../dnn && python download_models.py --model yolov8
python object_detection.py --model yolov8
```

---

*이 문서는 Claude Code(카리나 페르소나)가 `bmshin94/opencv` 리포지토리를 전수조사하여 작성했습니다. 💖*
