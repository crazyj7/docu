# Git simple guide  
---
<p style="text-align:right">crazyj7@gmail.com</p>  

## Start git repository download
> git clone *http://github.com/crazyj7/...git*  
> git clone *ssh://git@192.168...:2222/home/git/repos/....git*  

## ready
```
git config --global user.name "YourName"
git config --global user.email "email@test.com"
```
설정 내역은 ~/.gitconfig 폴더에 저장된다.
```
git config --list
설정 내역 확인
```

## Git Server : init
### git server 생성

ubuntu 기준.
git-core 패키지를 설치한다.
ppa 사용시.
```
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get install git-core
```
- 사용 방식
	- ssh 계정을 사용자 별로 만들어 그룹핑하여 접근제어를  하는 방식
	- 대표 계정을 하나 두고 공유하는 방식. (사용자별 접근제어가 되지 않음.)
```
대표 계정 생성
sudo adduser git
su - git
mkdir repos
```
---


## 패스워드 인증 생략 방법. ssh-keygen
- pull을 할 때마다 패스워드를 입력해야 되는 것은 귀찮다.  인증생략 방법은 없을까?

- 사용자들로 부터 ssh 키를 생성하게 하고 pub key를 받아 계속 추가해 주면 여러명이 사용가능. 윈도우는 git for windows를 설치하면 위 커맨드 사용 가능. 패스워드를 그냥 엔터로 입력해도 가능.
```
> ssh-keygen
패스워드 입력해도 되고, 그냥 (empty) 엔터쳐도 된다.
> %userprofile%\.ssh 폴더가 생성되고 파일들이 추가됨.
> id_rsa, id_rsa.pub 가 생기는데. pub를 메모장으로 열어서 복사하거나, 
 git 서버로 보낸다.
```
```
git서버에서 로그인 계정에 아래 추가. 권한설정도 해주어야 함.
mkdir .ssh
chmod 700 .ssh
touch  ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```
```
사용자들의 공개키를 계속 등록해 주면 된다. (위에 복사한 내용을 라인 단위로 붙여넣기하면 된다.)
cat id_rsa.pub >> ~/.ssh/authorized_keys
```
참고로 key pair 생성은 윈도우의 경우는 putty-keygen을 사용하면 된다고 한다. 또는 윈도우 git을 설치하였으면 ssh-keygen이 포함되어 있어서 커맨드로 생성 가능하다.

### ssh keygen 에러 케이스

>* 윈도우 한글계정인 경우 인코딩문제로 깨져서 파일 생성이 안되는 경우가 있다.
> ssh-keygen 으로 할 때 첫 번째 질문 저장위치에 한글이 깨져서 발생하는데, 그냥 id_rsa로 입력하면 현재 폴더에 생성할 수 있다. 다음 질문은 패스워드 설정이니 그냥 엔터로 입력.
```
> ssh-keygen
저장위치 ...xxx.../id_rsa? id_rsa  (현재폴더에 생성)
패스워드 .... ? [엔터]
```
> * 생성된 파일들을 %userprofile%\.ssh 폴더에 복사한다.

이렇게 하고 git 서버에 정상적으로 authorized_keys에 pub를 추가해줬는데도 안되면?
로컬에 ssh-agent가 문제가 있을 수 있다.
> * ssh-agent를 실행한다. 커맨드창에 ssh-agent 라고 입력. 상태확인은 뒤에 -s 옵션
> 에러 메시지 1058이 뜬 경우. powershell을 실행하여 
```
> ssh-agent 
실행할수없다. error 1058.
> powershell
>> set-service ssh-agent -startuptype manual
> ssh-agent
> ssh-add id_rsa
> ssh 계정@IP
패스워드 없이 로그인 되면 성공

다시 자동서비스 시작으로 설정
>> set-service ssh-agent -startuptype automatic
```

---


## 개발 전에 먼저 빈 저장소 생성,  git 서버에서 빈 저장소 만들기
디렉터리를 저장소명.git 으로 생성한다. 
``` 
su - git
cd ~/repos
mkdir testproject.git
cd testproject.git
```
해당 폴더로 이동하여 아래 커맨드를 실행
``` git init --bare --shared ```
shared 옵션은 같은 그룹에  쓰기 권한을 추가해 준다.

