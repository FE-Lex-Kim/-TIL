# git rebase

<br>

브랜치가 나온 경우 master와 합치려고 할때 merge와 rebase를 사용할 수 있다.

merge와 rebase를 비교하면서 어떤점이 다른지와 그리고 rebase가 무엇인지 알아보자.

<br>

## merge

![git rebase](../Images/git%20rebase/git%20rebase-1.png)

<br>

c2에서 iss53 브랜치를 만들어서 c3,c5 commit을 만들었다.

이때 기존의 master에 있는 c4도 commit을 진행한 상태이다.

<br>

따라서 iss53 브랜치의 base는 c2가 되고 이때 maser와 merge 작업을 하려고 할때 충돌이 일어난다.

c4와 c5의 충돌작업을 수행해서 master에 **merge한 새로운 로그가 남게 된다.**

<br>

따라서 git History를 보게 되었을때 위와같이 여러가지 가지가 나오고 그에대한 가지치기를 하게된 형태가 나오게된다.

<br>

## rebase

![git rebase](../Images/git%20rebase/git%20rebase-2.png)

<br>

마찬 가지로 c2에서 브랜치 experiment를 만들었다.

c2를 base로한 experiment 브랜치는 c4 commit을 만들었고, master 또한 c3를 만들었다.

<br>

![git rebase](../Images/git%20rebase/git%20rebase-3.png)

<br>

experiment와 master와 merge를 수행할때 c4와 c3가 충돌하게 된다.

이때 충돌작업을 처리하고 그대로 master history에 이어 붙인다.

<br>

이전에 experimnet 브랜치의 history는 없어지고 master의 history에 commit이 그대로 이어 붙여져 깔끔한 history 관리가 가능하다는 점이 있다.

<br>

### 명령어

`git rebase <basebranch> <topicbranch>`

topicbranch의 브랜치 commit history를 그대로 basebranch history에 붙인다는 의미이다.(basebranch가 rebase 대상임)

<br>

### 주의사항

이때 깃은 푸시를 하지 못 하고 에러를 낸다. origin 서버에 있는 내용과 현재 로컬에 있는 내용의 머지 베이스가 다르기 때문이다.

<br>

master는 base가 항상 동일하므로 문제가 나지 않으므로 다른 브랜치들을 master에 리베이스 해야한다.

만약 브랜치들을 리베이스 한채로 원격 저장소에 push하면 문제가 발생한다.

해당 브랜치를 다시 pull 받아 작업한후 리베이스를 해 push하게 됬을때, 원격저장소에 저장된 브랜치의 base와 현재 브랜치의 base가 다르기 때문에 에러가 난다.

<br>

위의 이유로 브랜치가 공용 브랜치 인경우는 리베이스를 사용하는 것을 권장하지 않는다.

머지 베이스가 서로 다른 상태에서 origin에 있는 내용을 풀 받거나 푸시를 할 때 복잡한 이슈가 생긴다.

만일 현재 작업하는 브랜치가 혼자 작업하는 브랜치가 아니라면 master와 리베이스 없이 사용을 하다가 꼭 필요한 경우만 리베이스를 하기를 권장 한다.

단 작업이 완료가 된 후 마지막 master에 머지하기 전에는 꼭 리베이스를 해야한다.

<br>

## 정리

- git history가 깔끔하게 유지되어서 협업에 좋다.
- 항상 rebase를 사용하는 것이아니라 주의사항에 따라 merge와 섞어 사용한다.

<br>

참고

- [https://tech.10000lab.xyz/git/git-rebase-workflow.html](https://tech.10000lab.xyz/git/git-rebase-workflow.html)
- [https://git-scm.com/book/ko/v2/Git-브랜치-Rebase-하기](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)
- [https://firework-ham.tistory.com/12](https://firework-ham.tistory.com/12)
- [https://brunch.co.kr/@anonymdevoo/7](https://brunch.co.kr/@anonymdevoo/7)
- [https://dongminyoon.tistory.com/9](https://dongminyoon.tistory.com/9)
- [https://www.youtube.com/watch?v=a6YhT82QaO0](https://www.youtube.com/watch?v=a6YhT82QaO0)
