# 깃헙 여러 계정 등록

## 환경
- Windows 11

1. ssh key 생성

```
$ ssh-keygen -t rsa -b 4096 -C [GITHUB_EMAIL]
$ Enter file in which to save the key : [id_rsa_personal | id_rsa_company]
```

개인 계정의 키와 회사 계정의 키를 생성

2. ssh add

```
$ ssh-add [id_rsa_personal | id_rsa_company]
```

만약 Could not open a connection to your authentication agent. 에서가 나온다면

```
$ eval $(ssh-agent)
```

위 명령어 입력 후 다시 시도하면 잘됨

3. 등록된 키 조회

```
$ ssh-add -l
```

4. ssh key 클립보드 복사

```
$ clip <~/.ssh/id_rsa_company.pub
```

5. 깃헙 계정 설정에서 ssh 등록

Settings > SSH Keys로 가서 복사한 내용 저장

6. ssh config 설정

~/.ssh 에서 config 파일 생성후 아래 내용 추가

```
# personal
Host github.com-self
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa

# company
Host github.com-work
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_company
```


7. 연결 확인

```
$ ssh -T company-github.com
```


8. gitconfig 설정

~/.gitconfig에 전역 설정

```
[user]
    name = [personal_name]
    email = [personal_email]
[includeIf "gitdir:~/Company"]
    path = .gitconfig-company
```

Company 디렉토리이면 ~/.gitconfig-company 파일을 불러온다
```
[user]
    name = [company_name]
    email = [company_email]
```

위와 같이 사용하면 ~/Company 디렉토리에서는 회사 계정을 사용하게 된다.