# auto-caption-project
---
# 제1장. 미니 프로젝트 개요

## 1.1 프로젝트 목표와 전체 구조

### 1.1.1 AI Agent 과정에서의 위치

이 미니 프로젝트는 **AI Agent 과정 초반부, LLM API를 실제로 처음 활용해 보는 단계**에 해당합니다. 대규모 이론이나 복잡한 에이전트 구조에 들어가기 전에, AI Agent가 외부 입력을 받아 처리하고 결과를 만들어내는 **기본 동작 흐름**을 경험하는 위치입니다. 즉, 이후 프롬프트 엔지니어링, RAG, 멀티 에이전트로 확장되기 전의 **기초 실습 단계**입니다.

---

### 1.1.2 이 미니 프로젝트를 수행하는 이유

이 프로젝트를 수행하는 이유는 다음과 같습니다.

① LLM과 AI 모델이 “말로만 이해하는 기술”이 아니라, 실제 입력과 출력이 연결되는 서비스 구조임을 체감하기 위함입니다.

② AI Agent가 처리하는 작업이 단일 API 호출이 아니라, 여러 단계를 거쳐 결과를 만들어낸다는 점을 이해하기 위함입니다.

③ 이후 복잡한 AI Agent 구조를 학습하기 전에, 데이터 흐름과 역할 분리를 명확히 익히기 위함입니다.

즉, 이 미니 프로젝트는 **AI Agent의 가장 단순한 형태를 직접 만들어 보는 경험**에 해당합니다.

---

### 1.1.3 핵심 이해 포인트

① AI는 스스로 동작하지 않으며, 사람이 설계한 흐름에 따라 단계적으로 처리됩니다.

② Whisper와 같은 AI 모델은 “기능 하나를 담당하는 도구”이며, 전체 서비스의 일부일 뿐입니다.

③ AI Agent란 여러 도구(API, 파일, 로직)를 연결하여 목적 있는 결과를 만드는 구조입니다.

이 프로젝트는 “AI Agent가 무엇인지”를 정의로 배우는 것이 아니라, **직접 만들어 보며 감각적으로 이해하는 단계**입니다.

---

### 1.1.4 프로젝트의 전체 처리 흐름 의미

자동 자막 생성 미니 프로젝트의 처리 흐름은 다음과 같은 의미를 가집니다.

① 유튜브 영상 URL 입력

→ 외부 세계로부터 AI Agent가 받는 입력입니다.

② 음성 파일 추출

→ AI 모델이 처리할 수 있도록 데이터를 전처리하는 단계입니다.

③ Whisper API를 통한 음성 인식

→ 특정 역할(STT)을 수행하는 AI 모델을 호출하는 단계입니다.

④ 텍스트 자막 생성

→ AI 모델의 결과를 사람이 활용할 수 있는 형태로 변환하는 단계입니다.

⑤ 자막 파일(SRT 또는 TXT) 저장

→ AI Agent의 최종 결과물을 외부로 출력하는 단계입니다.

이 흐름은 이후 **STT → 요약 → 번역 → TTS**, 또는 **STT → 검색 → RAG → 응답 생성**과 같은 복잡한 AI Agent 구조의 기본 골격이 됩니다.

---

### 1.1.5 다음 단계와의 연결

본 프로젝트는 **AI Agent 전체 과정에서 반드시 거쳐야 하는 기초 관문**에 해당합니다.

이후에는 다음과 같은 확장이 자연스럽게 이어집니다.

① 생성된 자막을 LLM으로 요약하는 AI Agent

② 자막을 번역하거나 질문에 답하는 대화형 AI Agent

③ STT와 TTS를 결합한 자동 뉴스 리더 AI Agent

---

## 1.2 사용 기술 및 개발 환경

### 1.2.1 핵심 기술 스택

① Python 3.10 이상

② OpenAI Whisper API

③ ffmpeg (음성 추출 도구)

④ requests, openai Python SDK

⑤ 로컬 파일 기반 미니 서비스 구조

