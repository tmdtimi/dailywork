## find

find grep more cat tail head

find /PATH -name(-type)(-size +10M)

查找路径下大于10M的txt文件

find . -name *.txt -size +10M -type f

find . -name config -type f(d)

find . -ctime +1 一天内

find . -cmin -30 权限修改

find . -mmin -30 内容修改

find . -amin -1 读过的

find . -maxdepth 3 -name "*.txt"

## grep

grep "a" 1.txt

grep -n "a" 1.txt

grep -n "a" *  ： 查找当前目录下所有文件中含"a"的行号

grep -rn "a" *.txt

grep -rn "a"  -A 2  -B 2 *


more +8 1.txt

tail -f 1.txt

##df

df -h 查看磁盘使用情况

du -h 当前路径下磁盘使用情况

du -h -d 1


## ps

ps 显示系统的运行进程

top : cpu占用情况

top -H -p pid :查看指定进程中每个线程的资源占用情况

jstack pid > ./dump.log 将指定进程中线程的堆栈信息输出到文件



netstat -napt 查看TCP连接状态

 


