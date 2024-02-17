# Cannot install under Rosetta 2 in ARM default prefix (/opt/homebrew)! 에러

```bash
Cannot install under Rosetta 2 in ARM default prefix (/opt/homebrew)!
To rerun under ARM use:
    arch -arm64 brew install ...
To install under x86_64, install Homebrew into /usr/local.
```

이 문제는 Homebrew를 ARM 아키텍처에서 설치하려고 할 때 발생하는 것으로 보인다.

해결 방법은 다음과 같다.

<br>

**ARM 아키텍처로 설치하기**: Rosetta 2를 통해 x86_64 아키텍처로 설치하는 대신, 명령어를 사용하여 ARM 아키텍처로 설치한다. 아래 명령어를 통해 시도할 수 있다.

```bash
arch -arm64 brew install 패키지명
```
