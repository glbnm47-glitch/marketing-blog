---
title: "클로드코드 MCP 설정 추천 2026: 직접 써보니 이거면 됩니다"
description: "클로드코드 MCP 설정 추천 TOP 5를 실사용 기반으로 정리했습니다. GitHub·Notion·Firecrawl 등 상황별 MCP 서버 선택법과 2026년 신기능 Tool Search까지 한 번에 정리."
date: 2026-06-29
lastmod: 2026-06-29
draft: false
tags: ["클로드코드", "MCP 설정", "클로드코드 MCP 추천", "클로드코드 사용법"]
categories: ["클로드코드"]
keywords: "클로드코드 MCP 설정 추천, 클로드코드 사용법, 클로드코드 MCP, MCP 서버 추천, claude code mcp"
slug: "claude-code-mcp-recommended-2026"
cover:
  image: ""
  alt: ""
  relative: false
---

MCP 설정하다 중간에 포기하셨나요? 공식 문서 따라가다 에러 나고, 뭘 설치해야 할지 몰라서 그냥 덮어두신 경험 있으시죠?
이 글 하나로 끝납니다.
실제로 1개월 이상 클로드코드 MCP를 써보며 추린 핵심 5개, 지금 바로 설정할 수 있도록 순서대로 정리했습니다.

---

## MCP가 뭔지 30초 안에 이해하기

MCP(Model Context Protocol)는 클로드코드가 외부 도구와 말을 섞을 수 있게 해주는 연결 규격입니다. 쉽게 말해 "클로드코드의 손과 발"입니다.

클로드코드는 기본 상태에서 파일 읽기·쓰기, 터미널 명령 실행 정도만 할 수 있습니다. 하지만 MCP를 연결하면 GitHub 이슈를 직접 열고, Notion 페이지를 생성하고, 웹사이트 데이터를 크롤링하는 것까지 자연어 명령 하나로 처리할 수 있게 됩니다.

클로드코드 사용법을 처음 익히는 단계라면 MCP 없이도 충분하지만, 업무 자동화나 반복 작업 처리에 쓰기 시작하는 순간 MCP의 존재감이 달라집니다.

---

## 2026년 신기능: MCP Tool Search로 컨텍스트 95% 절감

MCP를 설정하다 보면 금방 10개, 20개가 쌓입니다. 문제는 MCP를 많이 연결할수록 컨텍스트 창이 먼저 차버려 실제 작업 내용을 담을 공간이 줄어드는 것이었습니다.

2026년 1월 Anthropic이 **MCP Tool Search** 기능을 기본 탑재하면서 이 문제가 해결됐습니다.

기존 방식은 세션 시작 시 연결된 MCP 도구 정의를 모두 컨텍스트에 올렸습니다. MCP 50개 이상이면 세션 시작만으로 약 77,000 토큰이 소비됐습니다. Tool Search는 필요할 때만 해당 도구를 불러오는 **지연 로딩(lazy loading)** 방식으로 작동합니다. 50개 MCP 기준으로 77,000 토큰이 약 8,700 토큰으로 줄었습니다. **컨텍스트 사용량 95% 감소**입니다.

정확도도 올라갔습니다. Anthropic 내부 테스트에서 Opus 4 기준 MCP 평가 정확도가 49%에서 74%로 상승했습니다.

현재 MCP Tool Search는 별도 설정 없이 **기본 활성화** 상태입니다. 연결된 MCP 도구 설명이 컨텍스트 창의 10%를 초과하는 순간 자동으로 켜집니다.

---

## 상황별 MCP 추천 TOP 5

수십 개의 MCP 서버가 존재하지만, 대부분의 사용자에게 실질적으로 필요한 것은 5개 이내입니다. 상황별로 정리했습니다.

### 1. GitHub MCP — 개발자라면 무조건 1번

**대상**: 코드를 작성하고 PR·이슈를 다루는 개발자

GitHub MCP를 연결하면 클로드코드에서 자연어로 PR을 열고, 이슈를 생성하고, 코드 리뷰 코멘트를 달 수 있습니다. 브라우저를 열 필요가 없습니다. "현재 브랜치로 PR 만들어줘, 제목은 feat: 로그인 버그 수정" 한 줄이면 됩니다.

