# E325: ATTENTION 에러 처리

<br>

![E325: ATTENTION 에러 처리](../Images/E325-%20ATTENTION%20에러%20처리/E325-%20ATTENTION-1.png)

<br>

만약 git 작업하던도중 강제종료 당하거나 멈추었을때 강제로 나가게 되면 이렇게 기존의 작업 하던 commit이 남아 있어 처리해달라고 에러가 발생한다.

<br>

## 에러 처리방법

<br>

1. Found a swap file by the name 뒤의 `"~/dev/Algorithm/.git/.COMMIT_EDITMSG.swp"` 이부분을 기억해 놓는다.
2. 저 경로로 들어가서 .swp 파일을 삭제해준다

   2-1 만약 저 파일이 보이지 않는다면 숨김파일 이므로 `ls -all` 을 해서 찾으면 된다.

   2-2 `rm [파일이름]` 을 해주면 파일이 삭제된다.
