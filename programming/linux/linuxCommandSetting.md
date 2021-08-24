## linux 명령어 || 설정

제가 개발하면서 자주 사용할것같은 리눅스 명령어나 설정들을 모아놓은 페이지입니다.

### root 패스워드 초기화
> https://bono915.tistory.com/entry/Linux-%EB%A6%AC%EB%88%85%EC%8A%A4-root-%ED%8C%A8%EC%8A%A4%EC%9B%8C%EB%93%9C-%EB%B6%84%EC%8B%A4%EC%8B%9C-%EC%9E%AC%EC%84%A4%EC%A0%95-root-%ED%8C%A8%EC%8A%A4%EC%9B%8C%EB%93%9C-%EC%B4%88%EA%B8%B0%ED%99%94-%EB%B0%A9%EB%B2%95?category=710433

### 아파치 서버 시작시 No such file or directory: AH02291: Cannot access directory 'etc/httpd/logs' for main error log 문구가 뜰 경우

```
mkdir /etc/httpd/logs
mkdir /var/log/httpd
chown root:root /var/log/httpd
chmod 700 /var/log/httpd
ln --symbolic /var/log/httpd /etc/httpd/logs
```

### 쉘 스크립트 작성시 command not found 발생
```
# 이렇게 하면 count: command not found 발생
count = "123"
echo "$count"

# 띄어쓰기를 제거함
count="123"
echo "$count"
```

### 유저가 root 권한얻기
```
# 아래 명령어 실행후 root 패스워드 입력
su - 
```

### 유저가 sudo 실행시 비밀번호 입력하지 않게 하기
```
# 루트로 로그인 후 아래 명령어 실행
vi /etc/sudoers

# sudoers에 아래와 같이 추가
<유저이름> ALL=NOPASSWD:ALL

# 예시
# user ALL=NOPASSWD:ALL
```

### 세션이 끊어져도 프로세스가 실행되도록 하기
```
nohup [실행하고자하는 프로그램] &

# nohou.out에 로그 안남게 실행
nohup [실행하고자하는 프로그램] 1>/dev/null 2>&1 &
```

### 프로세스 확인
```
ps -ef

# grep으로 단어를 포함하는 프로세스 찾기
ps -ef | grep [단어]
```