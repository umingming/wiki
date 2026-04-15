# Sources 디렉토리

원본 자료를 이 폴더에 저장합니다.

## 지원 포맷
- PDF (`.pdf`)
- 텍스트 (`.txt`, `.md`)
- 웹 아카이브 (`.html`)
- 이미지 (`.png`, `.jpg`) - OCR 필요 시

## 명명 규칙
```
[저자-성]-[연도]-[짧은-제목].[확장자]
```

예시:
- `karpathy-2025-llm-wiki.pdf`
- `vaswani-2017-attention-is-all-you-need.pdf`
- `openai-2023-gpt4-technical-report.pdf`

## 소스 추가 절차
1. 이 폴더에 파일 저장 (명명 규칙 준수)
2. LLM에게 ingest 요청: "이 소스를 위키에 추가해줘"
3. LLM이 자동으로 summary 생성 및 index 업데이트