**그냥 일반적으로 ```git init --bare```를 사용한다**. (bare 옵션을 줘야 함. 그래야 공유를 위한 기본적인 파일 및 디렉터리들이 구성됨.) 

서버에서 생성한 빈 git 저장소를 받는다. ```git clone```
프로젝트를 생성하여 소스 파일들을 작성한다음 최초 커밋을 한다.
```
git clone ssh://id@IP:port/home/git/repos/testproject.git
cp ~/work/myfolder/testproject/*.cpp testproject/
...
cd testproject
git add *.cpp *.h ...
git commit -m "init"
git push origin master
```

## <font color=red>개발 중에 저장소를 만들어 올리기!!!</font>
> 처음부터 저장소만들어서 빈 저장소를 받고 작업하기 보다는 보통은 개발 부터 먼저 로컬에서 하다가 어느 정도 되면 서버로 올리는 경우가 많다...
> 이 때, 빈 저장소를 서버에 만들어 로컬에서 받고, 작업중인 프로젝트 파일들을 거기에 다 옮겨넣고 커밋푸시하는 방법이 있긴 하지만, 사본을 만들고 옮기고 하는 등 번거롭고 귀찮다.  더 간단한 방법이 있다. ...

**일단, 서버에 빈 저장소를 생성하는 것은** 동일하다.
```
사용할 계정으로 로그인: 예)git_server
/home/git_server$ mkdir myproject.git
/home/git_server$ cd myproject.git
/home/git_server/myproject.git$ git init --bare
기본 파일들이 구성됨.
```
로컬 git 저장소를 만들어 현재 프로젝트를 올린다.
이클립스의 경우 쉽게 왼쪽뷰에서 올린 프로젝트를 선택하여 우클릭에서 <font color=red>**Team-Share Project** 선택</font>
로컬 저장소를 만들어 준다.
```예를 들면 저장소를 d:\git\myproject```

**커밋 과 Push** (Team-Commit)
반영할 파일들을 Unstaged Changes에서 찾아 +시킨다. 그럼 Staged로 들어간다.
커밋 메시지를 작성하고 Commit and Push 버튼을 한다.

**원격 git 서버 저장소로 연결**
URI를 작성하여 로그인 계정 정보를 넣으면 서버로 올라간다.
```
ssh://git_server@1.2.3.4:22/home/git_server/home/myproject.git
```
github 같은 경우는
```
https://github.com/yourgithubid/myproject.git
```






## Work
### make new branch and work
최신 master 브랜치를 받아서 작업 브랜치를 만들고 작업한다.  
> git branch *alpha*  
> git commit *alpha*
> [work to do....]
> 
### commit and push
변경된 파일들을 index표시를 하고 커밋한다. 그 후 원격 서버에 올린다.  
> git status .  
> git add -u    
> git add *[files/dirs]*  
> git commit -m *"update message"*  
> git push  *(or git push origin master)*  
> 
### merge to master  
마스터 브랜치를 기준으로 새 작업 브랜치를 가져와 **병합** 한다.  
병합후 변경내역을 원격 서버에 올린다.  

> git branch  : 현재 브랜치 확인   
> git **checkout *master***  
현재 브랜치가 master로 변경됨  
> git **merge *alpha***    
현재 브랜치 기준으로 alpha 브랜치를 가져와 병합한다.  
> git push  

## rollback
### commit 하기전에 이제까지 작업한 파일을 **원래대로** 되돌리기(**Warn!**)  

>  <font color="red"> git checkout *[file] or [. (current dir)]*  </font>

### 마지막 커밋은 취소하고 수정했던 것도 전부 취소하기(Warn!)  
> *cancel last commit*  
>  <font color="red"> git **reset --hard HEAD^**  (HEAD~1 in Windows)  </font>
 <font color="red">**주의!!! 수정내역이 없어지므로 필요시 백업!**</font>
위 옵션에서 **--hard 옵션을 빼면 커밋만 취소하고 수정 파일은 그대로 유지**한다.  
보통은 아래와 같이 soft reset으로 로컬의 수정사항은 유지한다.
> git reset HEAD^  
> 작업 후, 다시 commit 

