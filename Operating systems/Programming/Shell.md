## Vim
:wq : write and quit


## Shell
### [[Shell basics]]
```bash
#----------FILE--------------
mv file1.sh file2.sh #change file name
cp file.sh #copy file
rm file.sh #remove file
rm -r dir #remove folder and all objects inside it
cat file.sh #show content of file
touch file.sh #create empty file
man cmd #manpages

#--------DIRECTORY------------
ls -la #list out all files -l long listing (with rights) -a list all (includes hidden files)
mkdir dir #make directory
cd subfolder #change working directory to subfolder
cd .. #change directory to parent directory
pwd #print working directory
chmod u+x file.sh #add execute rights for current user
chmod 744 file.sh #1=x 2=w 4=r

#-------FILE MANIPULATION------
cat file1.sh file2.sh #concatenate content of file1 in file2
diff file1.sh file2.sh #compare 2 files
gzip file.sh #compression
gunzip file.sh #decompression

#-------------OTHERS------------
test expression #test expression, return 1 or 0
echo $? #show exit code of last operation
```
### [[Shell variables]]
```bash
#-----------WILDCARDS------------
* #n characters
? #1 character

#-----------VARIABLES------------
$? #exit code
$$ #process id
$# #number of parameters passed
$* #arguments
$PWD #working directory
$PATH
$RANDOM #bash FUNCTION, return pseudorandom int
$HOME #home directory
$USER #username
```

### [[Scripting]]
```bash
#!/bin/bash
var1=1 #variable assignment (no space)
var2=2
date=`date` #function call with `function_name`
alt_date=$(date) #alternative with $(function name)
echo $var1 #print variable
result=`expr $var1 + $var2` #expr function: evaluate expressions, called with `expr`
alt_result=$((2 + $var1)) #alternative function call for expr $(())

#-------------BOOLEAN--------------
expr1 -a expr2 #and
expr1 -o expr2 #or
!expr #not
"(" expr1 -o expr2 ")" -a expr3
zahl1 -eq zahl2 #-ne -le -lt -ge -gt
string1 = string2 #not equal !=

#------LOOPS/CONDITIONALS-----------
#------WHILE-------
while [ expr ]; do #space after and before square brackets
	...
done #end with done
#------IF---------
if [ expr1 ]; then
	...
elif [ expr2 ]; then
	...
else
	...
fi #end with fi
#------CASE--------
a="string"
case $a in #no brackets around variable
	tri | ring) #end condition with bracket
		cmd1
		;; #end case with ;;
	?"tri"*)
		cmd2
		;;
	*)
		default_cmd
		;;
esac #end with esac

#-----------FUNCTION------------------
hi() {
	echo Hallo $1
}
hi Berlin #Berlin is $1, output Hello Berlin
```
### [[Pipe and redirect]]
reader.sh:
```bash
#!/bin/bash
read myvar
echo "Input was: $myvar"
```
pipe:
```bash
echo "hello" | ./reader.sh #use stdout of a program as stdin
#output: Input was: hello
```
redirection:
```bash
./reader.sh < input.txt #use content of file as stdin
#output: Input was <content of input.txt>

echo "hello" > file.txt #write hello in file
echo "hello" >> file.txt #add hello to file
```

>[! NOTE]- Pipe vs. Redirection 
>Pipe is used to pass output to another **program or utility**.
>Redirect is used to pass output to either a **file or stream**.
>
> Example: **`thing1 > thing2` vs `thing1 | thing2`**
> 
> `thing1 > thing2`
>	1.  Your shell will run the program named `thing1`
>	2.  Everything that `thing1` outputs will be placed in a file called `thing2`. (Note - if `thing2` exists, it will be overwritten)
>	
>If you want to pass the output from program `thing1` to a program called `thing2`, you could do the following:
>**`thing1 > temp_file && thing2 < temp_file` = `thing1 | thing2`**
>which would
>	1.  run program named `thing1`
>	2.  save the output into a file named `temp_file`
>	3.  run program named `thing2`, pretending that the person at the keyboard typed the contents of `temp_file` as the input.
>However, that's clunky, so they made pipes as a simpler way to do that. `thing1 | thing2` does the same thing as `thing1 > temp_file && thing2 < temp_file`

