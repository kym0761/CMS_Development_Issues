# CMS 개발 이슈

## Zero Density 플러그인 개발 이슈

Zero Density는 C++ 빌드가 안된다. 문제는 C++ 플러그인을 꼭 써야한다.
방법은 다음과 같다.
### 1 언리얼 엔진으로 플러그인과 함께 프로젝트를 빌드한다.
Zero Density와 같은 버전의 언리얼 엔진을 설치하고 플러그인을 해당 언리얼 엔진으로 빌드한다.

### 2 Zero Density 설치 경로에 넣어 준다.
Zero Density 설치 경로 C:\Program Files\Zero Density\Reality\[Version Name]\Engine\Plugins
에 들어간다.

### 3 플러그인의 모듈 ID를 바꿔준다.
Zero Density 경로의 Marketplace 플러그인의
Marketplace\Substance\Binaries\Win64 경로로 들어가 
UE4Editor.modules 파일에 접근하여 Build ID를 복사하고
개발한 플러그인에도 같은 경로에 들어가 UE4Editor.modules에 복사한 ID를 붙여넣는다.