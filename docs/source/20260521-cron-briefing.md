# Cron Job: daily-ai-news-blog-briefing

**Job ID:** a541b5e14bb0
**Run Time:** 2026-05-21 08:06:08
**Schedule:** 0 8 * * *

## Prompt

[IMPORTANT: The user has invoked the "blogwatcher" skill, indicating they want you to follow its instructions. The full skill content is loaded below.]

---
name: blogwatcher
description: "Monitor blogs and RSS/Atom feeds via blogwatcher-cli tool."
version: 2.0.0
author: JulienTant (fork of Hyaxia/blogwatcher)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [RSS, Blogs, Feed-Reader, Monitoring]
    homepage: https://github.com/JulienTant/blogwatcher-cli
prerequisites:
  commands: [blogwatcher-cli]
---

# Blogwatcher

Track blog and RSS/Atom feed updates with the `blogwatcher-cli` tool. Supports automatic feed discovery, HTML scraping fallback, OPML import, and read/unread article management.

## Installation

Pick one method:

- **Go:** `go install github.com/JulienTant/blogwatcher-cli/cmd/blogwatcher-cli@latest`
- **Docker:** `docker run --rm -v blogwatcher-cli:/data ghcr.io/julientant/blogwatcher-cli`
- **Binary (Linux amd64):** `curl -sL https://github.com/JulienTant/blogwatcher-cli/releases/latest/download/blogwatcher-cli_linux_amd64.tar.gz | tar xz -C /usr/local/bin blogwatcher-cli`
- **Binary (Linux arm64):** `curl -sL https://github.com/JulienTant/blogwatcher-cli/releases/latest/download/blogwatcher-cli_linux_arm64.tar.gz | tar xz -C /usr/local/bin blogwatcher-cli`
- **Binary (macOS Apple Silicon):** `curl -sL https://github.com/JulienTant/blogwatcher-cli/releases/latest/download/blogwatcher-cli_darwin_arm64.tar.gz | tar xz -C /usr/local/bin blogwatcher-cli`
- **Binary (macOS Intel):** `curl -sL https://github.com/JulienTant/blogwatcher-cli/releases/latest/download/blogwatcher-cli_darwin_amd64.tar.gz | tar xz -C /usr/local/bin blogwatcher-cli`

All releases: https://github.com/JulienTant/blogwatcher-cli/releases

### Docker with persistent storage

By default the database lives at `~/.blogwatcher-cli/blogwatcher-cli.db`. In Docker this is lost on container restart. Use `BLOGWATCHER_DB` or a volume mount to persist it:

```bash
# Named volume (simplest)
docker run --rm -v blogwatcher-cli:/data -e BLOGWATCHER_DB=/data/blogwatcher-cli.db ghcr.io/julientant/blogwatcher-cli scan

# Host bind mount
docker run --rm -v /path/on/host:/data -e BLOGWATCHER_DB=/data/blogwatcher-cli.db ghcr.io/julientant/blogwatcher-cli scan
```

### Migrating from the original blogwatcher

If upgrading from `Hyaxia/blogwatcher`, move your database:

```bash
mv ~/.blogwatcher/blogwatcher.db ~/.blogwatcher-cli/blogwatcher-cli.db
```

The binary name changed from `blogwatcher` to `blogwatcher-cli`.

## Common Commands

### Managing blogs

- Add a blog: `blogwatcher-cli add "My Blog" https://example.com`
- Add with explicit feed: `blogwatcher-cli add "My Blog" https://example.com --feed-url https://example.com/feed.xml`
- Add with HTML scraping: `blogwatcher-cli add "My Blog" https://example.com --scrape-selector "article h2 a"`
- List tracked blogs: `blogwatcher-cli blogs`
- Remove a blog: `blogwatcher-cli remove "My Blog" --yes`
- Import from OPML: `blogwatcher-cli import subscriptions.opml`

### Scanning and reading

- Scan all blogs: `blogwatcher-cli scan`
- Scan one blog: `blogwatcher-cli scan "My Blog"`
- List unread articles: `blogwatcher-cli articles`
- List all articles: `blogwatcher-cli articles --all`
- Filter by blog: `blogwatcher-cli articles --blog "My Blog"`
- Filter by category: `blogwatcher-cli articles --category "Engineering"`
- Mark article read: `blogwatcher-cli read 1`
- Mark article unread: `blogwatcher-cli unread 1`
- Mark all read: `blogwatcher-cli read-all`
- Mark all read for a blog: `blogwatcher-cli read-all --blog "My Blog" --yes`

## Environment Variables

All flags can be set via environment variables with the `BLOGWATCHER_` prefix:

| Variable | Description |
|---|---|
| `BLOGWATCHER_DB` | Path to SQLite database file |
| `BLOGWATCHER_WORKERS` | Number of concurrent scan workers (default: 8) |
| `BLOGWATCHER_SILENT` | Only output "scan done" when scanning |
| `BLOGWATCHER_YES` | Skip confirmation prompts |
| `BLOGWATCHER_CATEGORY` | Default filter for articles by category |

## Example Output

```
$ blogwatcher-cli blogs
Tracked blogs (1):

  xkcd
    URL: https://xkcd.com
    Feed: https://xkcd.com/atom.xml
    Last scanned: 2026-04-03 10:30
```

```
$ blogwatcher-cli scan
Scanning 1 blog(s)...

  xkcd
    Source: RSS | Found: 4 | New: 4

Found 4 new article(s) total!
```

```
$ blogwatcher-cli articles
Unread articles (2):

  [1] [new] Barrel - Part 13
       Blog: xkcd
       URL: https://xkcd.com/3095/
       Published: 2026-04-02
       Categories: Comics, Science

  [2] [new] Volcano Fact
       Blog: xkcd
       URL: https://xkcd.com/3094/
       Published: 2026-04-01
       Categories: Comics
```

## Notes

