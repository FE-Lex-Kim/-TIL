# Git Branch

각각의 브랜치는 다른 브랜치들에게 영향을 끼치지 않기떄문에
여러 Branch 을 만들어서 각각 여러 작업을 동시에 할수있다.


Branch를 통해 효율적인 작업을 할수있게 된다.<br>
그이후 통합 브랜치인 `master` 브랜치에 에 각각의 Branch에서 작업한 내용을 merge(병합)한다 .


---

## 1. git branch
현재 브랜치 확인을 확인 할수있고 어떤 브랜치들이 있는지 확인 가능하다.

![git branch](../images/git%20branch/git%20branch%20사용시%20현재%20브랜치%20확인가능.gif)

Asterisk표시가 있는 브랜치가 현재위치이다.

---

## 2. git branch -a
원격 브랜치를 포함한 모든 브랜치 목록을 확인 할 수 있다

![git branch-a](../images/git%20branch/git%20branch%20-a.gif)

---

## 3. git branch ‘만들고싶은 브랜치이름’

브랜치를 생성해준다.

![git branch 생성](../images/git%20branch/git%20branch만들기1.gif)

![git branch 생성](../images/git%20branch/git%20branch만들기2.gif)

pratice 브랜치를 생성했다!.

---

## 4. git checkout ‘가고싶은 브랜치이름’

이동하고싶은 브랜치로 이동한다.

![git checkout](../images/git%20branch/../git%20branch/git%20checkout.gif)

pratice 브랜치로 이동했다.

---

## 5. git merge ‘병합할 브랜치이름’

브랜치 이름 현재 브랜치랑 병합하게 된다.

![git merge](../images/git%20branch/git_merge.gif)

---

## 6. cat '파일이름'

![cat_command](../Images/git%20branch/cat_command.gif)

---

## 7. git branch -D ‘지울 브랜치 이름’

브랜치을 지워준다.

![git_branch_-D](../Images/git%20branch/git_branch_-D.gif)

![git_branch_-D2](../Images/git%20branch/git_branch_-D2.gif)

사라져 버린 practice 브랜치..

---

## 8. mv '파일명' '폴더명/파일명'/ git mv '파일명' '폴더명/파일명'

mv / git mv command를 사용하면 파일 이름을 바꾸게 해주거나 폴더로 파일을 이동시켜준다.

---

### 8-1. git status 상태에서 보면 새로운 파일이 생기고 그전파일이 삭제된걸로 뜬다.

![mv](../Images/git%20branch/mv_사용할시.gif)

![mv](../Images/git%20branch/mv_status.gif)

-> 한눈에 파일이 폴더에 이동한것을 파악하기 힘들다.

---

### 8-2. git status 상태에서 보면 renamed로 파일이 이동한것을 보여준다.

![git_mv](../Images/git%20branch/git_mv1.gif)

![git_mv](../Images/git%20branch/git_mv.gif)

-> 한눈에 파일이 폴더에 이동한것을 파악하기 쉽다.
