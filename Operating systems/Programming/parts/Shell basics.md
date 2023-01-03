### **1. Shell basics:**
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