### 1.2.2 사전 준비 사항

① OpenAI API Key 발급

② Python 가상환경 사용 권장 (아래의 명령어 참조)

③ ffmpeg 설치 완료 상태

- Windows: ffmpeg 설치 후 PATH 추가 (**시작 메뉴** → `환경 변수` 검색)
    
    ```python
    C:\ffmpeg\bin
    
    ```
    
- macOS: `brew install ffmpeg`
- Ubuntu: `sudo apt install ffmpeg`

④ git & github 설정 권장

```python
echo "# auto-caption-project" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://토큰@github.com/계정/레포지토리.git
git push -u origin main
```

---

# 제2장. 개발 환경 설정

## 2.1 프로젝트 폴더 구조 구성

다음 폴더 구조는 입력 데이터와 처리 로직, 출력 결과를 분리하여 관리하기 위한 기본 구성입니다.

.git 과 .venv는 생성 명령으로 자동 생성 합니다. 그외 다른 디렉토리는 직접 생성합니다.

```
auto-caption-project/
 ├─ .git/
 ├─ .gitignore/
 ├─ README.md
 ├─ .env
 ├─ .venv/
 ├─ input/
 │   └─ audio/
 ├─ output/
 │   └─ subtitles/
 ├─ main.py
 ├─ youtube_audio.py
 ├─ whisper_stt.py
 └─ requirements.txt

```

### 2.1.1 .venv/

프로젝트 전용 파이썬 가상환경 폴더입니다. 외부 라이브러리를 시스템과 분리하여 설치합니다.

### 2.1.2 input/

프로젝트 실행에 필요한 입력 데이터를 모아두는 폴더입니다.

### 2.1.2.1 input/audio/

유튜브에서 추출한 음성 파일(mp3 등)을 저장하는 폴더입니다. STT 처리의 입력 파일이 위치합니다.

### 2.1.3 output/

프로젝트 실행 결과물을 저장하는 폴더입니다.

### 2.1.3.1 output/subtitles/

Whisper STT 결과로 생성된 자막 파일(txt 또는 srt)을 저장하는 폴더입니다.

### 2.1.4 main.py

프로그램 실행 시작 파일입니다. 전체 흐름(URL 입력 → 음성 추출 → STT → 저장)을 순서대로 호출합니다.

### 2.1.5 youtube_audio.py

유튜브 URL에서 음성을 추출하여 `input/audio/`에 저장하는 모듈입니다.

### 2.1.6 whisper_stt.py

음성 파일을 Whisper API로 전송하여 텍스트로 변환하는 STT 모듈입니다.

### 2.1.7 requirements.txt

프로젝트 실행에 필요한 라이브러리 목록 파일입니다. 동일 환경 재현을 위해 사용합니다.

```python
# 파일 생성 및 라이브러리 목록 저장 (라이브러리 설치 후)
pip freeze > requirements.txt

# 라이브러리 환경 재현
pip install -r requirements.txt
```

---

## 2.2 필수 라이브러리 설치

### 2.2.1 가상환경 생성 및 활성화

```bash
# Windows 환경
py --list
py -3.12 -m venv .venv
.venv\Scripts\activate

# macOS 환경
python3 -m venv .venv
source .venv/bin/activate

# Ubuntu(Linux) 환경
python3 -m venv .venv
source .venv/bin/activate

```

가상 환경 삭제 명령

```python
# Windows 환경
rmdir /s /q .venv

# macOS / Ubuntu(Linux) 환경
rm -rf .venv

```

### 2.2.2 라이브러리 설치

```bash
pip install openai yt-dlp python-dotenv

```

- openai: Whisper 및 LLM API 호출을 위한 라이브러리입니다.
- yt-dlp: 유튜브 영상에서 음성을 추출하기 위한 도구입니다.
- python-dotenv: OpenAI API Key와 같은 환경 변수를 `.env` 파일로 관리하기 위한 도구입니다.

---

# 제3장. 유튜브 음성 추출 모듈 구현