- Auto-discovers RSS/Atom feeds from blog homepages when no `--feed-url` is provided.
- Falls back to HTML scraping if RSS fails and `--scrape-selector` is configured.
- Categories from RSS/Atom feeds are stored and can be used to filter articles.
- Import blogs in bulk from OPML files exported by Feedly, Inoreader, NewsBlur, etc.
- Database stored at `~/.blogwatcher-cli/blogwatcher-cli.db` by default (override with `--db` or `BLOGWATCHER_DB`).
- Use `blogwatcher-cli <command> --help` to discover all flags and options.

[IMPORTANT: The user has invoked the "insane-search" skill, indicating they want you to follow its instructions. The full skill content is loaded below.]

---
name: insane-search
description: "Use when normal web access fails or when accessing blocked/social/developer/media platforms such as X, Reddit, YouTube, GitHub, Stack Overflow, Naver Blog, HN, Medium, Substack, Threads, Mastodon, Bluesky, arXiv, and similar sites. Adapted from fivetaku/insane-search for Hermes using browser tools, Python urllib, public APIs, Jina-style fallback patterns, and yt-dlp where available."
version: 1.0.0
source_repo: https://github.com/fivetaku/insane-search
author: Hermes Agent
license: MIT
---

# Insane Search for Hermes

이 스킬은 Claude Code 플러그인 `fivetaku/insane-search`를 Hermes Agent 환경에 맞게 옮긴 버전이다.

## 언제 사용하나
다음 상황이면 이 스킬을 우선 고려한다.
- 일반 웹 접근이 막힘
- WebFetch류 접근이 402/403/차단/빈 페이지를 반환하는 플랫폼
- 소셜 미디어/X/레딧/Threads/Bluesky/Mastodon 내용 확인
- GitHub/Stack Overflow/Hacker News/arXiv 등 개발·연구 플랫폼 접근
- YouTube 등 미디어 메타데이터/자막 추출
- 네이버 블로그/뉴스/한국 커뮤니티 접근

## Hermes 환경에 맞춘 실행 원칙
원본 플러그인은 Claude Code의 WebFetch/WebSearch 보완용이지만, Hermes에서는 다음 방식으로 대응한다.

1. 먼저 `browser_navigate`, `browser_snapshot`, `browser_console`로 직접 접근 시도
2. 차단되면 공개 API / raw endpoint / RSS / JSON endpoint를 찾는다
3. HTML이 깨지면 Python `urllib`나 `execute_code`로 원문/JSON/Atom을 직접 가져온다
4. 미디어 플랫폼은 가능하면 `yt-dlp`를 사용한다
5. GitHub는 `gh`가 없더라도 raw.githubusercontent.com, 웹 페이지, 공개 API로 대체한다
6. `curl`이 없는 환경에서는 Python `urllib`를 기본 HTTP 클라이언트로 사용한다

## 현재 Hermes 환경 확인 사항
- `yt-dlp` 사용 가능: `/opt/data/home/.local/bin/yt-dlp`
- `curl` 없음
- `gh` 없음
- upstream 원본 저장소는 `/opt/data/external-skills/fivetaku-insane-search`에 zip 다운로드 방식으로 보관 가능하다. 다만 이는 Claude Code용 플러그인/스킬 원본 참조본이며, Hermes 실사용은 이 native `insane-search` 스킬을 기준으로 한다.

즉, 이 스킬은 Hermes에서 `browser_*` + `terminal/python urllib` + `yt-dlp` 조합으로 사용하는 것이 기본이다.

## 우선 접근 전략
### 1) 브라우저 우선
- 로그인 없이 읽히는 페이지는 `browser_navigate`로 먼저 확인
- 동적 렌더링 결과가 필요하면 `browser_console`로 DOM/JSON 추출

### 2) 공개 API / JSON / RSS 우선
- Reddit: `.json`
- Hacker News: Firebase API
- Bluesky/Mastodon/dev.to/npm/PyPI/Wikipedia/OpenLibrary: 공개 API
- arXiv: Atom API
- 학술 preprint/DOI 사이트가 403으로 막히면 DOI 패턴을 먼저 추정해 Crossref API(`https://api.crossref.org/works/<doi>`)로 title/abstract/authors/date를 확인하고, OpenAlex(`https://api.openalex.org/works?search=<title>` 또는 `works/doi:<doi>`)로 최신 버전·OA 상태를 교차검증한다. 논문 제목으로 GitHub repository 검색을 하면 저자 제공 PDF/README가 있는 경우가 있으므로 `api.github.com/search/repositories`와 `raw.githubusercontent.com`을 fallback으로 사용한다.
- RSS가 있으면 RSS 우선
- **Google News RSS**는 주제 탐색용으로 매우 유용하다. 다만 `news.google.com/rss/articles/...` 링크를 `urllib`로 직접 열면 스플래시/중간 페이지 HTML만 받는 경우가 많다. 이때는 `browser_navigate`로 연 뒤 `browser_console(expression='document.location.href')`로 최종 도착 URL을 확인하라. 스냅샷이 비어 있거나 `Please enable JS`만 보여도 최종 URL은 읽히는 경우가 있다.
- Google 일반 검색은 봇 차단(`sorry` 페이지)에 자주 걸리므로, 광범위한 주제 스캐닝은 Google Search보다 **Google News RSS + 원문 접근** 조합이 더 안정적이다.

### 3) Raw / alternate endpoint 우선
- GitHub: `raw.githubusercontent.com`
- Naver Blog: 모바일 URL / 모바일 user-agent 전략 참고
- 한국 커뮤니티/블로그: Jina-style 텍스트화 또는 대체 endpoint 우선

### 4) 미디어는 yt-dlp
- YouTube/Vimeo/Twitch/TikTok/SoundCloud 등은 `yt-dlp --dump-json` 또는 자막 추출 사용

