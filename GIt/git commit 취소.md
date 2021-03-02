# git commit 취소

git 작업시 commit을 실수했을때 취소할때가 있다.

<Br>

## commit 을 취소하는 커맨드

```bash
git reset --mixed HEAD^
$ git reset HEAD^
```

커밋중 가장 최근의 커밋을 삭제한다.

<Br>

## commit message 수정하는 커맨드

```bash
$ git commit --amend
```

<Br>

참고

- [https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)
