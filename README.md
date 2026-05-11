# Final Project

## 📌 시작하기 전에

다음이 설치되어 있어야 합니다.

- **Git** ([다운로드](https://git-scm.com/downloads))
- **Python 3.10+** ([다운로드](https://www.python.org/downloads/))
- **GitHub 계정** ([가입](https://github.com/signup))

Windows라면 Git을 설치하면 **Git Bash**가 같이 깔립니다. 이 문서의 명령어는 모두 Git Bash 기준입니다.

---

## 📁 프로젝트 구조

```
final_project/
├── .gitignore
├── README.md
├── requirements.txt
│
├── data/                # 데이터 폴더 (실제 데이터는 gitignore)
│   ├── raw/             # 원본 
│   ├── processed/       # 전처리된 데이터
│   └── external/        # 외부 데이터
│
├── src/                 # 모듈화된 .py 코드
│   ├── __init__.py
│   ├── crawler.py
│   ├── preprocess.py
│   └── nli_pipeline.py
│
├── notebooks/           # Jupyter 노트북
│   └── exploration.ipynb
│
├── models/              # 학습된 모델 가중치 (gitignore)
│
└── outputs/             # 결과물, 시각화
    └── figures/
```

> 💡 `data/`와 `models/` 폴더 안의 실제 파일들은 GitHub에 올라가지 않습니다. 용량이 크고 개인적인 데이터이기 때문입니다. 폴더 구조만 공유돼요.
>
> 데이터가 아직 얼마나 될지 몰라서 일단 ignore에 넣긴했는데 데이터 작으면 올려도 될거같기도 함요

---

## 🚀 처음 시작하기 (팀원용)

이 저장소를 본인 컴퓨터로 복사해오는 과정입니다. **딱 한 번만** 하면 됩니다.

### 1. 본인 정보 등록 (Git 처음 쓰는 경우)

```bash
git config --global user.name "본인깃허브이름"
git config --global user.email "본인이메일@example.com"
```

### 2. 작업할 위치로 이동

원하는 폴더로 이동하세요. 예시:

```bash
cd /c/projects
```

### 3. 저장소 복사 (clone)

```bash
git clone https://github.com/dlgusvy/final_project.git
cd final_project
```

`final_project` 폴더가 자동으로 만들어지고 모든 파일이 들어옵니다.

### 4. GitHub 인증 준비

처음 푸시할 때 GitHub 로그인을 요구합니다. **비밀번호 대신 Personal Access Token**을 사용해야 합니다.

**토큰 발급 방법:**

1. GitHub 접속 → 우측 상단 프로필 클릭 → **Settings**
2. 좌측 메뉴 맨 아래 **Developer settings**
3. **Personal access tokens** → **Tokens (classic)**
4. **Generate new token (classic)** 클릭
5. Note에 이름 적기 (예: `final_project`)
6. Expiration은 원하는 기간 선택
7. **`repo`** 체크박스 클릭
8. 맨 아래 **Generate token** 클릭
9. 나타난 토큰을 **메모장에 저장** (한 번만 보임!)

이 토큰을 푸시할 때 비밀번호 자리에 붙여넣으면 됩니다.

---

## 🌿 브랜치(Branch)란?

**브랜치 = 작업용 평행세계**입니다.

`main` 브랜치는 "최종본"이라고 생각하세요. 여기에 직접 작업하면 다른 사람과 충돌하기 쉽고, 잘못된 코드가 바로 최종본에 섞입니다.

그래서 협업에서는 이렇게 합니다:

```
main (최종본)
  │
  ├── feature/crawler       ← A가 크롤러 작업
  ├── feature/preprocess    ← B가 전처리 작업
  └── feature/visualization ← C가 시각화 작업
```

각자 **자기 브랜치**에서 자유롭게 작업하고, **완성된 것만 main에 합쳐 넣는(merge)** 방식입니다.

### 왜 좋은가?

- ✅ 다른 사람 코드와 섞이지 않아 **충돌이 적음**
- ✅ 실험적인 코드를 main에 안 올려도 됨
- ✅ Pull Request로 **코드 리뷰** 가능
- ✅ 망쳐도 브랜치만 삭제하면 끝

---

## 🔄 브랜치 기반 작업 흐름 (표준)

**이 흐름을 외우면 됩니다.** 매번 새 작업 시작할 때마다 반복.

### 1️⃣ main으로 가서 최신 상태 받기

```bash
git switch main           # main 브랜치로 이동
git pull                  # 최신 코드 받기
```

> 💡 옛날 Git에선 `git checkout main`을 썼습니다. 둘 다 같은 동작이지만 `switch`가 더 명확해서 권장됩니다.

### 2️⃣ 새 브랜치 만들고 그쪽으로 이동

```bash
git switch -c 브랜치이름
```

`-c`는 "create"의 약자. **만들면서 동시에 이동**합니다.

브랜치 이름 짓는 법:

| 작업 종류 | 형식 예시 |
|----------|----------|
| 새 기능 | `feature/crawler` `feature/preprocess` |
| 버그 수정 | `fix/null-handling` |
| 문서 | `docs/readme-update` |
| 실험 | `experiment/bert-test` |

> ⚠️ 한글도 가능하지만 영어가 안전합니다. 띄어쓰기 ❌, 슬래시(`/`)로 카테고리 구분.

### 3️⃣ 작업하기

자유롭게 코드를 짜고 파일을 수정합니다.

### 4️⃣ 커밋하기

```bash
git status                       # 무엇이 바뀌었는지 확인
git add .                        # 모든 변경사항 선택
git commit -m "한 일 설명"        # 로컬에 저장
```

**커밋 메시지 예시:**
- ✅ `리뷰 크롤러 함수 추가`
- ✅ `전처리 버그 수정`
- ❌ `수정함` (뭘 수정했는지 모름)

### 5️⃣ 내 브랜치를 GitHub로 보내기

```bash
git push -u origin feature/crawler
```

- `-u origin feature/crawler`: **처음 푸시할 때만** 붙이는 옵션. "이 브랜치를 GitHub의 같은 이름 브랜치와 연결해줘"라는 뜻.
- 같은 브랜치에서 두 번째부터는 그냥 `git push`만 해도 됩니다.

### 6️⃣ GitHub에서 Pull Request(PR) 만들기

웹브라우저에서 GitHub 저장소로 이동:

1. 푸시 직후엔 노란 띠로 **"Compare & pull request"** 버튼이 보입니다. 클릭.
   - 안 보이면 **Pull requests** 탭 → **New pull request** → 본인 브랜치 선택
2. 제목 작성 (예: `크롤러 기능 추가`)
3. 본문에 어떤 작업을 했는지 적기
4. **Create pull request** 클릭

### 7️⃣ 팀원과 리뷰 → main에 합치기(merge)

- 팀원이 코드를 보고 의견을 남깁니다.
- 문제 없으면 **Merge pull request** 버튼 클릭 → **Confirm merge**
- 이러면 내 브랜치가 main에 합쳐집니다 🎉

### 8️⃣ 정리

main으로 돌아와서 최신 상태 받고, 다 쓴 브랜치는 삭제:

```bash
git switch main
git pull
git branch -d feature/crawler     # 로컬 브랜치 삭제
```

GitHub의 원격 브랜치는 PR 머지 화면에서 **Delete branch** 버튼으로 삭제할 수 있습니다.

---

## 📋 명령어 한눈에 보기

### 매일 쓰는 것

| 명령어 | 의미 |
|--------|------|
| `git switch main` | main 브랜치로 이동 |
| `git pull` | 받아오기 |
| `git switch -c feature/이름` | 새 브랜치 만들고 이동 |
| `git status` | 내 변경사항 확인 |
| `git add .` | 변경사항 전부 선택 |
| `git commit -m "메시지"` | 로컬에 저장 |
| `git push` | GitHub로 보내기 |
| `git push -u origin 브랜치명` | 새 브랜치를 처음 보낼 때 |

### 가끔 쓰는 것

| 명령어 | 의미 |
|--------|------|
| `git branch` | 로컬 브랜치 목록 보기 |
| `git branch -a` | 모든 브랜치 보기 (원격 포함) |
| `git switch 브랜치명` | 다른 브랜치로 이동 |
| `git branch -d 브랜치명` | 로컬 브랜치 삭제 |
| `git log --oneline` | 커밋 기록 보기 |
| `git diff` | 마지막 커밋 이후 변경된 내용 보기 |

---

## 💡 자주 만나는 상황

### 📍 푸시했더니 `rejected` 에러가 떠요

```
! [rejected] feature/crawler -> feature/crawler (fetch first)
```

→ 같은 브랜치에 다른 사람이 먼저 푸시했는데 나는 못 받은 상태.

```bash
git pull
git push
```

### 📍 내가 어느 브랜치에 있는지 헷갈려요

```bash
git branch
```

별표(`*`)가 찍힌 게 현재 위치입니다.

```
* feature/crawler
  main
```

### 📍 main 브랜치에 실수로 커밋했어요 (push 전)

당황 금지. 새 브랜치를 만들어 옮기면 됩니다:

```bash
git branch feature/임시이름     # 현재 위치에 브랜치 생성 (이동 X)
git reset --hard origin/main    # main을 GitHub 상태로 되돌리기
git switch feature/임시이름     # 만들어둔 브랜치로 이동
```

이제 내 변경사항이 새 브랜치에 살아 있고, main은 깨끗합니다.

> ⚠️ `git reset --hard`는 강력합니다. **이미 push한 main**에는 절대 쓰지 마세요. 위 상황은 "내 로컬 main에만 잘못 커밋"한 경우입니다.
### 📍 커밋 메시지를 잘못 적었어요 (push 전)

**가장 최근 커밋의 메시지만 수정:**

```bash
git commit --amend -m "올바른 메시지"
```

> 💡 이미 push한 커밋의 메시지는 이 방법으로 바꾸지 마세요. 히스토리가 꼬입니다.

---

### 📍 방금 한 커밋을 취소하고 싶어요 (push 전)

상황에 따라 3가지 옵션이 있습니다. **위에서 아래로 갈수록 위험**합니다.

**① 커밋만 취소, 변경사항은 그대로 유지 (가장 안전, 추천)**

```bash
git reset --soft HEAD~1
```

→ 커밋이 "스테이징된 상태"로 되돌아갑니다. 메시지만 다시 쓰고 싶을 때 유용.

**② 커밋 + 스테이징 취소, 파일 변경은 유지**

```bash
git reset HEAD~1
```

→ `git status`에서 "변경됨" 상태로 돌아갑니다. `add`부터 다시 해야 함.

**③ 커밋 + 모든 변경 통째로 날리기 ⚠️ 위험**

```bash
git reset --hard HEAD~1
```

→ **변경사항이 영구 삭제됩니다.** 정말 다 버리고 싶을 때만 사용.

> 💡 `HEAD~1`은 "한 커밋 전". `HEAD~2`, `HEAD~3`처럼 숫자를 늘리면 그만큼 더 거슬러 올라갑니다.

---

### 📍 이미 push한 커밋을 되돌리고 싶어요

`reset`은 절대 쓰지 마세요. 다른 사람의 히스토리가 망가집니다. 대신 **`revert`** 를 씁니다.

```bash
git log --oneline                  # 되돌릴 커밋의 해시 확인 (예: a1b2c3d)
git revert a1b2c3d                 # 해당 커밋을 취소하는 "새 커밋"을 만듦
git push
```

`revert`는 기존 커밋을 지우지 않고, **"그 커밋을 반대로 한 새 커밋"** 을 추가합니다. 히스토리가 안전하게 유지됩니다.

---

### 📍 특정 파일만 이전 상태로 되돌리고 싶어요

**아직 커밋 안 한 변경사항 버리기:**

```bash
git restore 파일명
```

**스테이징(`add`)만 취소 (파일 내용은 유지):**

```bash
git restore --staged 파일명
```

**특정 커밋 시점의 파일로 되돌리기:**

```bash
git restore --source=a1b2c3d 파일명
```

---

### 📍 reset을 잘못 써서 작업물이 사라진 것 같아요

당황 금지. Git은 30일간 모든 동작을 기록합니다.

```bash
git reflog
```

지금까지의 HEAD 이동 기록이 다 보입니다:

```
a1b2c3d HEAD@{0}: reset: moving to HEAD~1
e4f5g6h HEAD@{1}: commit: 중요한 작업
...
```

복구하고 싶은 시점의 해시를 골라:

```bash
git reset --hard e4f5g6h
```

→ 그 시점으로 돌아갑니다. **`reflog` 덕분에 거의 모든 실수는 복구 가능합니다.**
### 📍 충돌(conflict)이 났어요

PR을 만들 때 GitHub가 "This branch has conflicts" 라고 알려주거나, `git pull` 시 이런 메시지가 뜹니다:

```
CONFLICT (content): Merge conflict in 파일명
```

**해결 방법:**

1. 해당 파일을 열면 다음과 같이 표시되어 있습니다:

   ```
   <<<<<<< HEAD
   내가 쓴 내용
   =======
   친구가 쓴 내용
   >>>>>>> main
   ```

2. 직접 편집해서 최종 모습으로 만들고, `<<<`, `===`, `>>>` 표시는 **모두 삭제**합니다.

3. 그다음:

   ```bash
   git add 파일명
   git commit -m "충돌 해결"
   git push
   ```

### 📍 커밋하다 갑자기 이상한 화면(Vim)에 갇혔어요

당황하지 말고 순서대로:

1. `Esc` 키
2. `:wq` 입력
3. `Enter`

다음부터는 `git commit -m "메시지"`로 메시지를 같이 적으면 안 갇혀요.

### 📍 실수로 파일을 add 했어요 (아직 commit 전)

```bash
git restore --staged 파일명
```

### 📍 다른 사람 브랜치를 받아오고 싶어요

```bash
git fetch                          # 원격 정보 업데이트
git switch 친구브랜치이름            # 자동으로 따라가는 로컬 브랜치 생성
```

### 📍 PR 머지 후 main으로 돌아왔는데 변경사항이 안 보여요

```bash
git pull
```

머지된 내용을 로컬 main으로 받아와야 보입니다.

---

## ✅ 팀 약속 (꼭 지켜요)

1. **main에 직접 푸시 금지** — 항상 브랜치를 만들고 PR로 합치기.
2. **작업 시작 전엔 무조건 main에서 `git pull`** — 충돌의 80%가 여기서 생깁니다.
3. **하나의 브랜치엔 하나의 목적만** — "크롤러 + 시각화 + 버그 수정" 다 한 브랜치에 넣지 말기.
4. **커밋 메시지는 한국어로 짧게라도 의미 있게** — "수정함" ❌, "리뷰 데이터 전처리 함수 추가" ✅
5. **데이터/모델 파일을 직접 커밋하지 않기** — `.gitignore`가 알아서 막아주지만 강제로 `git add -f`는 하지 말 것.
6. **머지된 브랜치는 정리하기** — 안 쓰는 브랜치가 쌓이면 헷갈립니다.
7. **모르면 물어보기** — 푸시 잘못해서 꼬이는 것보다, 멈추고 물어보는 게 빠릅니다.

---

## 🧭 처음 며칠은 이렇게 연습하세요

브랜치가 익숙해질 때까지 다음 흐름을 **외울 때까지** 반복:

```bash
# 1. main 최신화
git switch main
git pull

# 2. 새 브랜치 만들기
git switch -c feature/내작업

# 3. 작업...

# 4. 저장
git add .
git commit -m "한 일 설명"
git push -u origin feature/내작업

# 5. GitHub에서 PR 만들고 머지

# 6. 끝나면 정리
git switch main
git pull
git branch -d feature/내작업
```

---

## 🆘 정말 막혔을 때

1. `git status` 결과를 그대로 복사해서 팀원에게 공유
2. 어떤 명령어를 쳤고, 어떤 에러가 나왔는지 정확히 알려주기
3. **함부로 폴더를 지우거나 강제로 명령어 실행하지 말 것** — 작업물을 잃을 수 있습니다.
4. 특히 다음 명령어들은 **위험**하니 모를 땐 쓰지 마세요:
   - `git reset --hard`
   - `git push --force`
   - `git clean -fd`

---

## 📚 더 배우고 싶다면

- [Git 공식 한글 문서](https://git-scm.com/book/ko/v2)
- [GitHub 공식 가이드](https://docs.github.com/ko)
- [생활코딩 Git 강의](https://opentutorials.org/course/3837)
- [Learn Git Branching (시각화 학습)](https://learngitbranching.js.org/?locale=ko)

---

**Happy coding! 🚀**