### 5) 최후 fallback
- OGP 메타데이터
- Google cache / Wayback 등 아카이브형 접근
- `r.jina.ai/http://<url>` 텍스트화 fallback은 매우 유용하지만, **Reuters 등 일부 매체는 451** 또는 법적 제한으로 막힐 수 있다. 이 경우 제목/URL/발행시각만 확보하고, 본문 요약은 다른 원출처/동시기 보도/공식 발표로 보수적으로 보강하되 접근 한계를 명시하라.
- 쿠키 배너가 강한 사이트(예: 일부 법률/미디어 사이트)는 `r.jina.ai` 결과 상단이 쿠키/네비게이션으로 오염될 수 있다. 이때는 초반 잡음을 버리고 본문 문단이 시작되는 부분만 선별해서 읽어라.
- 접근 실패 사실과 원인을 명시하고 대안 제시

## 권장 사용 패턴
### GitHub 분석
- 저장소 README / raw 파일 / 공개 페이지 / 이슈/PR 구조 확인
- 코드블록, 설치 방법, 핵심 파일을 요약
- Hermes용 스킬/참조로 가져올 수 있으면 import

### YouTube 영상 요약
- `yt-dlp`로 메타데이터/자막 확보
- 자막 있으면 핵심 요약
- 자막 없으면 제목/설명/챕터 기준 요약

### Reddit/X 반응 분석
- 공개 API 또는 접근 가능한 syndication/json endpoint 우선
- 포스트 본문, 점수, 댓글 흐름, 반복 의견을 구조화

### 한국 사이트 접근
- 네이버 블로그/뉴스/커뮤니티는 모바일/텍스트화 endpoint를 우선 시도
- 요즘IT처럼 CloudFront 403으로 막히고 `r.jina.ai`도 차단 응답만 반환하는 사이트는 DuckDuckGo/Google News RSS 등 검색 결과 제목·스니펫, 관련 공개 GitHub 저장소/코드, 공식 링크를 보조 근거로 수집한다. 이 경우 원문 전문을 직접 인용하지 말고, 접근 실패 원인과 보조 근거 사용 사실을 결과물의 source notes에 명시한다.
- 본문, 제목, 날짜, 링크를 구조적으로 정리

## 주의사항
- 인증이 필요한 개인 데이터는 우회하지 않는다
- 차단된 사이트는 합법적 공개 endpoint / 공개 API / 아카이브 범위 내에서만 접근한다
- 정보를 가져오지 못하면 "접근 불가"와 원인을 명확히 적는다

## Reference files
- `references/README.md`
- `references/SKILL.original.md`
- `references/jina.md`
- `references/json-api.md`
- `references/public-api.md`
- `references/media.md`
- `references/naver.md`
- `references/fallback.md`
- `references/source.md`

[IMPORTANT: The user has invoked the "default-research-workflow" skill, indicating they want you to follow its instructions. The full skill content is loaded below.]

---
name: default-research-workflow
description: Default Hermes research workflow. Use for web research, trend analysis, competitor scans, source gathering, news/blog synthesis, and blocked-site retrieval. Prioritizes insane-search when ordinary access is limited.
version: 1.0.0
author: Hermes Agent
license: MIT
---

# Default Research Workflow

이 스킬은 Hermes의 기본 리서치 작업 흐름이다.

## Trigger conditions
다음과 같은 작업에 기본적으로 적용한다.
- 웹 리서치
- 뉴스/블로그/기술동향 조사
- 경쟁사/시장 조사
- 출처 수집
- 링크 분석
- 차단된 사이트/플랫폼 접근 시도

## Default stack
리서치 작업 시 가능하면 아래 순서로 함께 고려한다.
1. `insane-search`
2. 관련 도메인 지식베이스 스킬 (예: `air-studio-kb`)
3. 필요 시 일반 브라우저/웹 조사

## Mandatory behavior
### 1. Prefer source-first research
- 원출처에 가까운 링크를 우선한다.
- 재인용 기사보다 공식 블로그, 공식 문서, 원문 발표, GitHub 원 저장소를 우선한다.

### 2. Use insane-search when access is limited
다음 경우에는 `insane-search` 관점을 우선 적용한다.
- 일반 접근이 막히거나 내용이 비어 있음
- X, Reddit, YouTube, GitHub, Stack Overflow, Hacker News, Naver Blog/News 등 플랫폼 접근
- RSS/JSON/API/모바일/텍스트화 우회가 유리한 경우

### 3. Structure outputs for practical use
가능하면 결과를 아래 구조로 정리한다.
- 핵심 내용 요약
- 출처 링크
- 실무적 의미
- 후속 확인 포인트

### 4. Be explicit about access limits
- 못 가져온 정보는 가져온 것처럼 쓰지 않는다.
- 접근 제한/시각 불명/원문 미확인 상태를 명시한다.

## Practical research sequence
1. 검색 대상과 기간 정의
2. 원출처 후보 수집
3. 일반 접근 시도
4. 실패 시 insane-search 전략 적용
5. 중복 제거 및 핵심성 판단
6. 요약 + 실무 코멘트 작성

### Time-bounded news/trend briefings
- For scheduled daily/weekly briefings with a strict publication window, use Google News RSS queries with explicit `after:` / `before:` terms as a discovery layer, then resolve each Google News RSS article URL to the publisher URL before citing it.
- Prefer official/vendor/source pages over syndicated news results. Use the RSS item timestamp for discovery, but verify final publication time from the source page or metadata when available; otherwise label it as feed-based or time-unclear.
- If a Google News RSS URL opens to an empty page, navigate with the browser and read `document.location.href` from the console to capture the final destination URL.
- For source pages, use text extraction fallbacks such as `r.jina.ai/http://<url>` to gather enough article detail for summarization. If extraction fails for legal/access reasons, cite only confirmed metadata and explicitly state the access limit.
- See `references/time-bounded-news-briefing.md` for a compact workflow and pitfalls from daily AI/cloud briefing runs.
- For AI/cloud scheduled briefings, also consult `references/ai-cloud-daily-briefing-source-patterns.md` for proven discovery queries, canonical URL resolution, `r.jina.ai` fallback, and enterprise/presales selection criteria.

## Notes
- 이 스킬은 Hermes의 기본 리서치 태도/절차를 정의한다.
- 차단/플랫폼별 접근 문제는 `insane-search`를 우선 참조한다.