Cancel Merge (**병합 취소**)  
merge후 conflict나고 처리가 복잡할 경우 바로 이전으로 돌아가기    
>  <font color="red"> git merge --abort  </font>

### git commit 메시지 변경하기 (amend)
```
> git commit --amend
에디터로 내용 수정후
> git log -1
확인
또는 커맨드로 바로 메시지 설정도 가능
> git commit --amend -m "hello"
```

---

### History
GUI로 히스토리를 확인할 수 있다.
> gitk  

한글이 깨질 경우 *hangul encoding* 변경 후, 다시 시작
> git config --global gui.encoding utf-8  

---

### Errors
##  <font color="red"> pull fail? </font>
cancel all worked here. cancel commit.  
마지막 커밋한 변경 내역을 되돌리고, 다시 pull로 원격에서 업데이트한다.
**(Warn! 주의!!! 작업내용삭제됨!!!! 일단 백업해 두는 것을 추천.)** 
>  <font color="red"> git **reset --hard HEAD^**   (HEAD~1 in Windows) or HEAD   
> git pull  </font>

### Stash
그래도 pull이 실패한다면 stash로 작업내역을 백업하고 다시 받는다.  
> git stash ; 현재 작업한 내역을 백업. 
> git pull ; 현재 HEAD를 받아온다. 
> 
> git stash list ; 백업한 파일 목록 보기.
> git stash pop ; 백업한 내역을 가져온다. 
> git pull

참고  
> git reset --soft HEAD~ 는 HEAD를 한 커밋 이전으로 이동시킨다.  
> git reset HEAD~ 는 HEAD 이동 및 index도 이동한다.  
> git reset --hard HEAD~ 는 HEAD 이동, index 이동, working dir도 이전으로 돌린다.  
> git stash 는 reset을 하지만 변경내역은 백업을 해 둔다. 파일명 지정시 git stash save *[file]* , git stash list , git stash pop/apply 로 불러오기. **엉뚱한 브랜치에서 작업을 한 경우 이것을 원래 브랜치로 이동**시킬 때 사용가능    


#### cancel git add/  git add 취소하기?
> git reset [file/dir]  
뒤에 대상 미지정시 전체 취소


## <font color="red"> conflict?  충돌!!!
</font>

*edit conflicted file and git add/ git commit  
or choose one side*  

작업하고 commit을 하고 원격으로 push하는데 conflict가 발생할 수 있다. 이것은 원격에 이미 checkout받은 브랜치가 변경되었다는 것이다.  
> git add -u ; git commit -m "update" : 커밋하여 푸시 준비.
> git push : 푸시하는데 리모트에서 거부 발생!!! 
> git pull  : 최신 정보를 받아와 반영해야 한다. 
> conflict!!! 자동병합 충돌 발생!!! 충돌 파일 확인!!    
> git status : 충돌파일 정보    
> 파일이 병합처리되어 >>>(원격) <<<(로컬) 등 문자 포함  
> 이 때는 파일을 **수정/병합 하고 다시 커밋**하고 푸시하는 방식이 있고, **어느 한 쪽을 선택**할 수도 있다.  

충돌 발생시 충돌 파일을 수정하든지 어느 한 쪽을 골라야 한다.  
현재 브랜치 기준으로 내것과 원격의 것을 잘 구분해야 한다.  
#### 한 쪽을 선택하는 경우 conflict resolve
> <font color="red">git checkout --ours [file] </font> : 로컬파일이 진짜다. 로컬 우선!!!   
> <font color="red">git **checkout --theirs [file]**  </font> : **서버에 있는 최신것을 따르겠다. (주의!!! 로컬 파일 내용은 없어짐!!!!  로컬을 무시하겠다.)**
> 위와 같이 하고 git pull을 한다.   
#### 병합을 하는 경우 conflict resolve
> *or edit file (양쪽을 모두  반영하겠다.  >>> <<< 부분을 찾아 파일 수정 )*  
> git add -u  
> git commit -m *"update"*  
> git push  
> 
#### cancel delete local file
**파일을 잘못 삭제**했을 경우 복구하는 법  
> git status .  ; check which file is deleted.  
> git checkout HEAD [file]  

#### cancel work  
> git reset .  ; reset all worked  
> git checkout .  ; go before work  

---

