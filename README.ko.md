# Twitter Skill

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md)

Codex, Claude Code 및 기타 AI Agent에서 사용자의 로컬 X/Twitter 로그인 세션으로 X를 사용할 수 있습니다.

Twitter Skill은 Windows/macOS 데스크톱 앱, JSON CLI, 표준 Agent Skill로 구성됩니다. Agent는 X 검색, 여러 연결 계정 사용, 이미지 업로드, 게시물 작성과 답글, 게시물 삭제를 수행할 수 있으며 사용자의 X Cookie를 전달받지 않습니다.

## 기능

| 기능 | 설명 |
| --- | --- |
| 검색 | X 검색 연산자, 결과 수, 결과 모드 및 커서 페이지네이션 지원 |
| 계정 | 연결된 계정 목록 조회, 기본 계정 또는 지정 계정 사용 |
| 게시 | 텍스트 게시물, 답글 및 최대 4개의 이미지가 포함된 게시물 작성 |
| 미디어 | JPEG, PNG 및 WebP 이미지 업로드 |
| 관리 | 게시물 삭제 및 로컬 계정 상태 확인 |
| Agent 연동 | Codex, Claude Code, Cursor 및 `skills` CLI가 지원하는 Agent에 동일한 표준 Skill 설치 |

## 요구 사항

1. [GitHub Releases](https://github.com/chadyi/twitter-skill-releases/releases/latest)에서 최신 Twitter Skill 데스크톱 앱을 설치합니다.
2. 앱을 열고 하나 이상의 X 계정을 연결한 뒤 시스템 트레이에서 계속 실행합니다.
3. `npx`를 사용할 수 있도록 [Node.js](https://nodejs.org/)를 설치합니다.

## Agent Skill 설치

아래 방법 중 하나를 선택하세요. 두 방법 모두 동일한 Skill을 전역으로 설치합니다.

### 직접 실행

터미널에서 다음 명령을 실행합니다.

```bash
npx skills add chadyi/twitter-skill-releases -s twitter-skill -g -y
```

### Agent에게 설치 요청

아래 메시지를 Codex, Claude Code 또는 지원되는 Agent에 복사하세요.

```text
https://github.com/chadyi/twitter-skill-releases 에서 Twitter Skill을 전역으로 설치하세요. 다음 명령을 실행하세요.
npx skills add chadyi/twitter-skill-releases -s twitter-skill -g -y

설치 후 twitter-skill이 설치되었는지 확인하고 SKILL.md를 읽은 다음, 로컬 Twitter Skill 데스크톱 앱이 준비되었는지 확인하세요. X Cookie를 요청하거나 출력하거나 저장하지 마세요.
```

수동 확인:

```bash
npx skills list -g
```

나중에 업데이트:

```bash
npx skills update twitter-skill -g
```

## 사용 방법

설치 후 Agent에게 자연어로 요청하세요.

```text
X에서 GPT Image 2에 관한 최신 게시물 20개를 검색해 주세요.
```

```text
내 기본 X 계정으로 이 이미지를 첨부하고 "hello"를 게시해 주세요.
```

```text
이 X 게시물에 짧은 요약으로 답글을 작성해 주세요.
```

Agent는 Skill을 읽고 로컬 CLI를 호출합니다. CLI는 결과와 오류를 구조화된 JSON으로 반환합니다.

## 작동 방식

```text
Codex / Claude Code / 기타 Agent
  -> 설치된 twitter-skill/SKILL.md
  -> 로컬 Twitter Skill CLI
  -> 실행 중인 데스크톱 트레이 프로세스
  -> 서버에서 발급한 단기 요청 계획
  -> 사용자의 로컬 Electron 세션과 네트워크에서 X 요청 실행
```

- X 로그인 상태는 데스크톱 앱의 분리된 로컬 계정 파티션에 저장됩니다.
- X 요청은 공유 서버 IP가 아니라 사용자의 장치에서 전송됩니다.
- Skill에는 CLI 사용 지침만 포함되며 Cookie 또는 비공개 X 요청 구현은 포함되지 않습니다.
- 공개 저장소에는 Agent Skill과 릴리스 파일만 있습니다. 앱과 서버 소스 코드는 비공개로 유지됩니다.

Twitter Skill은 사용자가 로그인한 X 웹 세션을 사용하며 X 공식 API는 사용하지 않습니다.

## 데스크톱 다운로드 및 업데이트

최신 버전은 [Releases](https://github.com/chadyi/twitter-skill-releases/releases/latest)에서 다운로드하세요.

| 플랫폼 | 패키지 |
| --- | --- |
| Windows x64 | `Twitter-Skill-<version>-x64.exe` |
| macOS Universal | `Twitter-Skill-<version>-universal.dmg` |

각 Release에는 자동 업데이트 메타데이터, blockmap, macOS 업데이트용 ZIP 및 `SHA256SUMS.txt`도 포함됩니다. 데스크톱 앱은 이 공개 저장소에서 업데이트를 확인합니다.

## 문제 해결

### macOS에서 앱이 손상되어 열 수 없다고 표시됨

먼저 같은 Release의 `SHA256SUMS.txt`와 다운로드한 파일을 비교하세요. 앱을 `/Applications`로 이동한 다음 마우스 오른쪽 버튼으로 클릭하고 **열기**를 선택하세요. 최신 macOS에서는 **시스템 설정 > 개인정보 보호 및 보안**에서 **확인 없이 열기**를 선택할 수도 있습니다.

검증된 앱이 계속 손상되었다고 표시되면 이 앱의 격리 속성만 제거하세요.

```bash
sudo xattr -dr com.apple.quarantine "/Applications/Twitter Skill.app"
```

명령이 완료되면 앱을 다시 여세요. Gatekeeper를 시스템 전체에서 비활성화하지 마세요. 화면 예시와 추가 설명은 [macOS 문제 해결 요약](https://sysin.org/blog/macos-if-crashes-when-opening/)을 참고하세요.

### Agent가 `twitter-skill`을 찾지 못함

`npx skills list -g`를 실행하고 Skill을 다시 설치한 뒤 Agent를 재시작하여 전역 Skills를 다시 불러오세요.

### 로컬 CLI를 사용할 수 없음

Twitter Skill 데스크톱 앱을 한 번 열고 트레이에서 계속 실행하세요. 앱이 시작될 때 로컬 CLI 실행 파일이 생성됩니다.

### 로컬 X 로그인이 만료됨

데스크톱 앱을 열고 해당 계정을 다시 연결하세요. X Cookie를 Agent나 터미널에 붙여 넣지 마세요.

### 게시 요청 시간 초과

다시 시도하기 전에 X에서 계정과 게시물을 확인하세요. 시간 초과가 발생해도 게시가 이미 완료되었을 수 있습니다.

## 저장소 구성

```text
skills/twitter-skill/SKILL.md  표준 Agent Skill
GitHub Releases                Windows/macOS 설치 파일 및 업데이트 메타데이터
```

앱과 릴리스 문제는 [GitHub Issues](https://github.com/chadyi/twitter-skill-releases/issues)에 보고할 수 있습니다.