The user has provided the following instruction alongside the skill invocation: [IMPORTANT: You are running as a scheduled cron job. DELIVERY: Your final response will be automatically delivered to the user — do NOT use send_message or try to deliver the output yourself. Just produce your report/output as your final response and the system handles the rest. SILENT: If there is genuinely nothing new to report, respond with exactly "[SILENT]" (nothing else) to suppress delivery. Never combine [SILENT] with content — either report your findings normally, or say [SILENT] and nothing more.]

매일 오전 8시에 아래 형식으로 AI/클라우드 동향 브리핑을 작성해 이 채팅으로 보내라. 이 작업은 완전 자율 실행이며 질문하지 말고, 검색과 정리를 스스로 수행하라.

리서치 기본 원칙:
- 원출처에 가까운 링크를 우선한다.
- 일반 접근이 막히는 플랫폼(X, Reddit, YouTube, GitHub, Stack Overflow, Hacker News, Naver Blog/News 등)은 insane-search 전략을 우선 활용한다.
- 브라우저 접근이 제한되면 공개 API, RSS, JSON, raw endpoint, 모바일/텍스트화 우회, yt-dlp 등 대체 접근을 사용한다.
- 접근이 제한된 내용은 가져온 것처럼 쓰지 말고 한계를 명시한다.

검색 기간:
- 전날 00:00부터 당일 07:50까지 게시/업데이트된 내용만 대상으로 한다.
- 게시 시각이 확인되지 않으면 본문/메타데이터/피드 기준으로 최대한 판별하고, 불명확하면 '시각 불명'으로 표시하라.

추적 주제:
- Claude
- ChatGPT
- Gemini
- Harness Engineering
- 프롬프트 엔지니어링
- 컨텍스트 엔지니어링
- Agent / AI Agent
- AWS
- Google Cloud
- Azure
- Databricks
- Snowflake
- AI Coding
- OpenClaw
- Hermes Agent
- NVIDIA
- Amazon Bedrock
- Google Vertex AI
- RAG
- Ontology

반드시 아래 3개 섹션으로 작성하라.

1. 뉴스 기사 요약
- 해당 기간의 핵심 뉴스 5~10개를 선정하라.
- 각 항목마다 아래를 포함하라.
  - 제목
  - 날짜/시간
  - 출처
  - URL
  - 3~5문장 요약
  - 실무 관점 코멘트(너의 의견) 2~4문장
- 중복 기사/재배포는 제거하고, 가장 원출처에 가까운 링크를 우선하라.

2. 블로그/기술 포스트 요약
- 해당 기간의 핵심 블로그/기술 포스트 5~10개를 선정하라.
- 벤더 공식 블로그, 엔지니어링 블로그, 연구 블로그, 제품 업데이트 글을 우선하라.
- 각 항목마다 아래를 포함하라.
  - 제목
  - 날짜/시간
  - 출처
  - URL
  - 3~5문장 요약
  - 실무 관점 코멘트(너의 의견) 2~4문장

3. 자체 작성 블로그
- 위 기간 중 발견한 흐름에서 가장 중요한 주제를 1개 선정하라.
- 한국어 블로그 글을 직접 작성하라.
- 길이: 약 1,200~2,000자
- 형식:
  - 제목
  - 왜 지금 중요한가
  - 핵심 변화 3가지
  - 실무자에게 미치는 영향
  - 결론
  - 참고 출처 목록(URL 포함)
- 단순 요약이 아니라 인사이트를 담아 작성하라.

작성 원칙:
- 모든 답변은 한국어로 작성한다.
- 실무적 의미, Presales/Consulting 관점, 엔터프라이즈 도입 관점을 강하게 강조하라.
- 각 뉴스/블로그의 코멘트는 가능한 한 다음 관점을 우선 반영하라:
  - 이 이슈가 기업 고객의 어떤 Pain Point와 연결되는가
  - 메가존/AIR 관점에서 어떤 제안 기회로 이어질 수 있는가
  - 어떤 산업(금융, 제조, 유통, 공공, 제약 등)에 먼저 먹히는가
  - 고객 미팅에서 어떤 질문으로 이어가면 좋은가
  - PoC, 플랫폼, 거버넌스, 데이터, 비용최적화 관점에서 어떤 후속 액션이 가능한가
- 코멘트는 단순 감상이 아니라 '세일즈 대화 포인트'와 '제안 연결 포인트'가 되도록 작성하라.
- 허위 출처를 만들지 말고, 실제 확인 가능한 링크만 포함하라.
- 찾을 수 없는 주제는 억지로 채우지 말고 전체적으로 중요한 것 위주로 선별하라.
- 링크는 각 항목에 반드시 포함하라.
- 가독성을 위해 섹션/불릿/번호를 적절히 사용하라.



## Response

# AI/클라우드 동향 브리핑  
**검색 기준 시각:** 2026-05-20 00:00 ~ 2026-05-21 07:50 KST  
**작성 시각:** 2026-05-21 08:04 KST  
**주요 키워드:** Gemini, ChatGPT/Codex, AI Agent, AWS, Google Cloud, Databricks, NVIDIA, Harness, RAG/Context, Governance

---

## 1. 뉴스 기사 요약

