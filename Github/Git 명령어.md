Git 명령어
===

## 1. git 저장소 디렉토리 만들기

`git init` 

>만들고 싶은 저장소 위치에서 명령어를 치면 보이지 않는 .git 파일이 생성된다.

## 2. git add
`git add 파일명`

> 변경한 파일 중 올리길 원하는 것만 선택한다.




## 3. git commit
`git commit -m "설명할내용 적는곳"`
> 선택한 파일들을 묶어주어 설명 적어준다.

## 4. git remote add
> .git 디렉토리 생성한 폴더에 github 저장소 주소 입력한디

`git remote add origin https://~~`

>>origin 대신 원하는 이름을 적어도됨.

## 5. git push
> 만든 내용을 github에 올리기

`git push origin master`

>> 기본 브랜치 이름이 master

## 6. git clone
> github에 올라온 내용을 가져옴.
`git clone https://~~`