# Bash Scripting Notes
A Bash Shell Script is a plain text file containing a set of various commands that we usually type in the command line. It is used to automate repetitive tasks on Linux filesystem. It might include a set of commands, or a single command, or it might contain the hallmarks of imperative programming like loops, functions, conditional constructs, etc. Effectively, a **Bash script is a computer program written in the Bash programming language.**

### Build and Run Bash Script
**You can just create a file with extenstion .sh eg :- main.sh and write you code**

#### To RUN a bash/shell script there are 2 methods 

#### `Methode 1`
`Step 1` : Write you code and save as name.sh 
```bash
nano main.sh
#!/bin/bash
echo hello, world !!
```
`Step 2` : Give execute permission to the file.
```bash
chmod +x main.sh 
```
`Result`
```bash 
┌──(root💀kali)-[~/bash]
└─# chmod +x main.sh 
┌──(root💀kali)-[~/bash]
└─# ls -la
total 12
drwxr-xr-x  2 root root 4096 Jul  8 04:40 .
drwx------ 92 root root 4096 Jul  8 04:40 ..
-rwxr-xr-x  1 root root   34 Jul  8 04:40 main.sh
┌──(root💀kali)-[~/bash]
└─# 
```
`Step 3` : Execute using `./filename.sh`
```bash 
./main.sh      
```
`Result`
```bash 
┌──(root💀kali)-[~/bash]
└─# ./main.sh      
hello, world !! 
┌──(root💀kali)-[~/bash]
└─# 
```
#### `Methode 2`
`Step 1` : Write you code and save as name.sh 
```bash
nano main.sh
#!/bin/bash
echo hello, world !!
```
`Step 2` : Run script using bash
```bash 
bash main.sh        
```
`Result`
```bash 
┌──(root💀kali)-[~/bash]
└─# bash main.sh                                   
hello, world !!
┌──(root💀kali)-[~/bash]
└─# 
```

### Different print techniques 
**How to use `printf`**
```bash 
#!/bin/bash

printf "hello, world !!"
```
`Result`
```bash 
┌──(root💀kali)-[~/bash]
└─# bash main.sh
hello, world !!                                                                   
┌──(root💀kali)-[~/bash]
└─# 
```
**How to use `echo`**
```bash 
#!/bin/bash

echo "hello, world !!"
```
`Result`
```bash 
┌──(root💀kali)-[~/bash]
└─# bash main.sh
hello, world !!                                                                   
┌──(root💀kali)-[~/bash]
└─# 
```



