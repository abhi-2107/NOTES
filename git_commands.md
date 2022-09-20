# Git Useful Commands

### ls 
list all the files present in repository.

### git status
shows status of files. 

### git status -s
shows status in small details.

### git add -A / git add .
adds all untracked or modified files to git.

### git add filename.xyz
*you can also add a individual file as*.  

### git commit -m "commit message"
add commit message directly like this. 
you can also do the same with **git commit** but it will pop up next window that is not efficient way, So avoid direct commit without  commit message.

### git commit -a -m "without staging the file"
this way I can directly commit a file without going to staging area. this process is not recommended. Don't do this . 

### touch filename.xyz
use *touch* command to make a file through git.

### git clone Github-link
to clone a repository from github.

### git push origin master
push file to master branch. 

### git push 
push file to last origin.

### git branch
shows all the branches available.

### git branch hasmukhbhai
By this way a branch with name of **hasmukhbhai** is *Created*.

### git checkout hasmukhbhai
checkout command will move working area from *main* branch to *hasmukhbhai* branch

### git checkout -b newbranch
make a newbranch and switch into it directly.

### git merge hasmukhbhai
merge command must be used in master/main branch. this will merge branch into main branch.

### git log 
shows all the commited messages.

### git log -p -2
to see last two commits only. you can change the number of commits you want to see (2/3/4) any number.


