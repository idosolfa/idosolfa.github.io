# Personal Blog

[Hugo](https://gohugo.io/)와 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 테마로 만들고, GitHub Actions로 GitHub Pages에 배포합니다.

## 처음 설정

PaperMod 테마는 Git 서브모듈로 들어 있어서 클론할 때 함께 받아야 합니다.

```bash
git clone --recurse-submodules https://github.com/idosolfa/idosolfa.github.io.git
```

이미 클론했는데 `themes/PaperMod` 폴더가 비어 있다면:

```bash
git submodule update --init --recursive
```

Hugo는 [.github/workflows/hugo.yaml](.github/workflows/hugo.yaml)의 `HUGO_VERSION`과 같은 버전을 쓰세요.

```bash
brew install hugo   # macOS
hugo version
```

## 자주 쓰는 명령어

### 새 글 쓰기

```bash
# 파일 하나로 → https://idosolfa.github.io/posts/my-first-post/
hugo new content posts/my-first-post.md

# 이미지도 같은 폴더에 두고 싶을 때 (페이지 번들)
hugo new content posts/my-first-post/index.md
```

- 파일(폴더) 이름이 그대로 글 주소가 됩니다. 영문 소문자와 `-`로 지으세요.
- 제목은 파일 이름으로 자동 생성되니(`My First Post`), 만든 뒤 front matter의 `title`을 원하는 제목으로 고치세요.
- 새 글은 `draft: true`(초안)로 만들어집니다. **공개하려면 `draft: false`로 바꿔야 합니다.**

### 미리보기

```bash
hugo server        # http://localhost:1313/ , 저장하면 자동 새로고침
hugo server -D     # 초안(draft: true)까지 포함해서 보기
hugo list drafts   # 아직 초안인 글 목록
```

### 배포

`main` 브랜치에 push하면 GitHub Actions가 빌드해서 배포합니다. 진행 상황은 [Actions 탭](https://github.com/idosolfa/idosolfa.github.io/actions)에서 볼 수 있습니다.

```bash
git add .
git commit -m "post: OO 글 추가"
git push
```

push 전에 배포와 같은 방식으로 빌드해 보려면:

```bash
hugo build --gc --minify   # 결과물은 public/에 생기며 git에는 올라가지 않습니다
```

> 저장소를 새로 만들었다면 **Settings → Pages → Source**를 **GitHub Actions**로 한 번 설정해야 배포됩니다.

### 테마 업데이트

```bash
git submodule update --remote --merge themes/PaperMod
hugo server                # 화면이 깨지지 않았는지 확인
git add themes/PaperMod
git commit -m "chore: PaperMod 테마를 최신 버전으로 업데이트"
```

### Hugo 버전 올리기

로컬 Hugo와 워크플로우의 `HUGO_VERSION`은 **항상 같이** 올리세요. 버전이 다르면 로컬에서는 되는데 배포에서만 깨질 수 있습니다.

```bash
brew upgrade hugo
hugo version   # 이 버전으로 .github/workflows/hugo.yaml의 HUGO_VERSION을 수정
```

## 폴더 구조

```text
.
├── .github/workflows/hugo.yaml   # 빌드·배포 워크플로우
├── archetypes/default.md         # 새 글 템플릿
├── content/posts/                # 글
├── themes/PaperMod/              # 테마 (Git 서브모듈)
└── hugo.yaml                     # 사이트 설정
```

테마는 직접 수정하지 마세요. 서브모듈 안을 고치면 업데이트할 때 충돌하고 배포에도 반영되지 않습니다. 바꾸고 싶은 파일은 프로젝트의 `layouts/`나 `assets/`에 같은 경로로 만들어 덮어쓰세요.