```bash
claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

설치 후 `GITHUB_PERSONAL_ACCESS_TOKEN` 환경 변수만 설정하면 바로 사용 가능합니다.

### 2. Notion MCP — 기획자·문서 작성자 필수

**대상**: Notion을 업무 허브로 사용하는 모든 직군

클로드코드가 작성한 분석 결과나 코드 리포트를 Notion 페이지로 자동 저장하게 할 수 있습니다. 회의록 정리, 스펙 문서 초안 작성, 작업 목록 업데이트를 명령어 하나로 처리합니다.

```bash
claude mcp add notion -- npx -y @notionhq/notion-mcp-server
```

`NOTION_API_KEY`와 통합 권한 설정이 필요합니다. Notion 설정 > 인티그레이션에서 발급받을 수 있습니다.

### 3. Firecrawl MCP — 데이터 수집이 필요한 모든 사람

**대상**: 경쟁사 분석, 리서치, 가격 모니터링이 필요한 사용자

Firecrawl은 웹 크롤링과 데이터 추출을 AI가 직접 처리하게 해줍니다. "이 URL에서 가격 정보만 뽑아서 CSV로 만들어줘"가 실제로 작동합니다. JavaScript 렌더링이 필요한 복잡한 사이트도 처리합니다.

```bash
claude mcp add firecrawl -- npx -y firecrawl-mcp
```

무료 플랜에서 월 500 크레딧을 제공합니다. 가벼운 리서치 용도라면 비용 없이 쓸 수 있습니다.

### 4. Supabase MCP — 데이터베이스 연동 개발자

**대상**: Supabase를 백엔드로 사용하는 개발자

스키마 확인, 쿼리 작성, 데이터 조회를 클로드코드에서 직접 할 수 있습니다. "users 테이블에서 최근 7일 가입자 수 보여줘"처럼 자연어로 데이터를 다룰 수 있어 개발 속도가 눈에 띄게 빨라집니다.

공식 Supabase MCP는 Supabase CLI를 통해 설치할 수 있으며, 프로젝트 URL과 API 키를 환경 변수로 설정합니다.

### 5. Context7 MCP — 문서 최신화 작업

**대상**: 라이브러리·프레임워크를 자주 쓰는 개발자

클로드코드가 코드를 쓸 때 오래된 API를 참조하는 경우가 있습니다. Context7 MCP는 실시간으로 최신 공식 문서를 불러와 코드 생성에 반영합니다. React, Next.js, Tailwind CSS 등 주요 라이브러리를 지원합니다.

```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp
```

---

## MCP 설정 단계별 가이드

클로드코드에서 MCP를 추가하는 방법은 크게 두 가지입니다. 처음이라면 CLI 방식이 가장 쉽습니다.

### CLI 방식 (초보자 추천)

터미널에서 `claude mcp add` 명령어를 씁니다.

```bash
# 기본 형식
claude mcp add <서버이름> -- <실행 명령>

# 환경 변수를 같이 넘길 때
claude mcp add github -e GITHUB_PERSONAL_ACCESS_TOKEN=ghp_xxxx -- npx -y @modelcontextprotocol/server-github

# 추가된 서버 목록 확인
claude mcp list

# 서버 작동 여부 테스트
claude mcp test github
```

### 설정 파일 직접 편집 (팀 공유용)

프로젝트 루트에 `.mcp.json` 파일을 만들고 팀 전체가 쓸 MCP를 정의합니다.

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

환경 변수는 `${변수명}` 형식으로 참조합니다. 실제 토큰 값을 파일에 직접 쓰지 마세요. `.mcp.json`은 Git에 올라가기 때문에 실수로 노출될 수 있습니다.

---

## MCP 설정 범위 선택: local vs project vs user

MCP를 어디에 등록할지는 용도에 따라 달라집니다.

| 범위 | 저장 위치 | 적용 대상 | 추천 용도 |
|------|----------|----------|----------|
| **local** | `~/.claude.json` (프로젝트별) | 나만, 이 프로젝트에서 | 개인 토큰, 실험적 MCP |
| **project** | 프로젝트 루트 `.mcp.json` | 팀 전체, 이 프로젝트 | 팀 공통 도구, Git 버전 관리 |
| **user** | `~/.claude.json` (전역) | 나만, 모든 프로젝트 | 항상 쓰는 개인 도구 |

대부분의 경우 GitHub MCP나 Notion MCP처럼 개인 API 키가 필요한 것은 `user` 범위로, 팀이 공통으로 쓰는 데이터베이스 MCP는 `project` 범위로 설정하는 것이 안전합니다.

```bash
# user 범위로 등록
claude mcp add --scope user notion -- npx -y @notionhq/notion-mcp-server

# project 범위로 등록
claude mcp add --scope project supabase -- npx -y @supabase/mcp-server-supabase
```

---

## 자주 묻는 질문 FAQ

**Q. MCP를 추가했는데 클로드코드가 인식을 못합니다.**

`claude mcp test <서버이름>` 명령어로 먼저 연결 상태를 확인하세요. 대부분은 환경 변수 누락이나 Node.js 버전 문제입니다. Node.js 18 이상을 권장합니다.

**Q. MCP 여러 개 쓰면 느려지지 않나요?**

2026년부터 기본 활성화된 MCP Tool Search 덕분에 50개 이상 연결해도 세션 시작 속도에 큰 차이가 없습니다. 필요한 도구만 그때그때 불러오는 구조라 오히려 정확도가 높아집니다.

**Q. 무료로 쓸 수 있는 MCP가 있나요?**

GitHub MCP, Notion MCP, Context7 MCP는 별도 요금 없이 사용 가능합니다. Firecrawl은 무료 플랜에서 월 500 크레딧, Supabase는 무료 티어에서 MCP 연결을 지원합니다.

**Q. 클로드코드 서브에이전트와 MCP를 같이 쓸 수 있나요?**

네, 가장 강력한 조합입니다. 서브에이전트를 생성할 때 특정 MCP 도구만 허용하도록 제한할 수 있어 보안과 정확도가 모두 올라갑니다. 예를 들어 리서치 전용 서브에이전트에는 Firecrawl MCP만, 코드 리뷰 서브에이전트에는 GitHub MCP만 부여하는 식입니다.

---

## 마무리: 지금 당장 설치할 MCP 딱 1개

처음 클로드코드 MCP 설정 추천을 고민하고 있다면, 오늘 딱 1개만 설치하세요.

**개발자라면 GitHub MCP, 기획자·마케터라면 Notion MCP**입니다.

하나를 써보면 어떤 MCP가 내 워크플로우에 맞는지 감이 잡힙니다. 그다음에 Firecrawl이나 Context7 같은 도구를 추가하면 됩니다. 한 번에 다 설치하고 방치하는 것보다 하나씩 익혀가는 방식이 실제로 더 오래 씁니다.