## 3.1 유튜브 주소로 음성 파일 다운로드

### 3.1.1 youtube_audio.py 작성

```python
import os
import subprocess
from pathlib import Path

def download_audio(youtube_url: str) -> str:
    # 1) 저장 폴더 준비
    audio_dir = Path("input") / "audio"
    audio_dir.mkdir(parents=True, exist_ok=True)

    # 2) 출력 템플릿(확장자는 yt-dlp가 결정 → mp3 변환 후 mp3로 저장)
    outtmpl = str(audio_dir / "audio.%(ext)s")

    command = [
        "yt-dlp",
        "--no-playlist",
        "-x",
        "--audio-format", "mp3",
        "-o", outtmpl,
        youtube_url
    ]

    # 3) 실행 및 오류 처리
    result = subprocess.run(command, capture_output=True, text=True)
    if result.returncode != 0:
        raise RuntimeError(
            "yt-dlp 실행 실패입니다.\n"
            "ffmpeg 미설치/경로 문제이거나 URL이 잘못되었을 수 있습니다.\n"
            f"{result.stderr}"
        )

    # 4) 최종 mp3 경로 반환
    mp3_path = audio_dir / "audio.mp3"
    if not mp3_path.exists():
        raise FileNotFoundError(f"mp3 파일이 생성되지 않았습니다: {mp3_path}")

    return str(mp3_path)

```

이 단계에서는 유튜브 영상을 직접 다운로드하지 않고 음성만 추출합니다. 실무 환경에서도 서버 부하를 줄이기 위해 음성만 처리하는 구조를 사용합니다.

---

# 제4장. Whisper API 기반 음성 인식 처리

## 4.1 Whisper API의 역할 이해

Whisper는 자동 음성 인식(ASR) 모델로, 입력된 오디오를 텍스트로 변환합니다. 내부적으로 약 30초 단위로 음성을 분할하여 처리하며, 다국어 인식과 번역을 동시에 수행할 수 있습니다.

---

## 4.2 Whisper STT 모듈 구현

### 4.2.1 whisper_stt.py 작성

이 모듈은 `.env` 파일에 저장된 `OPENAI_API_KEY`를 불러와 Whisper API를 호출합니다.

```python
import os
from dotenv import load_dotenv
from openai import OpenAI

# 1) .env 파일 로드
load_dotenv()

# 2) 환경 변수 확인
if not os.getenv("OPENAI_API_KEY"):
    raise RuntimeError("OPENAI_API_KEY가 설정되지 않았습니다. .env 파일을 확인하세요.")

# 3) OpenAI 클라이언트 생성
#    OPENAI_API_KEY 환경 변수를 자동으로 사용합니다.
client = OpenAI()

def transcribe_audio(audio_path: str) -> str:
    with open(audio_path, "rb") as audio_file:
        transcript = client.audio.transcriptions.create(
            model="whisper-1",
            file=audio_file,
            response_format="text"
        )
    return transcript

```

이 단계에서 Whisper API는 음성 파일을 입력받아 순수 텍스트 형태의 결과를 반환합니다.

---

# 제5장. 자막 파일 생성 로직 구현

## 5.1 텍스트 파일 형태의 자막 저장

### 5.1.1 main.py 작성

```python
from pathlib import Path
from dotenv import load_dotenv

from youtube_audio import download_audio
from whisper_stt import transcribe_audio

def main():
    # 1) .env 로드(OPENAI_API_KEY)
    load_dotenv()

    # 2) 입력
    youtube_url = input("유튜브 URL을 입력하세요: ").strip()
    if not youtube_url:
        raise ValueError("유튜브 URL이 비어 있습니다.")

    # 3) 음성 추출(다운로드) → audio_path 반환
    audio_path = download_audio(youtube_url)
    audio_path = Path(audio_path)

    if not audio_path.exists():
        raise FileNotFoundError(f"음성 파일이 생성되지 않았습니다: {audio_path}")

    # 4) Whisper STT
    text = transcribe_audio(str(audio_path))

    # 5) 출력 폴더 생성 + 저장
    out_dir = Path("output") / "subtitles"
    out_dir.mkdir(parents=True, exist_ok=True)

    output_file = out_dir / "result.txt"
    output_file.write_text(text, encoding="utf-8")

    print("자막 파일 생성 완료:", output_file)

if __name__ == "__main__":
    main()
```