### 1) Google I/O 2026: “Gemini를 모든 제품의 에이전트 레이어로 확장”
- **날짜/시간:** 2026-05-21 04:30 KST  
- **출처:** Google Blog  
- **URL:** https://blog.google/innovation-and-ai/technology/ai/google-io-2026-all-our-announcements/  
- **요약:** Google은 I/O 2026에서 Gemini 3.5 Flash, Google Antigravity, AI Search, Workspace AI 기능, Universal Cart 등 100개 발표를 정리했다. 핵심은 Gemini를 검색, 개발도구, Workspace, 쇼핑, 개인 에이전트 경험 전반의 공통 AI 레이어로 확장하는 것이다. Gemini 3.5 Flash는 속도와 비용 효율을 강조하면서도 코딩·에이전트 벤치마크 성능을 전면에 내세웠다. Google은 “모델 발표”보다 “에이전트 기반 사용자 경험과 개발자 플랫폼”을 함께 묶어 시장에 제시했다.  
- **실무 관점 코멘트:** 기업 고객에게는 “모델을 하나 고르는 문제”보다 “검색, 업무도구, 개발, 데이터 분석에 AI를 어떻게 일관되게 붙일 것인가”가 핵심 과제로 이동하고 있다. Presales 관점에서는 Google Workspace/Google Cloud 고객에게 Gemini Enterprise, Vertex AI, BigQuery, 데이터 거버넌스를 묶은 **AI 업무 플랫폼 PoC**를 제안하기 좋다. 고객 미팅에서는 “현재 AI가 어느 업무 표면에 흩어져 있고, 권한/감사/비용 관리는 어디서 보고 있나?”를 질문 포인트로 잡을 수 있다.

---

### 2) Gemini 3.5 Flash 공개: “Frontier intelligence with action”
- **날짜/시간:** 2026-05-20 02:45 KST  
- **출처:** Google Blog  
- **URL:** https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-5/  
- **요약:** Google은 Gemini 3.5 제품군의 첫 모델로 Gemini 3.5 Flash를 공개했다. 이 모델은 장기 에이전트 작업, 코딩, 복잡한 실행형 워크플로우에 초점을 맞췄으며, Google Antigravity, Gemini API, Google AI Studio, Android Studio, Gemini Enterprise에서 제공된다. Google은 3.5 Flash가 기존 대형 모델급 지능을 Flash 계열의 속도와 비용 구조로 제공한다고 주장한다. 3.5 Pro는 내부 사용 중이며 다음 달 출시 예정이라고 밝혔다.  
- **실무 관점 코멘트:** “고성능 모델은 비싸고 느리다”는 엔터프라이즈 도입 장벽을 Flash 계열이 계속 압박하고 있다. 금융·제조·유통 고객에게는 고비용 범용 모델 대신, **업무별 라우팅과 비용 최적화된 모델 포트폴리오**를 제안할 수 있다. 고객 미팅에서는 “현재 PoC에서 모델 비용이 운영 전환의 병목인가, 아니면 품질·보안·연동이 병목인가?”를 물어보면 좋다.

---

### 3) OpenAI 모델, 이산기하학의 80년 난제 관련 중심 추측 반박
- **날짜/시간:** 2026-05-20 09:00 KST  
- **출처:** OpenAI  
- **URL:** https://openai.com/index/model-disproves-discrete-geometry-conjecture/  
- **요약:** OpenAI는 내부 범용 추론 모델이 평면 단위 거리 문제의 오랜 추측을 반박하는 구성을 찾아냈다고 발표했다. 해당 문제는 Paul Erdős가 1946년 제기한 조합기하학의 대표적 난제로, 외부 수학자들이 증명을 검토했고 동반 해설 논문도 공개됐다. OpenAI는 이 결과가 특정 수학 문제 전용 시스템이 아니라 범용 추론 모델에서 나왔다는 점을 강조했다. 이는 AI가 단순 보조 도구를 넘어 프런티어 연구의 독창적 아이디어 생성에 기여할 수 있음을 보여주는 사례로 제시됐다.  
- **실무 관점 코멘트:** 직접적인 기업 적용은 수학 연구처럼 보이지만, 메시지는 “AI가 검증 가능한 고난도 추론 업무를 수행할 수 있다”는 것이다. 제약 R&D, 소재, 금융 리스크 모델링, 최적화 문제를 가진 고객에게 **AI 연구 보조·가설 생성·검증 워크플로우** 제안 근거로 활용할 수 있다. 다만 고객에게는 “자율 발견”만 강조하기보다, 외부 검증·재현성·감사 가능한 증거 체계가 함께 필요하다고 설명해야 한다.

---

### 4) AWS Security Hub, 사용하지 않는 IAM 권한·역할·자격증명 리스크 탐지 추가
- **날짜/시간:** 2026-05-21 05:20 KST  
- **출처:** AWS What’s New  
- **URL:** https://aws.amazon.com/about-aws/whats-new/2026/05/aws-security-hub-unused-access/  
- **요약:** AWS Security Hub가 조직 전반의 미사용 IAM 권한, 역할, 자격증명을 탐지하는 기능을 추가했다. Security Hub는 IAM Access Analyzer와 연계해 90일 실제 사용 이력을 평가하고, 미사용 접근 권한을 노출·위협·보안 상태 findings와 함께 보여준다. 또한 실제 사용 패턴 기반 최소 권한 정책 추천을 온디맨드로 생성할 수 있다. 이 기능은 Security Hub Essentials에 포함되어 추가 비용 없이 제공된다.  
- **실무 관점 코멘트:** 클라우드 보안에서 가장 반복적으로 발견되는 Pain Point는 “권한은 많은데 실제 사용 여부를 모른다”는 점이다. 금융·공공·대기업 고객에게는 **IAM Least Privilege 진단, Security Hub 고도화, CIEM 성격의 클라우드 권한 정리 프로젝트**로 연결하기 좋다. 고객 미팅 질문은 “최근 90일 기준 미사용 권한을 조직 단위로 보고, 자동 정책 추천까지 받은 적이 있는가?”가 적합하다.

---

