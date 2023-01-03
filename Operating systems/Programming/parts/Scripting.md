### **3. Scripting**
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
#-----FOR---------
for var in <list>; do
	...
done
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