### 5.1.1 너무 긴 영상 용 main2.py 로 업그레이드

**10분 단위로 잘라서** 각 조각을 Whisper에 보내면 조각들을 순차 변환해서 result.txt로 합치기. **chunk 폴더가 있으면 chunk들을 모두 STT 후 합치는 방식**입니다. (분할은 대표적인 우회 방법입니다.) 

```python
from pathlib import Path
from dotenv import load_dotenv

from youtube_audio import download_audio
from whisper_stt import transcribe_audio

def transcribe_in_chunks(chunk_dir: Path) -> str:
    parts = sorted(chunk_dir.glob("part_*.mp3"))
    if not parts:
        raise FileNotFoundError("분할된 오디오(part_*.mp3)가 없습니다.")

    texts = []
    for i, p in enumerate(parts, start=1):
        print(f"[{i}/{len(parts)}] STT 처리 중: {p.name}")
        texts.append(transcribe_audio(str(p)))
    return "\n".join(texts)

def main():
    load_dotenv()

    youtube_url = input("유튜브 URL을 입력하세요: ").strip()
    audio_path = Path(download_audio(youtube_url))

    out_dir = Path("output") / "subtitles"
    out_dir.mkdir(parents=True, exist_ok=True)
    output_file = out_dir / "result.txt"

    # 1) 먼저 파일이 너무 크면 분할 처리 안내(자동화는 다음 단계에서)
    size_mb = audio_path.stat().st_size / 1024 / 1024
    print(f"오디오 파일 크기: {size_mb:.2f} MB")

    # 2) 분할 파일이 존재하면 분할 기반 STT
    chunk_dir = Path("input") / "audio" / "chunks"
    if chunk_dir.exists():
        text = transcribe_in_chunks(chunk_dir)
    else:
        # 분할 폴더가 없으면 단일 파일 STT 시도
        text = transcribe_audio(str(audio_path))

    output_file.write_text(text, encoding="utf-8")
    print("자막 파일 생성 완료:", output_file)

if __name__ == "__main__":
    main()

```

---

## 5.2 실행 결과 확인

테스트에 사용 가능한 URL:

- https://www.youtube.com/watch?v=0-q1KafFCLU
- https://www.youtube.com/watch?v=E7wJTI-1dvQ
- https://www.youtube.com/watch?v=Y8Wp3dafaMQ

```bash
(.venv) D:\donga_work2025\part2\book2\auto-caption-project>python main.py
유튜브 URL을 입력하세요: https://www.youtube.com/watch?v=Y8Wp3dafaMQ                      
자막 파일 생성 완료: output\subtitles\result.txt
```

① 유튜브 주소 입력

② 음성 다운로드 진행

③ Whisper STT 처리

④ result.txt 자막 파일 생성

이 결과물은 이후 SRT 변환, 번역, 요약 등 다양한 LLM 기반 확장 과제로 활용할 수 있습니다.

---

# 프로젝트 확장 아이디어

① SRT 타임스탬프 자막 생성

② 한국어 → 영어 번역 자막

③ 긴 영상 분할 처리

④ 웹 기반 입력 UI(FastAPI 연동)

⑤ LLM 요약 기반 하이라이트 생성

---

본 미니 프로젝트는 Whisper API를 활용한 대표적인 LLM 기반 서비스 구현 사례입니다. 단순한 API 호출을 넘어 입력 데이터 처리, 파일 변환, 결과 저장까지의 전체 흐름을 경험하는 데 목적이 있습니다. 이 구조는 이후 AI Agent, RAG, 멀티모달 서비스로 확장 가능한 기본 골격으로 활용할 수 있습니다.
