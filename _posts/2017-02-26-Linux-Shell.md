---
layout: post
title: "Linux Shell"
description: "Notes on Linux shell."
date: 2017-02-26
tags: [programming, Linux, notes]
comments: true
share: true
---

### 《Linux Shell Scripting Cookbook》

1. `echo` and `printf`

	Do not include ! within double quotes when use `echo`<br>
	`$ echo "hello!"`
	
	This command will return<br>
	`bash: !: event not found error`
	
	There are several equivalent ways to use `echo`:<br>
	`$ echo "hello"; echo hello; echo 'hello'`.
	
	Usage of `printf` is the same as that in C.
	
2. Environment variables (p.6)

	1. Check the environment variables that process uses<br>
      `$ cat /proc/$PID/environ`.<br>
      To look for PID for process, use `top` or<br>
      `$ pgrep [process name]`.
   2. Assign value to variables<br>
  	   `var=value`<br>
  	   It is different than `var = value` which is a mistake and means equality
  	   
  	    ```
  	    fruit=apple
  	    count=5
  	    echo "We have $count ${fruit}(s)"
  	    ```
  	   
   3. Use `export` to set environment variables<br>
   	   `$ export PATH="$PATH:/home/user/bin"`
   	   
   4. Use a user-defined function to set environment variables<br>
      We can add the following to `.bashrc`<br>
      `prepend() {[ -d "$2" ] && eval $1=\"$2':'\$$1" && export $1; }`.(Every space is crucial.)<br>
      Then we could use<br>
      `$ prepend PATH /home/user/bin`<br>
      to accomplish the same task.
      
      `[-d "$2"]` tests whether the second parameter refers to existent directory or not<br>
      `eval $1=\"$2':'\$$1"` adds `:` in two parameters<br>
      
      One problem is that if the first parameter is empty, then there will be an additional `:` at the end. To solve it,<br>
      `prepend() { [ -d "$2" ] && eval $1=\"$2\$\{$1:+':'\$$1\}\" && export $1; }`,
      where<br>
      `${parameter:+expression}`
      equals expression when parameter is not empty.

   5. Obtain the length of variable<br>
   		`$ var=12345660`<br>
   		`$ echo ${#var}`
   6. Test whether the current user is superuser(Root user's UID is 0)

      ```
      If [ $UID -ne 0 ]; then
          echo Non root user. Please run as root.
      else
          echo Root user
      fi
      ```

3. Concatenating files with `cat` and redirection

   `$ cat [-estuv][file]`
   
    Create a file:<br>
    `$ cat > foo.txt`<br>
    [type what you want] and then press Ctrl+D to save and exit

    Copy one file to another file<br>
    `$ cat < foo.txt > foo2.txt`         <— overwrite file foo2.txt <br>
    `$ cat < foo.txt >> foo2.txt`        <— put after (append the output)
    
    An example to write file when content is inside the script:
    
    ```
    #!/bin/bash
    cat<<EOF>log.txt
    LOG FILE HEADER
    This is a test log file
    EOF
    ```
    
    More usage of `cat`: (p.42)
      1. Cancatebate files<br>
      `$ cat file1 file2 file3 ...`
      2. Together with pipe<br>
      `$ echo "text"|cat - file1`
      (here `-` represents `stdin`)
      3. Add line numbers at the beginning<br>
      `$ cat -n file`
      
   Redirection (p.14)
   
       1. `0---stdin`;`1---stdout`;`2---stderr`
       2. `$ echo "text" > temp.txt`
       3. `$ cmd 2>stderr.txt 1>stdout.txt`

      
4. Array and associate array(like map in C++)

   Array is indexed by integers, i.e., 0,1,2,...<br>
   Declaration:<br>
   `$ declare array`  or<br>
   `$ array=(test1 test2 test3)`.<br>
   Then, use `$ echo ${array[0]}` or `$ echo ${array[*]}` to print all the elements, or `$ echo ${#array[*]}` to print the number of elements.

	Associate array is indexed by strings.<br>
	Declaration:<br>
	`$ declare -A ass_array`<br>
	Then, to define the associate array, use<br>
	`$ ass_array=([index1]=val1 [index2]=val2)` or independently<br>
	`$ ass_array[index1]=val1`<br>
	`$ ass_array[index2]=val2`.<br>
	Then, use `$ echo ${ass_array[*]}` to print all the values and `$ echo ${!ass_array[*]}` to print all the indexes.
   
5. Alias

   Example: create an alias for `apt-get install`:<br>
   `$ alias install='sudo apt-get install'`
   
   Alias are temporal and will be cleared when terminal is exited. If you want to make them permanently, put them in `~/.bashrc'`. If you want to delete alias, use `unalias`. And if the alias is already exits when declaring, then the old one will be replaced.
   
   For security, if you are sure not to use alias, put `\` in front of your commands.
   
6. A time taken sript

   ```
   #!/bin/bash
   start=$(date +%s)
   commands;
   statements;
   ...
   end=$(date +%s)
   difference=$(( end - start ))
   echo Time taken to execute commands is $difference seconds.
   ```
   
   Another way is to use `time<scriptpath>`
   
7. Functions

   Declaration:
   
   ```
   [function] func()
   {
       statements;
   }
   ```
   
   Inside the function body, `$1` refers to the first argument, `$2` refers to the second, `$n` refers to the nth argument, `$@` refers to all arguments.
   
   One may to `$ export -f func` to export the function from parent process.
   
   When the command is executed successfully, `$?` returns 0, otherwise non-zero. The following script could be used as the check of successful executions.
   
   ```#!/bin/bash
   CMD="some commands"
   $CMD
   if [ $? -eq 0 ];
   then
       echo "$CMD executed successfully"
   else
       echo "$CMD terminated unsuccessfully"
   fi
   ```
   
8. `read` command

   1. Read exact n chars, no `Enter` needed. <br>
      `$ read -n 2 var`
   2. Read while no showing, like password. <br>
      `$ read -s var`
   3. Read while showing some message. <br>
      `$ read -p "Enter input:" var`
   4. Read while user-defined terminating char occurs. <br>
      `$ read -d ":" var`
      
9. IFS(Internal Field Separator)

   Default IFS is space. Check by `$ echo $IFS`.
   
   Example, change `IFS` to `,` to separate CSV data:
   
   ```
   data="name, sex, rollno, location"
   oldIFS=$IFS
   IFS=,
   for item in $data;
   do
       echo $item
   done
   IFS=$oldIFS
   ```
   
10. Loops

   1. For loop

      ```
      for var in list;
      do 
         commands;
      done
      
      for i in {a..z}
      for i in {0..9}
      ```
      
   2. While loop

      ```
      while condition
      do
         commands;
      done
      ```
  3. Until loop

     ```
     x=0;
     until [ $x -eq 9 ];
     do
        let x++; echo $x;
     done
     ```
 
11. If clause

    ```
    if condition
    then
       commands;
    else if condition; then
       commands;
    else 
       commands;
    fi
    ```
    
    Comparison<br>
    `[ $var -eq 0 ]` or `[ $var -ne 0]`<br>
    `-gt`: greater than<br>
    `-lt`: less than<br>
    `-ge`: greater than or equal to<br>
    `-le`: less than or equal to<br>
    `-a`: and<br>
    `-o`: or <br>
    `[ $var -ne 0 -a $var2 -gt 2]` or use `&&` and `||`:<br>
    `[ $var -ne 0 ] && [ $var2 -gt 2 ]`
    
    For more conditions that are related to file, refer to (p.39):<br>
    `[-f $var]`: var is a normal file path or file name;<br>
    `[-d $var]`: var is a directory;<br>
    `[-e $var]`: var exists;<br>
    `[-x $var]`: var is executable;<br>
    `[-w $var]`: var can be written;<br>
    `[-r $var]`: var can be read;<br>
    
    Compare two strings, better use double `[[ ]]`:<br>
    `[[ $str1 == $str2 ]]`<br>
    `[[ $str1 != $str2 ]]`<br>
    `[[ -n $str ]]`: return true if str is non-empty;<br>
    `[[ -z $str ]]`: return true if str is empty.
    
12. Video and reply script (This is fun!!!) (p.45)

    ```
    $ script -t 2> timing.log -a output.session
    type commands;
    ...
    exit
    ```
    
    After videoing, to replay, use<br>
    `$ scriptreplay timing.log output.session`<br>
    Both files are needed, `timing.log` saves the time of commands executed, `output.session` saves the output.
    
13. Find (p.46)
   1. List all the files and files in the sub paths starting from base_path<br>
      `$ find base_path [-print]`
   2. Find files through names<br>
      `$ find path -name "*.txt" -print`<br>
      `-iname` ignores the capital letters.<br>
      `$ find . \( -name "*.txt" -o -name "*.pdf" \) -print`
   3. Find by path<br>
      `$ find /home/users -path "*/slynux/*" -print`
   4. Negate condition<br>
      `$ find . ! -name "*.txt" -print`
   5. Control the search depth<br>
      `$ find . -maxdepth 3 -mindepth 1 -name "f*" -print`<br>
      Use `-maxdepth` and `-mindepth` to control the search region. When restrict the search only in the current directory, use `-maxdepth 1`.
   6. Find by file category<br>
      `$ find . -type d -print`<br>
      `f`:file;`l`:symbolic link;`d`:directory;
   7. Find by time<br>
      `-atime`;`-mtime`;`-ctime`
   8. Find by size<br>
      `$ find . -type f -size +2k`
      `$ find . -type f -size -2M`
      `$ find . -type f -size 2G`
   9. Together with other commands by using `-exec`(best part!!!) (p.52)<br>
      `$ find . -type f -name "*.c" -exec cat {} \;>all_c_files.txt`<br>
      `{}` will be replaced by all files that are found one by one and `\;` is crucial that it is the argument of `-exec`.<br>
      `-exec` only supports one-line command. If multiple commands are required, use script:<br>
      `find . -exec ./commands.sh {} \;`