---
title: grep sed awk etc
date: 2021-01-01 20:40:13
tags: [shell]
cover: /img/linux.png

---
## <a name='export'></a>`export`
```sh
# export设置环境变量是暂时的，只在本次登录中有效，可修改如下文件来使命令长久有效
# 1. 修改profile文件：
# vi /etc/profile
# 在里面加入:
export PATH="$PATH:/opt/au1200_rm/build_tools/bin"

# 胜于修改环境变量，但只在本次登录中有效
export PATH=$PATH:/home/danyu

# 查看所有export了的variable（环境变量）
export -p

# 让A export
declare -x A
declare -x HOME="/root"

# upexported 
export -n A 

```
## <a name='ChangePATH'></a>Change `$PATH`
###1. <a name='underwindows'></a>under windows
```sh
# show the path variable
path 
# postpend
setx path "%path%;C:\gcc\bin"
# prepend 
setx path "C:\gcc\bin;%path%"
```
###2. <a name='method1addpath'></a>method 1：add path 
```sh 
sudo su 
echo $PATH
# /usr/local/sbin:/usr/loval/bin:/usr/sbin:/usr/bin:/root/bin
# 一般有一堆 /usr/bin

# 告诉你用的到底是哪个sudo
which sudo 

##  8. <a name='prependPATHsearchpath'></a>prepend PATH, 先search&用新加的那个path里面的软件
PATH=/root/$PATH
##  9. <a name='postpendPATH'></a>postpend PATH
PATH=$PATH:/some/dir
```
###3. <a name='method2:addpath'></a>method 2: add path

```sh
# 修改 ~/.bash_profile
PATH=$PATH:$HOME/bin
```
## <a name='echoprintf'></a>`echo & printf`
1. `echo`是回显，即代表回车显示，是 __自带换行__ 的; 而`printf`只是打印出来，__没有换行__
2. `echo`只是回显 __没有变量替换功能__；`printf`是有的
e.g. 假如我们定义好变量`a='hello world'`
则 `echo "%s" $a`  显示的结果就是`%s`
而 `printf "%s\n" $a`  显示的结果就是`hello world`

```sh 
printf "1\n2\n3\n" | xargs touch
# 开三个文件 分别为1 2 3

printf "1\n2\n3\n" | xargs -i touch {}.txt
# -i 把之前的输出变成输入
# 开三个txt文件 分别为 1.txt 2.txt 3.txt

printf "1\t2\t3\t" | xargs -d "\t" touch
# -d = delimiter
# 开三个文件 分别为 1 2 3

echo -e 
# -e = 如果没有-e 在引号里面的东东\n\r\t什么的就会直接读出来 加了-e 会format它
# 如果不想加-e 就用printf
printf '%s %s' arg1 arg2


echo {1..9} | xargs -n3
#  -n3 一次用三个arguments
#  输出： 1 2 3
#   	 4 5 6
#   	 7 8 9
```
## <a name='find'></a>`find`
```sh
find . -name "*\(1\)*" -print0 | xargs -0 rm
# move all the repetative files in the current directory



touch file{1..9}.jpg
# 生成file1.jpg 到file9.jpg
print "1\n2\n3\n" | xargs -i touch {}.txt
# 生成1.txt 2.txt 3.txt
find . -maxdepth 1 -name "*.jpg"
# only找现有文件夹的depth 所有jpg
find . -maxdepth 1 -name "*.jpg" | xargs tar -czvf jpeg.tgz
find . -type f -iname "test*"
# 找到所有文件名为test的文件(不区分大小写）

svn propget svn:externals -R
svn propset svn:externals . -F svn_ext_val.txt
# apply svn_ext_val.txt recursively
find . -type f -name "svn_ext_val.txt" -exec svn propset svn:externals . -F {} \;
find . -type f -name "svn_ext_val.txt" -exec echo "svn propset svn:externals . -F {}" \;


find . -maxdepth 1 -type f -name "*sim.py" -exec mv {} ./chips_sim/ \;
```
## <a name='cat'></a>`cat`
```sh
# concatenate two lst files into bc29_patch_m4.lst
echo "concatenate two edited lst file into bc29_patch_m4.lst"
cat bc29_m4.lst patch_m4.lst>./bc29_patch_m4.lst
```
## <a name='grep'></a>`grep`
语法：`grep pattern file(s)`
```sh
grep -rnw '/path/to/somewher/' -e 'pattern'
# -r or -R for recursive
# -n is line number
# -w stands for matching the whole word
# -l stands for "show the file name, not the result itself"
# --exclude, --include, --exclude-dir flags
# e.g. 
# 1. search through those files which have .c or .h extensions
grep --include=\*.{c,h} -rnw '/path/to/somewhere' -e "pattern"
# search all the files ending with .o extension
grep --exclude=*.o -rnw '/path/to/somewhere' -e "pattern"

history | grep "git commit" | grep "dotfile" # use pipes

grep "...-...-...." names.txt #get back the phone numbers in that file
grep -P "\d{3}-\d{3}-\d{4}" names.txt #works on linux but not mac  //IN Bsd version

```
## <a name='sed'></a>`sed`
```sh 
# substitute (find & replace) "foo" with "bar" on each line 
sed 's/foo/bar/' # replaces only 1st instance in a line 
sed 's/foo/bar/4' # replaces only 4th instance in a line 
sed 's/foo/bar/g' # replaces ALL instances in a line

# print first 10 lines of file (emulates behavior of "head") 
sed 10q

# print first line of file (emulates "head -1") 
sed q

# print last 10 lines of file (emulates "tail") 
sed -e :a -e '$q;N;11,$D;ba'


# 把a文件夹里的东东全复制到b文件夹并且keyword aaa重命名为bbb
echo "  2. Copy and rename files from old project: /"$old_proj_name  
ls * | sed -e "p;s/^$old_proj_name/$new_proj_name/" | xargs -n2 mv

# 把xxx文件里所有aaa keyword全部改成bbb
NEW_PROJ_NAME=${new_proj_name^^} #capitalized 
echo "  Replace keyword:"$old_proj_name "to" $new_proj_name "in" $new_proj_name"sim.py"   
sed -i "s/$old_proj_name/$new_proj_name/g" $new_proj_name"sim.py" 
sed -i "s/$OLD_PROJ_NAME/$NEW_PROJ_NAME/g" $new_proj_name"sim.py" 

# remove all the lines include it before "Mainloop:"
echo "remove lines before Mainloop in patch_m4.lst"
sed -i '1,/ORG 0x6000/d' patch_m4.lst 

# remove all the lines include it after "ORG 0x6000"
echo "remove lines after ORG 0x6000 in bc29_m4.lst"
sed -i '/ORG 0x6000/,$d' bc29_m4.lst  
```
## <a name='awk'></a>`awk`
语法：`awk 动作 文件名`

```sh
# 每行按空格或TAB分割，输出文本中的1、4项
awk '{print $1,$4}' log.txt
# 从文件中找出长度大于80的行
awk 'length>80' log.txt
# 输出包含"re" 的行
awk '/re/ ' log.txt
```
