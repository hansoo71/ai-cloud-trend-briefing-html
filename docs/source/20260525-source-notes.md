# Source notes — 2026-05-25

수집 창: 2026-05-24 00:00 ~ 2026-05-25 07:50 KST.

- `date`로 KST 현재 시각을 확인했다.
- 공식 RSS(OpenAI, AWS What's New/ML/DevOps, Google Cloud, Google AI, Azure Blog, Databricks, Harness, NVIDIA, Microsoft AI)를 먼저 확인했으나 창 내 신규 공식 포스트는 거의 없었다.
- Google News RSS는 discovery layer로 사용했고, 주요 Google News wrapper는 browser `document.location.href`로 게시자 URL을 해소했다.
- 원문 접근이 제한적인 StartupHub 항목은 feed metadata 수준으로만 보수적으로 사용했다.
- TechCrunch, InfoQ, Chosunbiz, Cybersecurity Insiders, Pulse2, Tech Times, iTnews, Quasa, G2, HackerNoon 등은 직접 브라우저 또는 r.jina.ai/urllib 추출을 통해 본문 핵심을 확인했다.

## Resolved canonical URLs
- AWS MCP Server GA: 모든 AWS API와 IAM 기반 거버넌스로 AI 코딩 에이전트 연결: https://www.infoq.com/news/2026/05/aws-mcp-ga/
- 한국 금융권, AI 보안 활용을 위해 망분리 규제 한시 완화 추진: https://biz.chosun.com/en/en-finance/2026/05/24/B7EH2EQ4ABC7NA6WH6ATQ5RX3Q/
- Google Cloud COO: AI 보안은 플랫폼 접근이 필요하며 “22초 공격 전환”에 대응해야 한다: https://techcrunch.com/2026/05/24/everyone-is-navigating-ai-security-in-real-time-even-google/
- CJ올리브영, 전사 Gemini Enterprise 이후 내부 AI Sandbox로 비개발자 도구 제작 확대: https://www.techtimes.com/articles/317070/20260524/olive-young-builds-internal-ai-sandbox-non-developer-staff-now-build-their-own-tools.htm
- CBA, AWS frontier AI agent로 새벽 온콜 DevOps 장애 대응 보조: https://www.itnews.com.au/news/cbas-devops-agent-is-helping-on-call-engineers-on-2am-wake-up-duty-626037
- Shadow AI 격차: 직원 80%는 비승인 AI를 쓰지만 공식 거버넌스는 12%: https://www.cybersecurity-insiders.com/shadow-ai-tools-governance-gap/
- Cranium AI, Aiceberg 인수로 Agentic AI 거버넌스·보안 역량 확대: https://pulse2.com/cranium-ai-acquires-aiceberg-to-expand-agentic-ai-governance-capabilities/
- Flowise: 오픈소스 시각형 Low-code AI Agent·RAG 플랫폼 리뷰: https://quasa.io/video/flowise-open-source-visual-low-code-platform-for-ai-agents-rag-comprehensive-review
- AI Security Posture Management 도구 6종 비교: AI-SaaS 상호작용 가시화가 핵심: https://learn.g2.com/best-ai-security-posture-management-tools
- GitLab-Google Cloud 협력 확대: Gemini 모델을 Duo Agent Platform에 통합: https://www.harianbasis.co/en/gitlab-expands-google-cloud-alliance
- 개발자가 2026년에 갖춰야 할 AI 역량: 프롬프트보다 RAG·평가·운영 이해: https://hackernoon.com/5-must-have-ai-skills-for-developers-in-2026-and-how-to-learn-them
- AI 리스크가 개인 업무 리스크가 되는 시대: 도구 사용·데이터 반출·품질 책임: https://startuphub.ai/5-ai-risks-that-can-get-you-fired/
- Google Cloud-Thales 독일 Sovereign Cloud: 규제 데이터의 전용 인프라·현지 통제: https://www.harianbasis.co/en/google-cloud-thales-germany-sovereign