### 5) AWS, DynamoDB 호환 오픈소스 어댑터 ExtendDB 발표
- **날짜/시간:** 2026-05-21 02:00 KST  
- **출처:** AWS What’s New  
- **URL:** https://aws.amazon.com/about-aws/whats-new/2026/05/aws-extenddb-dynamodb/  
- **요약:** AWS는 DynamoDB API를 구현하면서 pluggable storage backend를 지원하는 오픈소스 프로젝트 ExtendDB 0.1을 발표했다. 초기 reference backend는 PostgreSQL이며, 개발자 노트북, 온프레미스 데이터센터, 단절된 엣지 환경처럼 DynamoDB 관리형 서비스 사용이 어려운 곳에서 DynamoDB 프로그래밍 모델을 유지할 수 있다. 테이블, 아이템, 스트림 등 DynamoDB control/data plane API를 구현하며 Apache 2.0 라이선스로 GitHub에서 개발된다.  
- **실무 관점 코멘트:** 이 발표는 “클라우드 네이티브 API를 온프레미스·엣지·CI 환경까지 확장”하는 흐름으로 볼 수 있다. 제조, 국방, 공공, 망분리 환경 고객에게 DynamoDB 기반 애플리케이션의 **로컬 개발·테스트·하이브리드 배포 전략**을 제안할 수 있다. 다만 0.1 초기 버전이므로 운영계 적용보다는 개발/테스트, PoC, disconnected edge 검증부터 접근하는 것이 안전하다.

---

### 6) NVIDIA·Google Cloud, AI Builder 개발자 생태계 강화
- **날짜/시간:** 2026-05-20 05:30 KST  
- **출처:** NVIDIA Blog  
- **URL:** https://blogs.nvidia.com/blog/google-cloud-developer-community-ai-builders/  
- **요약:** NVIDIA와 Google Cloud는 Google I/O 계기에 공동 개발자 커뮤니티를 통해 10만 명 이상의 AI 빌더를 지원한다고 밝혔다. 큐레이션된 학습 경로, 핸즈온 랩, 이벤트를 제공해 개발자가 AI 애플리케이션을 더 쉽게 구축하도록 돕는 것이 핵심이다. 이는 모델·GPU·클라우드 인프라 경쟁이 개발자 교육과 생태계 확보 경쟁으로 확장되고 있음을 보여준다.  
- **실무 관점 코멘트:** 엔터프라이즈 AI 도입은 기술 플랫폼만 구매한다고 성공하지 않고, 내부 개발자 enablement와 표준 아키텍처가 함께 필요하다. 메가존/AIR 관점에서는 Google Cloud + NVIDIA 기반 고객에게 **AI Builder 교육, 레퍼런스 아키텍처, GPU/Vertex AI 비용 최적화 워크숍**을 패키징할 수 있다. 특히 제조·게임·미디어·리테일의 멀티모달/비전 AI 팀에 먼저 먹힐 가능성이 높다.

---

## 2. 블로그/기술 포스트 요약

### 1) OpenAI: Ramp 엔지니어들이 Codex로 코드 리뷰를 가속하는 방식
- **날짜/시간:** 2026-05-20 09:00 KST  
- **출처:** OpenAI  
- **URL:** https://openai.com/index/ramp/  
- **요약:** Ramp는 Codex with GPT-5.5를 코드 리뷰와 내부 agentic tooling 개발에 사용하고 있다고 밝혔다. 기존에는 첫 리뷰까지 몇 시간이 걸리던 pull request가 Codex를 통해 몇 분 안에 실질적 피드백을 받을 수 있게 됐다고 설명한다. Ramp는 Codex가 코드베이스 전체를 깊게 추론하고, CLI와 앱 경험을 함께 제공해 엔지니어가 기존 워크플로우 안에서 사용하기 쉽다고 평가했다. 또한 On-Call Assistant 같은 내부 에이전트 개발에도 Codex를 활용하고 있다.  
- **실무 관점 코멘트:** AI Coding 도입의 핵심 지표는 “개발자가 써봤다”가 아니라 “리뷰 대기시간, 결함 발견률, 온콜 대응시간이 줄었는가”다. 엔터프라이즈 고객에게는 Codex/Claude Code/Gemini CLI 등 도구 비교보다 **AI 개발 생산성 측정 체계와 보안 가드레일**을 먼저 제안하는 것이 유효하다. 고객 질문은 “AI 코드 리뷰를 필수 게이트로 넣을 수 있는 팀과, 아직 실험 단계에 머무는 팀의 차이는 무엇인가?”가 좋다.

---

### 2) Google Cloud: Data Agent Kit, IDE/CLI에 데이터 스킬과 도구를 연결
- **날짜/시간:** 2026-05-21 03:07 KST 기준 Google News feed 확인  
- **출처:** Google Cloud Blog  
- **URL:** https://cloud.google.com/blog/products/data-analytics/data-agent-kit-brings-data-skills-and-tools-to-your-ide-or-cli?hl=en  
- **요약:** Google Cloud는 Data Agent Kit을 통해 데이터 엔지니어링·데이터 사이언스 스킬, MCP 도구, IDE 플러그인을 통합한다고 설명했다. VS Code, Claude Code, Codex, Gemini CLI, Antigravity CLI 등 실무자가 사용하는 환경에서 BigQuery, AlloyDB, Google Cloud Storage 같은 엔터프라이즈 데이터에 안전하게 접근하도록 돕는다. 핵심 문제의식은 에이전트 개발이 빠르게 확산되지만 데이터 접근·보안·개발자 경험은 파편화되어 있다는 점이다. Data Agent Kit은 agentic context, memory, personalization을 위한 harness 역할을 지향한다.  
- **실무 관점 코멘트:** 이 글은 컨텍스트 엔지니어링이 “프롬프트 잘 쓰기”를 넘어 데이터 플랫폼·권한·IDE 경험의 문제로 바뀌고 있음을 보여준다. 고객에게는 BigQuery 기반 **Data Agent PoC**, MCP 기반 데이터 접근 표준화, 개발자별 권한/감사 체계 구축을 제안할 수 있다. 특히 데이터 분석 조직이 크지만 셀프서비스 분석이 막힌 금융·유통·제조 고객에게 적합하다.

---

