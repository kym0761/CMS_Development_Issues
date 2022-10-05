# CMS 개발 이슈

## Zero Density 플러그인 개발 이슈

Zero Density는 C++ 빌드가 안된다. 문제는 C++ 플러그인을 꼭 써야한다.
방법은 다음과 같다.

### 1. 언리얼 엔진으로 플러그인과 함께 프로젝트를 빌드한다.
Zero Density와 같은 버전의 언리얼 엔진을 설치하고 플러그인을 해당 언리얼 엔진으로 빌드한다.

### 2. Zero Density 설치 경로에 넣어 준다.
Zero Density 설치 경로 C:\Program Files\Zero Density\Reality\[Version Name]\Engine\Plugins
에 들어간다.

### 3. 플러그인의 모듈 ID를 바꿔준다.
Zero Density 플러그인 안에 있는 Marketplace 플러그인의 Marketplace\Substance\Binaries\Win64 의 
UE4Editor.modules 파일에 접근하여 Build ID를 복사하고 개발한 플러그인에도 같은 경로에 들어가 UE4Editor.modules에 복사한 ID를 붙여넣는다.

## 점프 현상 이슈

Zero Density에서 Camera Actor를 움직였을 시에, 플레이 중간에 점프 현상이 일어나 마치 카메라가 순간이동하는 모습이 보이는 이슈.
이유는 다음과 같다.
현실에서 사용하는 촬영용 카메라의 주사율이 60fps일 때, Zero Density Reality Editor의 fps가 60 미만일 시 일어나는 현상이다.
엔진에서 현실의 카메라와 싱크를 맞춰주기 위해서, 인게임 플레이의 무결성을 포기하고 점프 현상으로 차이난 시간을 보정하는 것으로 추정된다.
문제는, 점프가 일어나는 동안 Input을 받지 않은 상태인데도 Input을 받은 듯하게 행동하므로 보는 사람 입장에서는 비정상적인 수준으로 이동한 걸로 보이게 된다.

해결 방안은 다음과 같다.

### 1. 컴퓨터를 바꾼다.
더 좋은 컴퓨터를 사용한다. 더 좋은 컴퓨터에서 현실 카메라 주사율과 똑같은 수준의 fps를 유지한다면 문제가 해결된다.

### 2. 현실 카메라의 주사율을 낮춘다.
만약, Editor의 fps가 40정도가 나오는 정도라면, 현실의 카메라를 30fps로 바꾸면 Editor에서 30 고정 fps를 충분히 줄 수 있으므로 해결된다.

### 3. 최적화를 진행한다.
여기서 말하는 최적화는 코드 최적화가 아니라 렌더링 최적화다.
텍스쳐 품질을 줄이거나, 라이트 무버블을 스태틱으로 바꾸거나, 혹은 메쉬의 폴리곤 갯수를 줄이거나 하는 등의 작업을 한다.

