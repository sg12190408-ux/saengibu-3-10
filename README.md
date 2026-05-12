# 3학년 10반 · 생기부 내비게이터 (GitHub Pages 배포 패키지)

학생 24명의 생기부 분석 페이지를 노션 학급 홈에 임베드하기 위한 정적 사이트입니다.

## 구조

```
saengibu-pages/
├── .nojekyll              # Jekyll 빌드 비활성화
├── robots.txt             # 검색엔진 전체 차단
├── index.html             # 학생 정보 없는 안내 홈
├── 404.html               # 슬러그 못 찾으면 정보 노출 없는 안내
└── r/
    ├── [12자리 hex]/index.html   # 학생 1
    ├── [12자리 hex]/index.html   # 학생 2
    └── ...                       # 학생 24명
```

## 보안 설계

- URL에 학번 대신 추측 불가능한 12자리 hex 슬러그 사용 (예: `/r/3b881c822eaa/`)
- 모든 페이지에 `noindex, nofollow, noarchive` 메타 + 사이트 전역 `robots.txt`
- 홈(`/`)과 404 페이지에 학생 정보 일절 노출 X
- 학생-슬러그 매핑은 별도 엑셀(`saengibu-pages_URL매핑표.xlsx`)에서만 관리. 외부 공유 금지

## 배포 순서 (웹 UI 기준 — 가장 단순)

1. **GitHub repo 생성**
   - github.com 로그인 → New repository
   - 이름 예: `saengibu-3-10`
   - **Public** 선택 (Pages는 무료 plan에선 Public 필수. 슬러그 보호로 충분히 안전)
   - "Add a README" 체크하지 말고 생성

2. **파일 업로드**
   - 새 repo 메인 화면에서 "uploading an existing file" 클릭
   - `saengibu-pages` 폴더 내 모든 파일·폴더를 한 번에 드래그 (`.nojekyll`도 누락하지 말 것)
   - Commit 메시지 자유롭게 작성 후 "Commit changes"

3. **Pages 활성화**
   - Settings → Pages
   - Source: **Deploy from a branch**
   - Branch: **main** / **/ (root)** → Save
   - 1~2분 후 상단에 `https://USERNAME.github.io/REPO-NAME/` 형태 URL 발급

4. **URL 매핑표 업데이트**
   - `saengibu-pages_URL매핑표.xlsx` 열기
   - Ctrl+H로 `USERNAME` → 본인 GitHub 사용자명, `REPO` → repo 이름으로 일괄 치환
   - F열 URL이 진짜 접근 URL이 됨

5. **노션 학급 홈에 임베드**
   - 각 학생 카드/페이지 안에 `/embed` 입력 → F열 URL 붙여넣기
   - 또는 노션 임베드 블록의 'Embed link' 입력란에 URL 붙여넣기
   - iframe 형태로 차트까지 그대로 보임

## 배포 후 확인 체크리스트

- [ ] `https://USERNAME.github.io/REPO/` 가 안내 홈으로 뜨고 학생 정보 없음
- [ ] `https://USERNAME.github.io/REPO/r/존재하지않는슬러그/` 가 404 안내로 뜸
- [ ] 학생 본인 URL이 모바일·아이패드·갤탭·노션 임베드 모두에서 차트 정상 표시
- [ ] Google에서 `site:USERNAME.github.io/REPO` 검색 시 결과 없음 (며칠 후 확인)

## 업데이트 방법

학생 페이지를 수정하면 같은 슬러그 폴더의 `index.html`을 GitHub에서 직접 편집 또는 새 파일로 업로드하면 1~2분 후 반영됩니다.

## 페이지 회수 (필요 시)

- 특정 학생 페이지만 비활성화: 해당 슬러그 폴더의 `index.html`을 빈 안내 페이지로 교체 또는 폴더 삭제
- 전체 비활성화: Settings → Pages → Source를 None으로 변경 (즉시 비공개)