### 3) AWS: Strands Evals에 이미지-텍스트 작업용 MLLM-as-a-Judge 평가기 추가
- **날짜/시간:** 2026-05-21 03:01 KST  
- **출처:** AWS Machine Learning Blog  
- **URL:** https://aws.amazon.com/blogs/machine-learning/multimodal-evaluators-mllm-as-a-judge-for-image-to-text-tasks-in-strands-evals/  
- **요약:** AWS는 Strands Evals SDK에 이미지-텍스트 작업을 평가하는 네 가지 MLLM-as-a-Judge 평가기를 발표했다. Overall Quality, Correctness, Faithfulness, Instruction Following 기준으로 모델 응답을 원본 이미지에 근거해 채점한다. 텍스트 전용 평가기로는 이미지 캡션, 청구서 추출, 차트 분석, 화면 요약에서 발생하는 시각적 환각을 잡기 어렵다는 점을 문제로 제기한다. 이 평가기는 기존 Case → Experiment → Report 워크플로우와 CI에 통합할 수 있다.  
- **실무 관점 코멘트:** 멀티모달 AI PoC가 늘수록 “보여주기 데모”와 “운영 가능한 품질관리”의 차이가 커진다. 보험 청구, 제조 검사, 리테일 상품 이미지, 의료 문서 OCR 고객에게 **멀티모달 평가 자동화와 AI 품질 게이트**를 제안하기 좋다. 고객 미팅에서는 “현재 이미지 기반 AI 결과를 사람이 샘플링 검수하는가, 아니면 CI/CD에서 자동 평가하는가?”를 물어볼 수 있다.

---

### 4) AWS: SageMaker AI와 vLLM으로 실시간 음성 애플리케이션 구축
- **날짜/시간:** 2026-05-21 02:10 KST  
- **출처:** AWS Machine Learning Blog  
- **URL:** https://aws.amazon.com/blogs/machine-learning/build-real-time-voice-applications-with-amazon-sagemaker-ai-and-vllm/  
- **요약:** AWS는 SageMaker AI의 bidirectional streaming과 vLLM Realtime API를 결합해 실시간 speech-to-text 서비스를 구축하는 방법을 소개했다. 예시는 Mistral AI의 Voxtral-Mini-4B-Realtime-2602 모델을 vLLM 컨테이너로 SageMaker 엔드포인트에 배포하고, WebSocket 기반으로 오디오 입력과 전사 출력을 동시에 스트리밍한다. 기존 request-response 추론은 오디오 전체 수신 후 처리해야 해 지연시간이 크다는 한계가 있다. 글은 음성 에이전트, 실시간 자막, 컨택센터 분석, 접근성 도구를 주요 적용 대상으로 제시한다.  
- **실무 관점 코멘트:** 컨택센터 AI는 RAG 챗봇보다 ROI가 명확한 영역이지만, 지연시간·품질·운영 안정성이 병목이다. 통신, 금융 콜센터, 유통 CS 조직에 **실시간 음성 AI PoC, 상담 요약, QA 자동화, 상담사 코파일럿**을 제안할 수 있다. 비용 관점에서는 관리형 SageMaker와 vLLM 오픈소스 서빙을 결합해 “성능과 통제권”을 함께 설명하기 좋다.

---

### 5) Databricks: Unity Catalog로 대규모 AI Agent 거버넌스 구현
- **날짜/시간:** 2026-05-20 21:41 KST  
- **출처:** Databricks Blog  
- **URL:** https://www.databricks.com/blog/governing-ai-agents-scale-unity-catalog  
- **요약:** Databricks는 조직 내 AI 에이전트가 수천 개로 늘어날 때 발생하는 권한·감사·PII 접근 문제를 Unity Catalog 중심으로 관리해야 한다고 설명했다. 기존 거버넌스는 사람이 결정을 내리고 애플리케이션이 예측 가능하게 실행된다는 가정에 기반했지만, 에이전트는 자율적으로 도구를 호출하고 매번 다른 경로를 선택한다. 따라서 “무엇을 할 수 있는지”를 통제하고 “실제로 무엇을 했는지”를 관찰하는 방식이 필요하다. Unity Catalog와 Unity AI Gateway를 모델 호출, 도구 invocation, MCP 서버, 스킬, 에이전트까지 확장하는 접근을 제시한다.  
- **실무 관점 코멘트:** 엔터프라이즈 AI Agent 도입의 최대 장벽은 모델 성능보다 “누가 어떤 데이터와 도구에 접근했는가”다. Databricks 고객에게는 **Agent Governance Assessment, Unity Catalog 권한 모델 정비, AI Gateway 기반 비용/감사 체계**를 제안할 수 있다. 금융·제약·공공처럼 감사 추적이 중요한 산업에서 특히 강한 메시지다.

---

### 6) Harness: CI 비용 절감 전략
- **날짜/시간:** 2026-05-20 09:00 KST  
- **출처:** Harness Blog  
- **URL:** https://www.harness.io/blog/reduce-ci-costs-without-slowing-down-development  
- **요약:** Harness는 CI 비용이 단순 클라우드 비용을 넘어 테스트 비효율, 캐싱 부재, 인프라 과다할당, 거버넌스 부족에서 발생한다고 설명했다. 테스트 최적화, intelligent caching, right-sizing, 정책 기반 통제를 통해 개발 속도를 늦추지 않고 비용을 줄이는 접근을 제시한다. AI 코딩과 자동화가 확산될수록 빌드·테스트·배포 실행 횟수는 증가하므로 CI 비용 관리가 더 중요해진다.  
- **실무 관점 코멘트:** AI Coding 도입 후 고객이 놓치기 쉬운 비용은 모델 API 비용이 아니라 “더 자주 실행되는 CI/CD 파이프라인 비용”이다. 플랫폼 엔지니어링 고객에게 **CI/CD 비용 진단, 테스트 최적화, 캐시 전략, FinOps 연계 대시보드**를 제안할 수 있다. 고객 질문은 “AI 도입 이후 PR 수, 빌드 수, 테스트 실행시간, 실패 재시도 비용이 어떻게 변했는가?”가 좋다.

---