### Ignore files
If you dont want to upload some files or dirs,
create **.gitignore** file in the current directory.  
add files or dirs  
ex)    cat > .gitignore
> build/  
> \*.obj
> dataset/
> \*.o

---

### Branch merge
> * fast-forward    :  가장 편한 경우. 럭키. 할 게 없음.
> only one branch is changed or it doesn't exist same file changed.)     
> a -> b(master) : no change    
>      b(newbranch) -> x ->y(bugfix)  
> merge : a->b->x->y (master=bugfix)
> git push  *or*  git pull   
특별히 겹치는 부분이 없으면 자동으로 충돌없이 병합된다.  

> * merge commit    
> Both branches are changed. Update conflict files and commit  
>  <font color="red">git **checkout *master*** : **마스터를 가져와서** (항상 메인을 기준으로)    
> git **merge *alpha*** : **알파를 불러와 현재(마스터)와 합친다.**    </font>  conflict나면 해당 파일을 수정하고 병합한다.  여러 브랜치가 하나의 브랜치(현재 브랜치)로 합쳐진다. **과거 히스토리 유지**.    


> * **rebase**    
> a -> b -**> c -> d** (master)     
> b -> **x -> y** (alpha(bugfix))  
> 위 버그 수정 브랜치를 마스터로 반영하고, **마스터 브랜치만 유지**하고 싶다. 그런데 버그패치동안에 마스터가 변경된 상태다.   
> merge: a->b->c->d **->x->y** (master=bugfix)  
> git **checkout alpha**   : 수정 브랜치를 가져와서  
> git **rebase master**  : 마스터에 연결시도     
> *update conflict files* : 충돌발생 파일 수정(c,d,x,y)       
> git **add -u**    
> git **rebase --continue**  : 리베이스 계속 진행    
> git **checkout master** : 마스터를 가져와서    
> git **merge alpha**   : 알파를 현재로 불러와서 병합.  
베이스를 새로 지정한다. 위의 **브랜치 트리가 한 줄로** 만들어진다. 과거 히스토리 변경 주의.     


### Delete branch
브랜치 삭제 -d 옵션으로 안되면 -D 옵션으로 한다.   ( **tag, branch 모두 삭제는 -d 옵션**을 붙여주면 된다. 또는 --delete)
> git branch -d *[alpha]*  
---


### Delete file
로컬 및 저장소에서 파일 삭제  
> * Remove the file in both local and remote github  
<font color="red">
> git rm *[file]*  
</font>

저장소에서만 삭제하고 **로컬 파일은 보존**  
> * Remove remote file in github but remain it in local  
<font color="red">
> git **rm --cache** *[file]*  
</font>

### Delete all .git files
리눅스에서 모든 .git 폴더 제거하기(incude subdir)  
> check files...  
> find . -name '.git'  
> WATCHOUT!!!  
> find . -name '.git' -prune -exec rm -rf {} +  

윈도우에서 모든 .git 폴더 제거하기(incude subdir)  
> batch file create  
FOR /r "c:\temp" %%f IN (.git) DO RD /s /q "%%f"  

삭제하기 전에 목록 출력만(rd /s /q 대신 echo) 해서 확인을 하는 것이 좋다.  


## Repository mirror (저장소를 다른 서버로 이전)
저장소 복사
> git clone --mirror [가져올 URL] 
> git push --mirror [가져욜 URL]  

### <font color=red>git remote -v</font>
### <font color=red>git remote set-url --push origin [URL] </font>
```
# git remote -v 
원격지 저장소 주소 확인

# git clone --mirror https://user@a.c.com/maypp/test.git
먼저 옮길 내용을 clone --mirror 으로 받는다. (git 구조를 받음)

올릴 곳의 주소로 설정
# cd test.git
# git remote -v
# git remote set-url --push origin http://local.com/mobile/android.git

옮길 서버에는 git init --bare로 초기화된 저장소를 만들어 준다.
$ mkdir android.git
$ cd android.git
$ git init --bare

이제 push --mirror로 올린다....
# git push --mirror
이제 로컬의 내용을 변경된 주소로 올린다. (git 구조를 올림)

```

## Tag
태그는 버전명을 주거나 할 때 지정해 준다.

