# Git Tutorial

### Initial configuration (SSH)

**You have to create one ssh key using `ssh-keygen` copy the is_rsa.pub and paste in** [Settings of SSH ssh-key](https://github.com/settings/keys)



### Git Config Command     `Step 1`

```shell
git config --global user.name "Your name"

git config --global user.email "Your email"
```
```
note : if you want to set a text editor default like sublime or vscode use 

optional : git config --global core.editor "subl --wait"

for opening that code : git config --global -e
```

```shell 

for the issue of the new line we have to set core.autocrlf 'autocrlf command is used to change how Git handles line endings'

** For Mac **

git config --global core.autocrlf input

** For Windows **

git config --global core.autocrlf true

```

### Git Init Command     `Step 2`

```shell 

'To initialize a repository and  Git creates a hidden directory called .git which y stores all of the objects and refs that Git uses and creates as a part of your projects history'

git branch -m master

git init 
```

### Git Add Command     `Step 3`
```shell
git add README.md

git add . 

or 

git add <file names>

'for checking status'

git status

```

### Git Commit Command     `Step 4`

```shell

git commit -m "what is actualy u done eg : - updates code, bug fixed"

git status 

then create one repo in you github.com and copy the ssh link.
'this must be clean'

git push --set-upstream git@github.com:mr4dot/crypto_notes.git master


```


### OR 
```shell 
echo "# crypto_notes" >> README.md

git branch -m master

git init

git add README.md

git commit -m "first commit"

git remote add origin git@github.com:mr4dot/crypto_notes.git

# first upload
git push -u origin master 

# if force upload use -f 

git push -u -f origin master
```