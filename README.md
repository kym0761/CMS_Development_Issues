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

### 이유
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
가장 효과적인 방법은 텍스쳐의 LOD Bias의 값을 건드려 텍스쳐 품질을 에디터에서 조절하는 것이다.

### 4. 액터의 행동들을 매우 느린 수준을 유지하거나 혹은 움직이지 않게 한다.
무슨 말이냐면, 초당 0.01cm 움직이는 정도로 매우 느리게 움직인다면 점프가 발생해도 움직이는 정도가 매우 낮으므로 점프 현상이 일어나도 우리 눈으로는 알 수가 없다는 뜻이다.
카메라가 움직일 일도 없고, 움직이는 액터가 없거나 혹은 매우 느린 움직임이면 점프 현상은 별 상관 없을지도 모른다. 다만 이는 해결 방법은 아니고 문제를 무시하는 방법이니 좋은 선택은 아니다.

## 마우스가 뷰포트에 먹히는 현상 해결법

마우스를 뷰포트를 클릭하여 먹히는 현상은, InputGameOnlyMode 등의 이유로 마우스가 인게임에 먹히는 현상이다.
이는 당연히 인풋 모드를 바꾸거나 PlayerController의 Show Mouse Cursor를 건드리면 그만이다.
가장 쉬운 방법은 PlayerController를 새로 만드는 것인데 Zero Density는 ZDGameMode에서만 올바르게 동작한다.
즉, PlayerController를 새로 만들어서 쓰려면 GameMode를 만들어 지정해야 하는데,
Zero Density에서 사용하려면, ZDGameMode를 상속받은 GameMode를 만들어야 하는 것을 잊지 말아야한다.

## Pixotope에서 플러그인 사용법

Pixotope도 언리얼 엔진을 커스텀한 버추얼 프로덕션 프로그램이다.
Zero Density처럼 플러그인을 따로 만들어야하는데, Pixotope는 C++ 빌드가 가능하다.
윈도우즈 10 환경 기준, Pixotope와 Visual Studio가 설치되어 있어야한다.
Visual Studio는 .NET, C++ 데스크톱 개발, C++ 게임 개발 워크로드가 총 3가지 설치되어 있어야 한다.
 
Pixotope SDK는 Pixotope 런처에서 다운받을 수가 있다.
Pixotope SDK 버전은 Pixotope와 맞춰야 한다. 버전은 런처에서 Help 들어가면 확인할 수 있다.

SDK는 6기가 Zip 파일인데, 압축을 풀면 25기가 정도 되는 큰 SDK다.
이 SDK를 Pixotope Editor 폴더를 찾아서 붙여넣으면 된다.

그 후, Pixotope Editor\Engine\Binaries\Win64\UE4Editor.exe 를 실행하여 C++ 프로젝트를 만들고 플러그인을 Pixotope 버전으로 빌드하면 된다.
사실 언리얼 바닐라 버전 플러그인도 동작은 하는데, 혹시나 생길 오류의 가능성 때문에 Pixotope에서 빌드를 하는 것을 추천한다.


