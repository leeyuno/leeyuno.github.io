# leeyuno.github.io — 개발자 사이트

개인 개발 앱들의 소개 페이지, 개인정보처리방침·약관, AdMob `app-ads.txt`를 한곳에서 호스팅합니다.
GitHub Pages가 `main` 브랜치 루트를 Jekyll로 빌드합니다(플러그인 없음). 푸시하면 1~2분 뒤 반영됩니다.

> 2022년 모바일 청첩장은 루트에서 내렸습니다. 파일은 git 기록(커밋 `0e12a67`)에 남아 있습니다.

## 구조

```
_config.yml           개발자 이름·문의 메일·기본 시행일·AdMob 게시자 ID (모든 문서가 이 값을 씀)
_data/apps.yml        앱 목록 → 홈 카드, 앱 페이지 헤더·메뉴
_data/doc_titles.yml  문서 이름(privacy 등) → 메뉴 제목
_layouts/             default(공통 틀), app(앱 소개 페이지)
app-ads.txt           _config.yml의 admob_publisher_ids로 생성 → /app-ads.txt
index.html            홈 (앱 목록)
<slug>/index.md       앱 소개 (= 지원 URL로도 사용)
<slug>/privacy.*      개인정보처리방침
<slug>/terms.* …      그 밖의 문서
_templates/new-app/   새 앱 폴더 템플릿 (빌드 제외)
assets/icons/<slug>.png   앱 아이콘 256px
```

## 앱별 URL

| 앱 | 소개 / 지원 | 개인정보처리방침 | 비고 |
|---|---|---|---|
| 먹싸자 | `/BabyLog/` | `/BabyLog/privacy/` | ⚠️ iOS·Android 앱 설정 화면이 이 경로를 엽니다. 폴더 이름을 바꾸지 마세요 |
| 동네왕관 | `/dongne-crown/` | `/dongne-crown/privacy/` | 같은 내용을 앱 서버 `/privacy`도 보여 줍니다 |

## 새 앱 추가하기

1. `_templates/new-app/`을 `<slug>/`로 복사하고, 파일 안의 `SLUG`와 앱 이름을 바꿉니다.
   - slug는 URL이 되므로 한 번 정하면 바꾸지 않습니다(앱과 스토어에 링크가 박힘).
2. 아이콘: `sips -Z 256 AppIcon-1024.png --out assets/icons/<slug>.png`
3. `_data/apps.yml`에 항목을 추가합니다. `pages`에는 폴더에 있는 문서 이름을 적습니다.
4. 개인정보처리방침을 앱이 실제로 처리하는 데이터에 맞게 채웁니다(앱의 `PrivacyInfo.xcprivacy`, 스토어 개인정보 라벨과 일치).
5. 그 앱이 다른 AdMob 계정을 쓰면 `_config.yml`의 `admob_publisher_ids`에 추가합니다(같은 계정이면 할 일 없음).
6. 출시되면 `apps.yml`의 `status: released`, `app_store`/`play_store` 주소를 채웁니다.

## 문서 원본

방침·약관은 각 앱 저장소의 원본을 옮겨 왔습니다. 원본을 고치면 이 저장소도 함께 고칩니다(각 파일 머리말에 원본 위치가 있습니다).

- 먹싸자: `leeyuno/BabyLog` `docs/privacy.md`, `docs/terms.md` (`{{ site.app_name }}` → 먹싸자)
- 동네왕관: `leeyuno/dongne-crown` `server/src/legal/*.html` (`{{OPERATOR_NAME}}` 등 → `site.*`)

## 로컬 미리보기 (선택)

```sh
gem install jekyll && jekyll serve   # http://127.0.0.1:4000
```

## 배포 확인

```sh
curl -sI https://leeyuno.github.io/app-ads.txt | head -1     # 200
curl -s  https://leeyuno.github.io/app-ads.txt               # google.com, pub-..., DIRECT, f08c47fec0942fa0
```
