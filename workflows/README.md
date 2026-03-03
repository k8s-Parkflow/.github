# 🤖 GitHub-Jira 협업 자동화 가이드

이 폴더에는 우리 팀의 **개발 생산성**을 높이고, **Jira 티켓 관리**를 자동화하기 위한 GitHub Actions 워크플로우가 들어있습니다.

---

## 1. PR 제목 자동 완성 (Reusable Workflow)
**파일명:** `reusable-pr-title.yml`

### 🔍 기능
- 브랜치 이름에서 Jira 티켓 번호(예: `PROJ-10`)를 자동으로 추출합니다.
- PR을 생성하거나 수정할 때, 제목 맨 앞에 `[Jira번호]`를 자동으로 붙여줍니다.
  - *예: `feature/PROJ-10-login` 브랜치에서 PR 생성 -> 제목: `[PROJ-10] 로그인 로직 구현`*

### 🛠️ 각 서비스 레포지토리 적용 방법
각자의 레포지토리(Backend, Infra 등)에 `.github/workflows/pr-title.yml` 파일을 만들고 아래 코드를 붙여넣으세요.

```yaml
name: "PR Title Auto Format"

on:
  pull_request:
    types: [opened, edited, synchronize]

jobs:
  call-workflow:
    uses: k8s-Parkflow/.github/.github/workflows/reusable-pr-title.yml@main
```

---

## 2. 우리 팀 협업 규칙 (Convention)

자동화가 정상적으로 작동하려면 아래 규칙을 꼭 지켜주세요!

1. **브랜치 명명 규칙:** 반드시 Jira 티켓 번호를 포함해야 합니다.
   - ✅ 추천: `feature/PROJ-10-api`, `fix/PROJ-22-db-error`
   - ❌ 비추천: `feature/login-fix` (자동화가 번호를 찾지 못함)
2. **Commit 메시지:** Husky가 설치되어 있다면 브랜치 이름을 보고 커밋 메시지에도 번호를 자동으로 붙여줍니다.
3. **Jira 연동:** PR이 머지(Merge)되면 해당 Jira 티켓은 자동으로 **Done** 상태로 변경됩니다.

---

## 💡 왜 이렇게 하나요?
- **시간 절약:** 지라 보드를 일일이 옮기는 번거로움을 줄입니다.
- **추적 가능성:** 1년 뒤에도 특정 코드가 어떤 기획 의도(Jira 티켓)로 작성되었는지 1초 만에 찾을 수 있습니다.
- **DevOps 문화:** 우리 팀의 **CQRS & K8s 프로젝트** 성공을 위한 최소한의 약속입니다! 🚀
