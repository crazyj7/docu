# Git simple guide  
---
<p style="text-align:right">crazyj7@gmail.com</p>  

## Start git repository download
> git clone *http://github.com/crazyj7/...git*  
> git clone *ssh://git@192.168...:2222/home/git/repos/....git*  

## Work
##### make new branch and work
최신 master 브랜치를 받아서 작업 브랜치를 만들고 작업한다.
> git pull  
> git branch *alpha*  
> [work to do....]
##### commit and push
변경된 파일들을 index표시를 하고 커밋한다. 그 후 원격 서버에 올린다.  
> git status .  
> git add -u    
> git add *[files/dirs]*  
> git commit -m *"update message"*  
> git push  *(or git push origin master)*  
##### merge to master  
마스터 브랜치를 기준으로 새 작업 브랜치를 가져와 **병합** 한다.  
병합후 변경내역을 원격 서버에 올린다.  
현재 브랜치 확인  
> git branch  
> git checkout *master*  
현재 브랜치가 master로 변경됨  
> git merge *alpha*    
현재 브랜치 기준으로 alpha 브랜치를 가져와 병합한다.  
> git push  

##### rollback
작업한 파일을 **원래대로** 되돌리기(Warn!)  
> git checkout *[file] or [. (current dir)]*  

마지막 커밋은 취소하고 수정했던 것도 전부 취소하기(Warn!)  
> *cancel last commit*  
> git reset --hard HEAD^  (HEAD~1 in Windows)  
위 옵션에서 --hard 옵션을 빼면 커밋만 취소하고 수정 파일은 그대로 유지한다.  
> git reset HEAD^  

Cancel Merge (병합 취소)  
merge후 conflict나고 처리가 복잡할 경우 바로 이전으로 돌아가기    
> git merge --abort  


### History
GUI로 히스토리를 확인할 수 있다.
> gitk  

한글이 깨질 경우 *hangul encoding* 변경 후, 다시 시작
> git config --global gui.encoding utf-8  


### Errors
#### pull fail?
cancel all worked here. cancel commit.  
변경 내역을 되돌리고, 다시 pull로 원격에서 업데이트한다.**(Warn! 주의!!! 작업내용삭제됨!!!! 일단 백업해 두는 것을 추천.)** 
> git **reset --hard HEAD^**   (HEAD~1 in Windows) or HEAD   
> git pull  

그래도 pull이 실패한다면 stash로 작업내역을 백업하고 다시 받는다.  
> git stash
> git pull

참고  
> git reset --soft HEAD~ 는 HEAD를 한 커밋 이전으로 이동시킨다.  
> git reset HEAD~ 는 HEAD 이동 및 index도 이동한다.  
> git reset --hard HEAD~ 는 HEAD 이동, index 이동, working dir도 이전으로 돌린다.  
> git stash 는 reset을 하지만 변경내역은 백업을 해 둔다. 파일명 지정시 git stash save *[file]* , git stash list , git stash pop/apply 로 불러오기. 다른 브랜치에 잘못 작업한 것을 원래 브랜치로 이동시킬 때 사용가능    


#### cancel git add?
> git reset [file/dir]  

#### conflict?
*edit conflicted file and git add/ git commit  
or choose one side*  
충돌 발생시 충돌 파일을 수정하든지 어느 한 쪽을 골라야 한다.  
현재 브랜치 기준으로 내것과 원격의 것을 잘 구분해야 한다.  
> git checkout --ours [file]  
> git checkout --theirs [file]  
> *or edit file*  
> git add -u  
> git commit -m *"update"*  
> git push  
#### cancel delete local file
파일을 잘못 삭제했을 경우 복구하는 법  
> git status .  ; check which file is deleted.  
> git checkout HEAD [file]  

#### cancel work  
> git reset .  ; reset all worked  
> git checkout .  ; go before work  


### Ignore files
If you dont want to upload some files or dirs,
create .gitignore file in the current directory.  
add files or dirs  
ex)     
> build/  
> \*.obj
> dataset/
> \*.o
>

### Branch merge
> * fast-forward    
> only one branch is changed or it doesn't exist same file changed.)     
> a -> b(master) : no change    
>      b(newbranch) -> x ->y(bugfix)  
> merge : a->b->x->y (master=bugfix)
> git push  *or*  git pull   
특별히 겹치는 부분이 없으면 자동으로 충돌없이 병합된다.  

> * merge commit    
> Both branches are changed. Update conflict files and commit  
> git **checkout *master*** : 마스터를 가져와서     
> git **merge *alpha*** : 알파를 불러와 현재(마스터)와 합친다.    
충돌이 나면 해당 파일을 수정하고 병합한다.  여러 브랜치가 하나의 브랜치(현재 브랜치)로 합쳐진다. 과거 히스토리 유지.    


> * rebase    
> a -> b -**> c -> d** (master)     
> b -> **x -> y** (bugfix)  
> 위 버그 수정 브랜치를 마스터로 반영하고, 마스터만 유지하고 싶다. 그런데 버그패치동안에 마스터가 변경된 상태다.   
> merge: a->b->c->d **->x->y** (master=bugfix)  
> git **checkout alpha**   : 수정 브랜치를 가져와서  
> git **rebase master**  : 마스터에 반영   
> *update conflict files* : 충돌발생 파일 수정       
> git **add -u**    
> git **rebase --continue**   
> git **checkout master**   
> git merge alpha   
베이스를 새로 지정한다. 위의 브랜치 트리가 한 줄로 만들어진다. 과거 히스토리 변경 주의.     


### Delete branch
브랜치 삭제 -d 옵션으로 안되면 -D 옵션으로 한다.  
> git branch -d *[alpha]*  

### Delete file
로컬 및 저장소에서 파일 삭제  
> * Remove the file in both local and remote github  
> git rm *[file]*  

저장소에서만 삭제하고 **로컬 파일은 보존**  
> * Remove remote file in github but remain it in local  
> git **rm --cache** *[file]*  


### Delete all .git files
리눅스에서 모든 .git 폴더 제거하기(incude subdir)  
> find . -name '.git' -prune -exec rm -rf {} +  

윈도우에서 모든 .git 폴더 제거하기(incude subdir)  
> batch file create  
FOR /r "c:\temp" %%f IN (.git) DO RD /s /q "%%f"  

삭제하기 전에 목록 출력만(rd /s /q 대신 echo) 해서 확인을 하는 것이 좋다.  


---
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTgyNDAzNzkxLDEwMTkyNjY0NTldfQ==
-->