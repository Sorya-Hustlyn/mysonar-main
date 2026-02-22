<p align="right">
  <a href="README.md">English</a> &nbsp;·&nbsp; <a href="README.ja.md">日本語</a>
</p>

<div align="center">

# MySonar

실시간 자막을 지원하는 데스크톱 오디오 플레이어.

<br>

![Windows](https://img.shields.io/badge/Windows-Release-0078D4?style=flat-square)&nbsp;&nbsp;![macOS](https://img.shields.io/badge/macOS-QA_Testing-lightgrey?style=flat-square)

<br>

**v1.0.1** &nbsp;·&nbsp; 출시일 2026. 02. 22 &nbsp;·&nbsp; 개발 Hustlyn

<br>

<img src="preview/icon.png" width="72">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="preview/snrpack_icon.png" width="72">

</div>

<br>

<video src="preview/demo.mp4" controls width="100%"></video>

![MySonar](preview/00-page/00-track.png)

---

## 지원 포맷

| 종류 | 포맷 |
|---|---|
| 오디오 | WAV · FLAC · MP3 · OGG · OPUS · AAC · M4A · AIFF · WebM |
| 자막 | SRT · VTT · ASS / SSA · SMI · LRC · TXT |

---

## 자막 파일 이름 규칙

자막 파일은 오디오 파일과 같은 폴더에 놓으면 자동으로 인식됩니다.

### RAW (언어 없음)

| 패턴 | 예시 (오디오: `rain.flac`) |
|---|---|
| `<파일명>.<자막확장자>` | `rain.srt` |
| `<파일명>.<오디오확장자>.<자막확장자>` | `rain.flac.srt` |

RAW 자막은 자막 선택기에서 **RAW** 로 표시됩니다.

### 언어 코드 포함

| 패턴 | 예시 (오디오: `rain.flac`) |
|---|---|
| `<파일명>.<언어>.<자막확장자>` | `rain.en.srt` · `rain.ja.vtt` |
| `<파일명>.<오디오확장자>.<언어>.<자막확장자>` | `rain.flac.ko.srt` |

`<언어>` 는 소문자 2–3자리 ISO 639-1 코드여야 합니다 (예: `en`, `ko`, `ja`, `fra`).

**예시 — `rain.flac` 폴더 구성:**

```
rain.flac
rain.srt            ← RAW
rain.en.srt         ← 영어
rain.ja.vtt         ← 일본어 (다른 포맷)
rain.ko.ass         ← 한국어
rain.flac.zh.srt    ← 중국어 (전체 파일명 형식)
```

하나의 트랙에 여러 언어와 포맷을 동시에 사용할 수 있습니다. 같은 언어에 여러 자막 파일이 있으면 언어 선택기 옆에 포맷 선택기(SRT / VTT / …)가 함께 표시됩니다.

---

## 패키지 포맷 (.snrpack)

`.snrpack` 파일은 컬렉션 전체(오디오, 자막, 커버 이미지, 메타데이터)를 하나의 파일로 묶어 공유하거나 백업하는 용도로 사용합니다. 내부 구조는 ZIP 아카이브입니다.

**내부 구조:**

```
collection.snrpack  (ZIP 아카이브)
├── manifest.json        포맷 버전 및 앱 식별 정보
├── collection.json      이름, 작성자, 설명, 태그, 트랙 목록, 그룹
├── audio/               오디오 파일 (탐색 지원을 위해 비압축 저장)
│   ├── 0/  song.flac
│   └── 1/  ...  (파일명 충돌 시 인덱스 증가)
├── images/              커버 이미지 (압축 저장)
│   └── 0/  cover.jpg
└── subtitles/           자막 파일, 오디오와 인덱스 일치 (압축 저장)
    └── 0/  song.en.srt
             song.ja.vtt
```

`collection.json` 에는 컬렉션 이름, 작성자, 설명, 태그, 커버 이미지 경로, 트랙 목록이 저장됩니다. 각 트랙 항목에는 오디오 메타데이터(제목, 아티스트, 앨범, 재생 시간, 포맷, 비트레이트 등), 별점, 태그, 그리고 언어 코드와 함께 연결된 자막 파일 목록이 포함됩니다. 트랙 그룹과 순서도 함께 저장됩니다.

**내보내기:** 컬렉션 우클릭 → **패키지 내보내기**
**가져오기:** `.snrpack` 파일을 앱 창에 드래그하거나, 컬렉션 우클릭 → **패키지 가져오기**

`.snrpack` 파일은 시스템에 커스텀 아이콘으로 등록됩니다. 파일을 더블클릭하거나 **연결 프로그램 → MySonar** 를 사용하면 바로 열 수 있습니다.

<table>
  <tr>
    <td width="50%" align="center"><img src="preview/03-packing/30_snrpack.png" width="100%"><br>파일 탐색기에서의 .snrpack 파일</td>
    <td width="50%" align="center"><img src="preview/03-packing/31_snrpack-open.png" width="100%"><br>더블클릭 또는 연결 프로그램으로 바로 열기</td>
  </tr>
</table>

---

## 기능

### 재생
- 실시간 분석기 오버레이가 적용된 파형 탐색 바
- 트랙 간 크로스페이드 (지속 시간 조절 가능)
- 파일별 마지막 재생 위치 기억

### 오디오 처리
- 10밴드 이퀄라이저 (32 Hz – 16 kHz) — 심플 / 프로 모드, 기본 프리셋 제공
- 다이나믹 컴프레서 및 저음 부스트
- 볼륨 노멀라이제이션 (ReplayGain) (BETA)
- 공간 음향 — HRTF 바이노럴 포지셔닝 (X / Y / Z 축)
- 모노 테스트 음원을 이용한 좌우 청력 밸런스 보정

### 자막
- 오버레이 표시 — 폰트, 크기, 색상, 외곽선, 그림자, 위치 조절 가능
- 현재 큐를 강조 표시하고 자동 스크롤되는 스크립트 패널
- 인라인 색상 태그 렌더링 (SRT / VTT / SMI)
- 이전 / 다음 자막 큐로 이동

### 플레이리스트 및 컬렉션
- 파일 또는 폴더를 창 어디에나 드래그 앤 드롭
- 다중 선택: Ctrl+클릭 · Shift+클릭 · 마우스 드래그
- 플레이리스트 내 접을 수 있는 트랙 그룹, 드래그로 순서 변경 가능
- 이름, 재생 시간, 파일 크기 기준 정렬 (자연 숫자 순)
- 컬렉션 — `.msc` 파일로 저장되는 이름 있는 플레이리스트
  - 컬렉션당 여러 커버 이미지, 드래그로 순서 변경 가능
  - 별점 (0–5) 및 커스텀 태그
  - 페이지당 트랙 수: 5 / 10 / 20
- 태그 필터 및 최소 별점 필터를 이용한 검색
- 전체 메타데이터를 확인할 수 있는 트랙 정보 모달

### 화면 및 UI
- 7가지 테마: Dark · Light · Dark Rose · Light Rose · Light Marine · Dark Marine · Pink
- 둥근 모서리의 투명 창
- 앨범 아트: 내장 메타데이터, 동일 이름 이미지 파일, 또는 드래그 앤 드롭으로 덮어쓰기
- 키 입력 시 화면에 표시되는 동작 오버레이
- 상태 표시줄 — 실시간 재생 정보

---

## 스크린샷

<table>
  <tr>
    <td align="center"><img src="preview/00-page/00-track.png"><br>트랙 화면</td>
    <td align="center"><img src="preview/00-page/01-collection.png"><br>컬렉션</td>
    <td align="center"><img src="preview/00-page/02-edit_collection.png"><br>컬렉션 편집</td>
  </tr>
  <tr>
    <td align="center"><img src="preview/00-page/03-import-srnpack.png"><br>패키지 가져오기</td>
    <td align="center"><img src="preview/00-page/04-eq.png"><br>이퀄라이저</td>
    <td align="center"><img src="preview/00-page/05-password.png"><br>비밀번호 잠금</td>
  </tr>
</table>

### 테마

<table>
  <tr>
    <td align="center"><img src="preview/02-theme/20_dark.png"><br>Dark</td>
    <td align="center"><img src="preview/02-theme/21_light.png"><br>Light</td>
    <td align="center"><img src="preview/02-theme/22_dark-rose.png"><br>Dark Rose</td>
    <td align="center"><img src="preview/02-theme/23_light-rose.png"><br>Light Rose</td>
  </tr>
  <tr>
    <td align="center"><img src="preview/02-theme/24_dark-marine.png"><br>Light Marine</td>
    <td align="center"><img src="preview/02-theme/25_dark-marine.png"><br>Dark Marine</td>
    <td align="center"><img src="preview/02-theme/26_pink.png"><br>Pink</td>
    <td></td>
  </tr>
</table>

### 설정

<table>
  <tr>
    <td align="center"><img src="preview/01-settings/10_setting.png"><br>일반</td>
    <td align="center"><img src="preview/01-settings/11_setting.png"><br>오디오</td>
    <td align="center"><img src="preview/01-settings/12_setting.png"><br>자막</td>
    <td align="center"><img src="preview/01-settings/13_setting.png"><br>컨트롤</td>
  </tr>
  <tr>
    <td align="center"><img src="preview/01-settings/14_setting.png"><br>태그</td>
    <td align="center"><img src="preview/01-settings/15_setting.png"><br>언어</td>
    <td align="center"><img src="preview/01-settings/16_setting.png"><br>보안</td>
    <td align="center"><img src="preview/01-settings/17_setting.png"><br>정보</td>
  </tr>
</table>

---

## 단축키

| 키 | 동작 |
|---|---|
| `Space` | 재생 / 일시정지 |
| `←` / `→` | 탐색 ±1초 |
| `Shift` + `←` / `→` | 탐색 ±5초 |
| `Ctrl` + `←` / `→` | 탐색 ±0.2초 |
| `↑` / `↓` | 볼륨 ±5% |
| `Z` / `X` | 이전 / 다음 자막 줄 |
| `Alt` + `←` / `→` | 이전 / 다음 자막 큐로 이동 |
| `[` / `]` | 자막 폰트 크기 −1 / +1 px |

`Ctrl+A`(전체 선택)를 제외한 모든 단축키는 **설정 → 컨트롤**에서 변경할 수 있습니다.
탐색 양(±1초 / ±5초 / ±0.2초)도 카테고리별로 조절 가능합니다.

---

## 다국어 지원

기본 제공 언어: **영어 · 한국어 · 일본어**

다른 언어를 추가하려면 [sample_local.json](sample_local.json) 을 템플릿으로 사용하세요.
파일 이름을 `<언어코드>.json` (예: `fr.json`)으로 변경하고 내용을 번역한 뒤, 아래 경로에 넣으면 됩니다:

```
<실행 파일 경로>/locales/<언어코드>.json
```

같은 언어 코드의 파일이 있으면 사용자 파일이 기본 내장 파일보다 우선 적용됩니다.

---

## 로드맵

| | |
|---|---|
| Mac 빌드 및 배포 | 서명된 macOS 릴리즈 빌드 및 배포 |
| 자막 자동 생성 | Whisper 모델 연동을 통한 VTT 자막 자동 생성 |
| 사용자 정의 테마 | JSON 파일 또는 앱 내 에디터를 통한 커스텀 테마 지원 |
| 메뉴얼 & 문서 | 앱 내 도움말 및 온라인 문서 작성 |

---

## 릴리스 노트

<details>
<summary><strong>v1.0.1</strong> &nbsp;— 2026. 02. 22</summary>

<br>

**버그 수정**

- **Windows 네트워크 경로(UNC) 재생 오류** — `\\server\share\...` 형식의 UNC 경로(SMB 공유, NAS 등)에 있는 오디오 파일이 재생되지 않던 문제를 수정했습니다. 내부 경로 처리 방식의 오류로 해당 위치의 파일이 로드되지 않았습니다.
- **네트워크 드라이브의 snrpack 이미지** — 네트워크 공유에 저장된 `.snrpack` 패키지 내 이미지가 표시되지 않던 문제를 수정했습니다.
- **snrpack 내보내기 시 커버 이미지 포함** — `.snrpack` 패키지를 만들 때 오디오와 같은 이름의 이미지 파일(예: `music.flac` 옆의 `music.png`)이 자동으로 감지되어 패키지에 함께 포함됩니다.
- **정렬 옵션 저장** — 플레이리스트에서 설정한 정렬 기준과 방향이 앱을 종료해도 유지됩니다.

</details>