- 먼저 checkout으로 받고, 그 안으로 들어가서 작업한다.
- 현재 갖고 있는 태그 목록
 ```
 git tag
 ```
- 현재 받은 checkout에 **tag 설정**하고 push로 올림.
 ```
git tag v1.0
git push origin v1.0
-- 이렇게 해서 서버로 올린다.
```
 
- 태그로 받기
 ```
git checkout v1.0
```

- 태그 삭제 (-d)  
 ```
git tag -d v1.0
```

### 태그 v.1.0을 수정할 때는 1.1 브랜치를 만들어서 작업
### **작업 예제**

태그로 **가져와서**
```git checkout v1.0```
**브랜치를 만들고**
```git branch 1.1```
<font color="red">**Warn!: 해당 브랜치로 전환** (이걸 안해주면 소스 날릴 수도) 음 기억하자 branch checkout </font>
```git checkout 1.1```
<font color="red">
**요약: 복잡하니 간단하게 새로운 브랜치 작업시에는 아래만 기억**
``` git checkout -b new브랜치명 ```
</font>

이제 소스코드 작업을 하고 다 끝나면...
```git add -u
git commit -m "patch 1.1"
git push
```

> 브랜치를 원격에 올리려면...
> git push origin 브랜치명
> git branch --set-upstream-to origin/브랜치명

-master로 위 브랜치 합치기
 ```
마스터로 가서
git checkout master
브랜치 1.1을 가져와 머지함
git merge 1.1
git status
충돌파일 편집 수정
git add -u
git commit -m "merge 1.1"
git push
```
 ```
 태그를 추가하여 push로 올림
 git tag v4.0
 git push origin v4.0
```


### ETC
- github 인증 (개인 access token 방식)
1. github 로그인 : https://github.com/login
2. profile  - settings - developer settings - personal access tokens 메뉴
3. generate token 버튼 클릭
4. repo를 체크하고 generate token 클릭.
5. 생성된 token 스트링 복사.
6. 위 token 스트링을 암호 대신 사용
7. git clone, pull 등 커맨드 사용시 위 토큰으로 인증해 두면 다음부터 자동인증이 됨.

- **core.autocrlf 기능** 꺼주기 : (리눅스에서 코드에 ^M이 붙는 현상 방지)
	- 이미 ^M이 붙어 있다면, notepad++ 같은 편집기에서 UNIX EOL로 수정하여 다시 commit하면 해결된다. 
```
$ git config --global core.autocrlf false
```

## Compare/비교하기

### <font color=red>버전간 비교하기</font>
- 최신 버전과 옛날 특정 버전을 비교.
최신 버전을 받아놓은 상태에서 실행
```git diff 비교버전```
(구버전은 커밋해시값을 사용해도 된다.)
- 위와 같은데 특정 파일에 대해서만 diff 사용.
```git diff 비교버전 파일명```



### branch 간 비교하기

- master와 note 두 브랜치의 각각 최신 버전끼리 비교하기
```
git diff master..note
```

- note와 master가 마지막으로 같았던 old 버전에서 note의 최신 버전을 비교하기 (즉, note 브랜치만의 작업 내역)
```
git diff master...note
```

### vimdiff
기본으로 제공되는 vimdiff 사용법
git diff 대신 git difftool 커맨드를 사용. 
git merge 대신 git mergetool 커맨드를 사용.

^+w+w ; 화면 이동 (마지막 w대신 방향키를 사용해도 가능)
[ + c  ; 앞쪽 이동 
] + c  ; 뒤쪽 이동, next compare  (next diff position) 
d+p ; 현재 창 채택. diff put. (밀어넣기. 현재 기준의 블록을 반대쪽에 복사)
d+o ; 다른 창 채택. diff obtain. (가져오기)
:diffupdate ; diff를 업데이트하기. (화면 갱신)


