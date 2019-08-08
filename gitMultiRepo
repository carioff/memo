svn 은 하나의 repository 만 설정할 수 있지만 git 은 여러개의 repository 를 설정할 수 있다.

git remote add alt alt-machine:/path/to/repo
remote 가 http/https 일경우 다음과 같이 설정하면 된다.

git remote add alt https://github.com/carioff/msolver.git
인증이 필요할 경우 매번 id 와 암호를 입력해야 하므로 id 정도는 url 에 같이 포함시키는 것이 편리하다. id와 @ 를 도메인 앞에 붙여 주면 된다.

git remote add alt https://github.com/carioff/msolver.git
 

모든 remote 에서 branch 및 update 된 내역을 가져오려면 다음 명령어 실행(HEAD 에 merge 하지는 않는다.)

git remote update
alt remote 의 master branch 에서 fetch 하고 현재 HEAD 에 pull

git pull alt master

확인 
$ git remote -v

Git push가 안되는 경우 (fatal: refusing to merge unrelated histories)
git pull origin 브런치명 --allow-unrelated-histories
