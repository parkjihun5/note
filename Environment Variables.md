## 환경변수
환경변수는 운영체제 수준에서 정의되는 동적 변수로, 소프트웨어 실행 시 참조되는 설정 값을 저장
보안이 필요한 인증 정보나 실행 환경에 따라 변하는 설정 값을 코드 외부에서 관리하기 위해 사용

### 환경변수 사용 목적
보안성 확보
    데이터베이스 비밀번호, API 인증 키 등 민감한 정보가 소스 코드에 직접 노툴되는 것을 방지
환경 독립성
    동일한 코드 내에서 개발, 테스트, 운영 환경에 따라 다른 설정값(데이터베이스 주소, 포트 번호 등)을 주입할 수 있음
유지보수 효율
    설정 변경 시 코드를 수정하거나 재컴파일하지 않고 환경변수 값만 변경하여 즉시 반영 가능

### 파이썬 환경변수 조작 방법
파이썬 표준 라이브러리 os 모듈을 사용하여 시스템 환경변수에 접근하고 제어

### python-dotenv
.env 파일에 키-값 쌍으로 환경변수를 정의하고 이를 프로세스의 환경변수로 로드하는 방식
외부 라이브러리인 python-dotenv 설치가 필요

.env 파일 구성 예시
    등호의 앞뒤로 공백을 넣지 않음
DEBUG=True
SECRET_KEY=123123123123
PORT=123123123

.env 파일을 로드하여 사용하는 코드
import os
from dotenv import load_dotenv

# .env 파일의 내용을 환경변수로 로드
load_dotenv()

# 로드된 환경변수 참조
debug_mode = os.getenv('DEBUG')
secret_key = os.getenv('SECRET_KEY')
server_port = os.getenv('PORT')

print(f"DEBUG: {debug_mode}")
print(f"SECRET_KEY: {secret_key}")
print(f"PORT: {server_port}")

### 환경변수 보안 및 저장소 관리
.env 파일에는 보안이 필요한 중요 정보가 포함되므로 git과 같은 버전 관리 시스템에 업로드 되지 않도록 차단해야 한다

### 프로젝트 루트(Root) 디렉토리에 위치
.env 파일은 프로젝트 최상단 디렉토리에 위치
환경변수를 다루는 library들은 별도의 설정이 없으면 최상위 디렉토리를 기준으로 작동

### .gitignore 설정
.gitignore 파일은 git 추적에서 제외할 파일 목록을 지정하는 설정 파일
.env 파일명을 .gitignore에 명시하여 원격 저장소 유출을 방지