### 더 좋은 diff 외부 툴: p4merge
- P4Merge 설치 방법.
[https://teddylee777.github.io/git/study-git-2](https://teddylee777.github.io/git/study-git-2)
- 다른 것은 다 uncheck하고 merge and diff tool 만 체크하여 설치한다.

환경 변수에 C:/Program Files/Perforce 를 추가
아래 커맨드 실행
```
git config --global diff.tool p4merge
git config --global difftool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
git config --global difftool.prompt false
```

```
git config --global merge.tool p4merge
git config --global mergetool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
git config --global mergetool.prompt false
```

git diff 대신 git difftool 커맨드를 사용. 
git merge 대신 git mergetool 커맨드를 사용.


## 로그 보기
### 커밋 버전과 메시지 확인
git log
commit [커밋명]
로그 정보
반복

### 변경된 파일명 보기
*마지막 커밋&Push에서 변경된 파일이 뭐지?*
<font color="red">**git log --name-only -1**</font>
(마지막 커밋에서 변경된 파일 목록 조회)
또는
git log --name-only
(커밋별로 변경된 파일 목록 조회)


### 커밋한 것을 그 전 버전과 변경된 부분 보기
*마지막 커밋&Push에서 변경된 내용이 뭐지?*
<font color="red">**git log -p -1**</font>
git log [커밋명] -p -1
(커밋명은 앞에서 구분가능할 정도인 일부 몇 글자만 써도 된다.)

> git log에서 본 메시지를 보고 커밋명을 확인한 다음 몇 글자만 추가.
git log 463a -p -1
위 당시 커밋버전은 그 전과 비교하여 작업한 내용이 diff로 출력된다.
(-)는 이전 소스 내용. (+)는 해당 버전 소스 내용.

### 변경된 코드 보기
git diff
<font color="red"> git diff ^HEAD </font> ; git add 한 경우.

### 이전 파일 보기
git log [파일명] ; 해당 파일명 변경. commit hash
git show  ;  현재 커밋 정보 보기. commit hash
git show HEAD^:./파일명
git show HEAD^^:./파일명
git show [커밋해시]:./파일명
git checkout [커밋해시] 파일명



## 이전으로 돌아가기 rollback

커밋을 많이 했는데, 그 전으로 돌아가기
이전의 돌아갈 버전을 확인.

### Reset
최근의 몇 개 커밋을 삭제하여 돌아간다. (실제 삭제하지는 않는다?)
git reset --hard 버전 (이 버전으로 돌아가고, 이 버전의 다음 버전들은 모두 삭제)
버전은 커밋명. 
HEAD는 최신버전, 
HEAD~1은 마지막 커밋을 잘못한 경우 사용한다. (최신 이전 버전으로 돌아갈 때.)  이미 push를 해서 다른 사람이 pull을 했다면, 이 방식은 문제가 될 수 있음. 공지가 필요.

### Revert
최근 커밋들은 그대로 유지한채, 이전 버전을 가져와 최신으로 커밋한다. (즉, 돌아가긴 하지만 히스토리를 남겨둔다.)
git revert 버전

## 현재 폴더에서 git 대상 파일인가?
현재 폴더에서 관리대상 파일들
```git ls-files```

현재 폴더에서 관리대상 파일이 아닌 목록
<font color="red">
```git ls-files --others```
</font>



## <font color=red>특정 버전으로 돌아가기.</font>
```
# 다운로드
git clone ....
# 로그 확인. 커밋 확인
git log
commit 해시1
로그1
commit 해시2
로그2
...
# 해당 커밋으로 돌아감.
git checkout 커밋해시값
```
- 최신 버전과 비교하려면 windiff 같은 것으로 directory compare를 수행.
- 커밋 단계별 변화부분 확인시에는 github desktop 프로그램을 사용하면 편리하다.
- 



---
> Author : crazyj7@gmail.com


<!--stackedit_data:
eyJoaXN0b3J5IjpbNjgwNjgwMzA2LC0xODYyNzk3MzE0LDc5Mj
YwMDk2NiwtMTQ3NzA5OTkxMCwxMDk4NTMxODczLDI2MTY4ODE5
NiwtNTg0NTg5MTc0LDE4NTk4NTc5NTgsMjk1ODIwNzM2LC0xMT
UzNDQ2NDIxLDczNjI1MDcxMSwtMTg1MTUxNTM3MywtOTM4ODUw
MTUsLTMxMTcyMDA5NCw5MjczMzU0NzEsLTEzMTExMTc4MTYsND
MwMzY1NDkzLDE2Nzk4MTU0MjYsLTQ4Njc0MTY3OCw3MjcxOTQ1
ODFdfQ==
-->