### 7) Harness: TanStack·RubyGems 공급망 공격 분석
- **날짜/시간:** 2026-05-20 09:00 KST  
- **출처:** Harness Blog  
- **URL:** https://www.harness.io/blog/mini-shai-hulud-explained-how-the-tanstack-and-rubygems-supply-chain-attacks-worked  
- **요약:** Harness는 TanStack과 RubyGems 관련 공급망 공격 사례를 분석하며, npm, PyPI, RubyGems 같은 패키지 생태계와 CI/CD 파이프라인, 탈취된 토큰, trusted publishing 흐름이 어떻게 악용될 수 있는지 설명했다. 공격은 단일 패키지 감염을 넘어 파이프라인과 자격증명 확산으로 이어질 수 있다. 개발 속도가 빨라지고 AI가 코드·패키지 변경을 더 많이 생성하는 환경에서는 공급망 검증의 중요성이 커진다.  
- **실무 관점 코멘트:** 고객의 소프트웨어 공급망 보안은 이제 SAST/DAST만으로는 부족하다. 금융, 커머스, SaaS, 공공 SI 고객에게 **SBOM, artifact registry consolidation, token hygiene, CI/CD secret scanning, 패키지 provenance 검증**을 묶어 제안할 수 있다. 고객 미팅에서는 “AI가 생성한 dependency 추가를 누가 검토하고, 어떤 정책으로 차단하는가?”를 질문하면 좋다.

---

## 3. 자체 작성 블로그

### 제목  
**AI Agent 시대의 승부처는 모델이 아니라 ‘컨텍스트와 거버넌스’다**

### 왜 지금 중요한가  
지난 하루의 흐름을 보면 AI 시장의 무게중심이 또 한 번 이동하고 있다. Google은 I/O 2026에서 Gemini 3.5와 Antigravity, AI Search, Workspace 기능을 묶어 “에이전트가 모든 제품 표면에 들어가는 시대”를 선언했다. OpenAI는 Codex가 실제 엔지니어링 조직의 코드 리뷰와 온콜 도구 개발에 들어가고 있음을 보여줬고, Databricks는 수천 개 AI Agent가 데이터와 도구를 호출하는 상황에서 Unity Catalog 기반 거버넌스가 필요하다고 강조했다. AWS와 Google Cloud의 글도 같은 방향이다. 이제 경쟁은 “어떤 모델이 더 똑똑한가”에서 “그 모델이 어떤 컨텍스트에 안전하게 접근하고, 어떤 결과를 검증받으며, 어떤 비용 구조로 운영되는가”로 넘어가고 있다.

### 핵심 변화 3가지  
첫째, 컨텍스트 엔지니어링이 프롬프트 작성법을 넘어 데이터 플랫폼 아키텍처가 되고 있다. Google Cloud Data Agent Kit은 Claude Code, Codex, Gemini CLI 같은 개발 환경과 BigQuery, AlloyDB, Cloud Storage를 연결한다. 이는 에이전트가 업무를 수행하려면 문서 몇 개를 프롬프트에 붙이는 수준이 아니라, 권한이 통제된 데이터·도구·메모리 계층이 필요하다는 뜻이다.

둘째, AI Agent 거버넌스가 보안팀의 새 과제가 됐다. Databricks는 에이전트가 어떤 고객 PII에 접근했는지 단일하게 답하지 못하는 조직의 위험을 지적했다. AWS Security Hub의 미사용 IAM 권한 탐지도 같은 맥락이다. 에이전트가 늘어날수록 과잉 권한, 감사 부재, 비용 폭증은 더 빨리 현실 문제가 된다.

셋째, AI 도입의 품질 기준이 고도화되고 있다. AWS Strands Evals의 멀티모달 평가기는 이미지 기반 응답이 실제 이미지에 근거했는지 자동 평가한다. OpenAI의 수학 난제 사례도 중요한 메시지를 준다. AI 결과가 가치 있으려면 “그럴듯함”이 아니라 외부 검증, 재현성, 평가 체계가 있어야 한다.

### 실무자에게 미치는 영향  
Presales와 Consulting 관점에서 지금 고객에게 던질 질문은 “어떤 LLM을 쓰실 건가요?”가 아니다. “AI가 접근해야 하는 핵심 데이터는 어디에 있고, 권한은 어떻게 통제하며, 결과 품질은 어떻게 검증하고, 비용은 누가 책임집니까?”가 되어야 한다. 금융·공공·제약 고객은 거버넌스와 감사가 우선이고, 제조·유통 고객은 멀티모달 평가와 실시간 운영 자동화가 우선일 수 있다. 개발조직 고객에게는 Codex류 도구 도입과 함께 코드 리뷰, CI 비용, 공급망 보안을 묶어 제안해야 한다.

### 결론  
AI Agent 도입의 다음 단계는 화려한 데모가 아니라 운영 가능한 플랫폼이다. 컨텍스트, 권한, 평가, 비용, 감사 로그를 한 설계 안에 넣지 못하면 에이전트는 빠르게 “통제 불가능한 자동화”가 된다. 반대로 이 다섯 가지를 잘 묶으면 고객은 단순 챗봇이 아니라 실제 업무를 수행하는 AI 운영체계를 만들 수 있다. 지금이 바로 AI 플랫폼 아키텍처, Agent Governance, AI 품질 평가 체계를 제안할 타이밍이다.

### 참고 출처  
- Google I/O 2026 발표 정리: https://blog.google/innovation-and-ai/technology/ai/google-io-2026-all-our-announcements/  
- Gemini 3.5 발표: https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-5/  
- Google Cloud Data Agent Kit: https://cloud.google.com/blog/products/data-analytics/data-agent-kit-brings-data-skills-and-tools-to-your-ide-or-cli?hl=en  
- OpenAI Ramp Codex 사례: https://openai.com/index/ramp/  
- Databricks Unity Catalog Agent Governance: https://www.databricks.com/blog/governing-ai-agents-scale-unity-catalog  
- AWS Security Hub unused access: https://aws.amazon.com/about-aws/whats-new/2026/05/aws-security-hub-unused-access/  
- AWS Strands Evals multimodal evaluators: https://aws.amazon.com/blogs/machine-learning/multimodal-evaluators-mllm-as-a-judge-for-image-to-text-tasks-in-strands-evals/
