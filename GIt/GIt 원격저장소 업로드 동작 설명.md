# Git 원격 저장소 업로드 동작과정.

github의 remote repository에 push하는 과정의 단계로는 아래가 있다.

1. workspace
2. staging index
3. local repository
4. remote repository

---

## 1. `workspace`는 `.git` 디렉터리 바깥의 프로젝트 폴더를 의미한다.

workspace에서 add을 통해 staging index로 파일에 등록을 하게 된다.

workspace 에서 index로의 이동 index에서 local repository로 이동 그이후 remote repository로 파일을 올릴수 있다.


## 2. `staging idnex`

worespace에서 add로서 `local repository`에 `commit`하기전에 대기상태에 있게해준다.

## 3. `local repository`

staging index에서 commit을 하게되어 `local repository`로 옮기게된다. 파일에대한 커밋내용을 올리고 참고할수 있게한다.

## 4. `remote repostitory`

`local repository` 에서 `push`하여 
Github에 본격적으로 올리게되는 마지막 단계이다.

----

## 쉽게정리하게 되면.

workspace -> staging index -> local repository -> remote repository

command : `add` -> `commit` -> `push`
