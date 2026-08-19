# 설문 링크 모음 웹앱

학생들이 각자 만든 설문(구글폼 등) 링크를 직접 등록하고, 다른 학생들은 누가 만들었는지 알 수 없는 상태로 목록에서 골라 참여할 수 있는 정적 웹앱입니다.

## 화면 구성

- `index.html` — 홈 화면. "링크입력" / "설문진행" 두 버튼.
- `link-input.html` — 학생이 반, 번호, 이름, 설문 링크를 입력해 등록.
- `survey.html` — 등록된 설문 링크 목록(반/번호/이름 비공개, 링크만 표시)을 보여주고 참여하도록 연결.

## 데이터 저장

Supabase(Postgres)를 백엔드로 사용합니다.

- `public.survey_links` 테이블: `class_name`, `student_number`, `student_name`, `link` 저장. RLS로 익명(anon) INSERT만 허용하고, 직접 SELECT는 막혀 있습니다.
- `public.public_survey_links` 뷰: `id`, `link`, `created_at`만 노출하는 익명화된 뷰. `survey.html`은 이 뷰만 조회합니다.

클라이언트에는 Supabase anon key가 포함되어 있지만, RLS 정책으로 보호되므로 공개되어도 안전합니다.

## 배포 (GitHub Pages)

`.github/workflows/deploy-pages.yml` 워크플로가 `main` 브랜치에 푸시될 때 자동으로 GitHub Pages에 배포합니다.

최초 1회, 저장소 **Settings → Pages → Build and deployment → Source**를 **GitHub Actions**로 설정해야 합니다 (API로는 설정할 수 없어 수동으로 한 번 해주셔야 합니다).
