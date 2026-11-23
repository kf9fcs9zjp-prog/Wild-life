# GitHub Pages 배포 (준비 완료 · 저장소 생성 권한만 남음)

`gh`(Pickle GitHub App) 로 API 를 호출해 봤지만 저장소 생성이 거부되었습니다.

```
$ gh repo create kf9fcs9zjp-prog/wild-life --public
GraphQL: Resource not accessible by integration (createRepository)
```

원인: 연결된 Pickle GitHub App 설치 권한에 **Administration(저장소 생성) 과 Pages 권한이 없다.**
현재 부여된 권한은 `contents:write, issues:write, pull_requests:write, actions:write, checks:read, metadata:read, statuses:read` 뿐이다.
그래서 `POST /repos/{owner}/{repo}/pages` (Pages 활성화) 도 같은 이유로 불가능하다.

## 남은 절차 (사용자 1분 + 명령 3줄)

1. 폰/PC 브라우저에서 https://github.com/new → Repository name **wild-life**, **Public**, README 추가 없이 Create.
2. 이 폴더에서:
   ```bash
   cd work/wild-life-pages
   git branch -M main
   git remote add origin https://github.com/kf9fcs9zjp-prog/wild-life.git   # 이미 있으면 생략
   git push -u origin main
   ```
3. Pages 켜기 — `gh` 로 한 번 시도하고, 거부되면 저장소 설정에서:
   ```bash
   gh api repos/kf9fcs9zjp-prog/wild-life/pages -X POST -f 'source[branch]=main' -f 'source[path]=/'
   ```
   실패 시 GitHub 웹 → Settings → Pages → Source: **Deploy from a branch** → Branch: **main** /(root) → Save.
4. https://kf9fcs9zjp-prog.github.io/wild-life/ 가 HTTP 200 인지 확인.

## 폴더 내용
- `index.html` — 게임 본편(단일 파일, PWA 메타·매니페스트·아이콘 링크 포함)
- `icon.png`(512) · `icon-192.png` · `apple-touch-icon.png`(180) · `manifest.webmanifest`
- `.nojekyll` — Jekyll 처리 없이 그대로 서빙

아이폰: 사파리로 주소를 연 뒤 **공유 → 홈 화면에 추가** → WILD LIFE 아이콘으로 전체화면 실행.
