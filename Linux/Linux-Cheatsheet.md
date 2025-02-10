# Linux Cheatsheet

## Package Management

### Debian (apt)

```bash
su -
sudo su
sudo visudo
exit

sudo apt purge package-name  # 卸载软件包并删除配置文件
sudo apt --purge autoremove
apt list --installed
dpkg -i/r package.deb  # 安装或者卸载软件包
sudo dpkg -P package_name  # 卸载软件包并删除配置文件
dpkg -l package_name  # 查询软件包信息
dpkg -l  # 查询已安装的软件包列表
```

### Arch (pacman)

```bash
pacman -Ss package_name  # 搜索软件包
sudo pacman -S package_name  # 安装软件包
sudo pacman -R package_name  # 卸载软件包
sudo pacman -Rs package_name  # 卸载软件包并删除相关依赖
sudo pacman -Sy  # 更新软件包列表
sudo pacman -Syu  # 升级系统中的所有软件包
sudo pacman -Sc  # 清理已下载的软件包
sudo pacman -Qdt  # 列出系统中未使用的依赖
sudo pacman -Rns $(pacman -Qdtq)  # 清理系统中未使用的依赖
pacman -Q  # 查看已安装的软件包
pacman -Qi package_name  # 查看特定软件包的详细信息
pacman -Qo /path/to/file  # 查看文件属于哪个软件包
```

### AUR (yay)

```bash
yay -Ss package_name  # 搜索 AUR 中的软件包
yay -S package_name  # 安装 AUR 中的软件包
yay -R package_name  # 卸载 AUR 中的软件包
yay -Rs package_name  # 卸载 AUR 中的软件包并删除相关依赖
yay -Syu  # 更新 AUR 中的软件包
yay -Sc  # 清理已下载的 AUR 软件包
yay -Yc  # 清理系统中未使用的 AUR 依赖
yay -Q  # 查看已安装的 AUR 软件包
yay -Si package_name  # 查看特定 AUR 软件包的详细信息
```

## System Monitoring

```bash
dmesg
h/top
kill; killall; pkill
free -h
df -h  /  du
ip
ifconfig  95.91.235.49
netstat -tuln
ping
lsusb
```

## Shell Commands

```bash
bash -c 'command'
history; !10
fzf: ctrl-R(S) / ctrl-T / alt-C
cd
ls -a/la
pwd
env
cp
rm -r
mkdir (-p)
mv
cp
ln
awk
grep -r "#include" .  /  -i（不区分大小写）  /  -v(反向匹配)
rg hello
find
file
cat
sed
cut -d: -f5 /etc/passwd
man / whatis
type
test [
echo / echo "text" >> <file_name>  / echo #RANDOM | exec_file  /  echo -en "..." 在同一行输出而不添加换行符号
command | tee file  # 输出到屏幕，同时又存入到文件
tr
diff
which
where
whereis (fzf)
wc
chsh -s /bin/bash(zsh)
wget url -P path
curl -o path/filename url
tree
fzf
stat file_name
chmod 777 (-R) example.txt
chmod ugo+rwx example.txt
usermod
gpasswd
export MY_VAR="Hello"
telnet
&& ||
dpkg -l | grep python3.8  # 管道符号
tar -xvf file.tar
tar -xzvf file.tar.gz
extract
ln [源文件] [目标文件]  # 创建硬链接
ln -s [源文件] [目标文件]  # 创建软链接
ufw
feh [image]
gedit 
viewnior . &
```

## Vim

```bash
~/.vimrc
ctags -R /opt/XMClib/XMC_Peripheral_Library_v2.1.16/
TlistOpen; TlistClose; TlistToggle(f8); ctrl-w/r 
:set path?   
:find filename; ctrl-^
:term
gc/gcc
ctrl-E
F9
\fif \fo
```

## SSH

```bash
eval $(ssh-agent)
ssh-add /path/to/your/private/key
ssh-add -d /path/to/your/private/key
ssh-add -l
ssh username@hostname_or_ip
ssh -T git@github.com
ssh-keygen
ssh-keygen -t rsa -b 4096 -C "9xiongxiao9@gmail.com" ~/.ssh/my_rsa_key
```

## Python Virtual Environment

```bash
cd ~/.venv
python3.8 -m venv myenv38
source myenv38/bin/activate
deactivate
```

## Docker

```bash
sudo service docker start/stop
sudo usermod -aG docker your_username
docker pull ubuntu  # 拉取镜像
docker push <镜像名:tag版本>  # 上传镜像
docker images  # 列出本地镜像
docker search <名字关键字>  # 搜索镜像仓库
docker rmi <镜像ID或名称>  # 删除镜像
docker build -t <镜像名称>:<标签> <Dockerfile目录>  # 构建自定义镜像
docker save -o <输出文件路径><镜像名：tag版本>  # 打包本地镜像文件
docker load -i <加载文件路径>  # 导入本地镜像文件
docker run -it --name my_container ubuntu /bin/bash  # 创建并启动新的容器
docker run -it --rm ubuntu /bin/bash  # 运行容器并在停止后立刻删除它
docker ps  # 查看正在运行的容器; docker ps -a  查看所有容器（包括已停止的）
docker start/stop <容器ID或名称>  # 启动或停止已经创建的容器
docker exec -it my_container /bin/bash  # 进入已经运行的容器
docker rm <容器ID或名称>  # 删除容器
docker rename <旧容器名称> <新容器名称>  # 容器重命名
docker commit -a "作者信息" -m “log信息” <容器id> <目标镜像名称：tag版本>  # 容器打包成镜像
docker cp <文件目录> <容器id>：<目标目录>  # 拷贝文件到容器
docker cp <容器id>：<文件目录> <宿主机目标目录>  # 拷贝容器文件到宿主机
docker update <容器id><相关设置>  # 更新容器设置
docker-compose up  # 根据docker-compose.yml来启动所有的服务
docker-compose down  # 停止并删除所有的服务
```

## Git

```bash
git config --global init.defaultBranch main
git init  # 在当前目录创建git的log文件
git config --global user.email "xiao.xiong@tum.de" 
git config --global user.name "xiaoxiong"
git config --global credential.helper store  # 配置 Git 凭据帮助器
git clone https://github.com/user2/repository.git  # 克隆远程仓库到本地仓库
git remote -v  # 查看远程仓库状态
git remote add origin https://github.com/user1/repository.git  # 连接远程仓库
git remote add upsteram https://github.com/user2/repository.git
git remote remove origin  # 删除与远程仓库的连接
git remote set-url origin git@github.com:axiaohe/AP.git 
git remote rename <old name> <new name>
git add . / git add <file name>
git restore --staged <file>  # 这个命令会将指定的 <file> 从暂存区中移除，只影响暂存区。
git restore <file>  # 将暂存区的版本恢复到工作目录，覆盖当前工作目录中的文件。
git restore --source=<commit id> <file>  # 恢复指定提交的版本到工作目录
git rm <file>  # 从git中删除这些文件
git rm --cached file1 file2 file3  # 从Git中删除这些文件的跟踪
git mv <current path> <new path>
git reset <file>  # 将文件从暂存区中移除，但不会修改工作目录中的文件。
git reset HEAD <file> 
git reset --hard/soft/mixed <commit id>  # 回退到某次提交的版本,删除该版本之后的commit信息
git reset --sort <commit id>  # 撤销当前提交并回到暂存区
git reset --mixed <commit id>  # 撤销当前提交和暂存区，但不修改工作区
git reset --hard <commit id>  # 撤销当前提交，暂存区和工作区
git revert HEAD  # 撤销最新的提交
git revert <commit_hash>  # 为每个指定的提交创建一个新的提交，以反转这些提交的更改
git revert --continue  # 撤销提交导致了冲突，在解决完冲突后git add，然后完成撤销过程
git commit -m "commit name"  # 提交
git commit --amend -m 'commit name'  # 修改最近一次的提交信息
git rebase -i [start point]  # 修改之前某次的提交信息
git log  # 查看commit信息
git status  # 查看branch信息
git diff <id1> <id2>  # 显示两版本的区别
git diff --cached  # 显示所有已经被添加到暂存区，但还没有被提交的文件的更改内容
git blame <file>  # 查看每一行代码的修改历史
git blame -L 10,20 <file>  # 只查看第10行到第20行的修改历史
git fetch origin main  # 从远程仓库获取最新的提交和分支信息，但不会将这些更改合并到你的当前分支
git merge （--no-ff / -squash / --ff-only） <branch name>  # 合并分支
git merge --abort  # 放弃合并，回到未合并之前的状态
git rebase <branch name>  # 变基某分支到当前分支
git rebase <master> <feature>  # 将feature变基到master分支的头
git cherry-pick <commit-hash>  # 从一个分支选择特定的提交，然后将其应用到当前分支
git cherry-pick <feature>  # 将feature分支中的最新commit提交到master分支
git push (--force) origin main  # 提交到远程仓库（--forch强制推送更改到远程仓库，会覆盖远程仓库的历史）
git push --set-upstream origin main  # 提交到远程仓库，并将其远程分支设为上游分支
git push origin local_branch:remote_branch  # <远程主机名>  <本地分知名>：<远程分支名>
git pull origin main  # 拉取远程仓库的最新更改到本地（--rebase）
git pull origin local_branch:remote_branch  # <远程主机名>  <本地分知名>：<远程分支名>
git branch <branch name>  # 创建一个分支
git branch --set-upstream master origin/next  # 手动建立追踪关系
git branch -M main  # 将当前分支改名为main
git branch -d <branch name>  # 删除分支
git branch  # 查看分支
git checkout <branch name>  # 切换分支
git checkout origin/main -- <filename>  # 从某个仓库某个分支检出文件到当前工作目录
git checkout <commit id>  # 恢复到指定的commit
git checkout <commit id> -- <filename>  # 恢复某个特定文件到指定的commit状态
git checkout (-b) branch-name  # （创建并）切换到分支
git switch branch-name  # 切换到分支
git log
git log --graph --oneline --all  # 可视化查看分支
git clone <repository URL> [branch]  # 克隆远程仓库到本地计算机
git stash
git stash save “save message”
git stash list
git stash show
git stash apply
git stash apply stash@{num}
git stash drop
git stash drop stash@{num}
git stash pop
git stash clear
git stash push --keep-index  # 保存工作目录中的更改而不包括暂存区中的更改
git stash push -u  # 保存未被追踪的文件
git diff  # 显示对未暂存文件的更改
git diff [<options>] [<commit>] [--] [<path>…​]
git diff > changes.patch
git apply changes.patch
touch .gitignore  # （*.log  /temp  debug/  debug?.log  debug[0-9].log）
	# 开头的行是注释。
	* 匹配零个或多个字符。
	? 匹配单个字符。
	[abc] 匹配方括号内的任何一个字符。
	** 匹配多级目录。
	! 不忽略
lazygit
```

## CMake

```cmake
# comments  or  #[[ block comments ]]
cmake_minimum_required(VERSION 3.0.0)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Werror")
project(PROJ_NAME)
set(SRC_LIST add.c  div.c   main.c  mult.c  sub.c)  # or  set(SRC_LIST add.c;div.c;main.c;mult.c;sub.c)
list(APPEND SRC_LIST ${SRC_LIST} ${TEMP})
list(REMOVE_ITEM SRC_1 ${PROJECT_SOURCE_DIR}/main.cpp)
include(MyCustomMacros)
add_executable(app  ${SRC_LIST})
target_compile_features(myapp PRIVATE cxx_std_17)
target_compile_options(myapp [PRIVATE|PUBLIC|INTERFACE] -Wall -Wextra -Werror)
add_dependencies(myapp mylib)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src SRC_LIST)
file(GLOB/GLOB_RECURSE MAIN_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
include_directories(${PROJECT_SOURCE_DIR}/include)
find_package(Boost REQUIRED COMPONENTS system filesystem)
add_dependencies(myapp mylib)
add_library(slib STATIC ${SRC_LIST})
add_library(dlib SHARED ${SRC_LIST})
link_directories(${PROJECT_SOURCE_DIR}/lib)
link_libraries(<static lib> [<static lib>...])
target_link_libraries(<target> <PRIVATE|PUBLIC|INTERFACE> <item>... [<PRIVATE|PUBLIC|INTERFACE> <item>...]...)
message([STATUS|WARNING|AUTHOR_WARNING|FATAL_ERROR|SEND_ERROR] "message to display" ...)
add_definitions(-DDEBUG)
add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
install(TARGETS my_executable my_library
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)
install(DIRECTORY include/ DESTINATION include)
ExternalProject_Add(zlib
  URL https://zlib.net/zlib-1.2.11.tar.gz
  PREFIX ${CMAKE_BINARY_DIR}/zlib
  INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
)
ExternalProject_Get_Property(zlib INSTALL_DIR)
enable_testing()
add_executable(my_test_executable test.cpp)
add_test(NAME my_test COMMAND my_test_executable)
set_tests_properties(my_test PROPERTIES TIMEOUT 5)
add_custom_target(run_tests
    COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure
    DEPENDS my_test_executable)
add_custom_command(
    OUTPUT student_mpirun.opt
    COMMAND sed -n -E -e 's/.*!mpirun_option ((--bind-to|--map-by|--rank-by)\\s\\S+).*/\\1/p' ${CMAKE_SOURCE_DIR}/student_submission.cpp > student_mpirun.opt
    DEPENDS student_submission.cpp
    COMMENT "Generating student_mpirun.opt"
)
set_property()
```

## CMake Macros

```cmake
PROJECT_SOURCE_DIR, PROJECT_BINARY_DIR
CMAKE_CURRENT_SOURCE_DIR, CMAKE_CURRENT_BINARY_DIR
CMAKE_BINARY_DIR
LIBRARY_OUTPUT_PATH, EXECUTABLE_OUTPUT_PATH
RUNTIME_OUTPUT_DIRECTORY, LIBRARY_OUTPUT_DIRECTORY, ARCHIVE_OUTPUT_DIRECTORY
CMAKE_RUNTIME_OUTPUT_DIRECTORY, CMAKE_LIBRARY_OUTPUT_DIRECTORY, CMAKE_ARCHIVE_OUTPUT_DIRECTORY
CMAKE_CXX_STANDARD
CMAKE_CXX_FLAGS, CMAKE_CXX_FLAGS_DEBUG, CMAKE_CXX_FLAGS_RELEASE
PROJECT_NAME
CMAKE_BUILD_TYPE
```

## Compilation

```bash
mpicxx
nvcc
make
make clean
make distclean
make install
make uninstall
make program 
make debug
nvim CMakeLists.txt; mkdir build; cd build; cmake ..; make
bear -- make
bear -- g++ main.cpp
g++ helloworld.cpp -fsanitize=address -g -o helloworld  # 检查是否有内存泄露 Sanitizers，调试，改名
g++ -c -o helper.o helper.cpp
g++ -I path/to/eigen-version/ mycode.cpp -o mycode
g++ `pkg-config --cflags eigen3` matvec.cpp -o matvec
	-E  # stop at preprocessing
	-S  # stop at assembly step
		-O0 Do not optimize.
		-O1 Optimize.
		-O2 Optimize even more.
		-Os Optimize for file size.
	-c  # stop at the object files step, 只编译和汇编源代码，而不链接,生成object文件
-lld  # 查看链接库文件
-Wall  # 开启所有的警告信息
g++ -O2 -ftree-loop-vectorize -fopt-info-vec-all mycode.cpp  # 自动矢量化
readelf -h code.o
```

## Debugging

```bash
nm file_name  # 查看二进制文件的符号表信息
echo $?  # 查看最近执行的命令的退出状态码
clang-format:
	# For formatting a single file
	clang-format -style=file -i mycode.cpp
	# For formatting all files in a directory
	clang-format -style=file -i src/*
	# For formatting all .h and .cpp files in all child directories
	find . \( -iname "*.h" -o -iname "*.cpp" \) -exec clang-format -style=file -i {} \;
clang-tidy main.cpp -checks="cppcoreguidelines-*" -- -std=c++17  # 代码静态分析
clang-tidy main.cpp -checks=cppcoreguidelines-prefer-member-initializer -- -std=c++17
Google Benchmark
Profiling with gprof
	Compile your code with [g++ -pg mycode.cpp -o mycode]
	Execute your code normally and it will generate data (stored in gmon.out)
	Analyze the data with [gprof mycode gmon.out > report.txt]. Read report.txt in any text editor.
Profiling with perf
	perf record ./your_program
	perf report
Measuring time with the chrono library
GCC: -finline-functions: Consider all functions for inlining.
GCC: -ffast-math, -fassociative-math: aggressive optimization flags
GCC: -O0 (no opt.), -O1 (optimize), -O2 (optimize more), -O3 (optimize most),
	 -Ofast (optimize most and disregard IEEE/ISO specifications for math functions)
GCC: -funroll-loops: Unroll loops
g++ -g your_program.cpp -o your_program && pahole your_program
```

## GDB

```bash
gdb helloworld:
	b break <line number>; break line-or-function if (condition)
	watch [watch var; watch(x>y)]
	r run
	s step
	n next
	c continue
	p print <variable>
	l list
	q quit
	i info breakpoints(i b)/locals...
	until
ulimit -c 0  # 禁止生成核心转储文件
ulimit -c unlimited  # 允许生成核心转储文件，不限制大小
gdb ./my_program core
```

## VSCode Tasks

### tasks.json

```json
{
	"version": "2.0.0",
	"tasks": [
		{
			"label": "cmake",
			"type": "shell",
			"command": "cmake",
			"args": [
				"--build",
				"${workspaceFolder}/build"
			],
			"group": {
				"kind": "build",
				"isDefault": false
			},
			"problemMatcher": []
		},
		{
			"label": "make",
			"type": "shell",
			"command": "make",
			"args": [
				"-C",
				"${workspaceFolder}/build"
			],
			"dependsOn": ["cmake"],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"problemMatcher": ["$gcc"]
		},
		{
			"label": "clean",
			"type": "shell",
			"command": "make",
			"args": [
				"-C",
				"${workspaceFolder}/build",
				"clean"
			],
			"group": {
				"kind": "build",
				"isDefault": false
			},
			"problemMatcher": []
		}
	]
}
```

### launch.json

```json
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": "build",
			"type": "cppdbg",
			"request": "launch",
			"program": "${workspaceFolder}/build/executable_program",
			"coreDumpPath": "${workspaceFolder}/path/to/core/file",
			"args": [],
			"stopAtEntry": false,
			"cwd": "${workspaceFolder}",
			"environment": [],
			"externalConsole": false,
			"MIMode": "gdb",
			"miDebuggerPath": "/usr/bin/gdb",
			"setupCommands": [
				{
					"description": "Enable pretty-printing for gdb",
					"text": "-enable-pretty-printing",
					"ignoreFailures": true
				}
			],
			"preLaunchTask": "make",
			"postDebugTask": ""
		}
	]
}
```

## Java

```bash
java -version
javac <file-name.java>  # 编译
java <file-name>  # 运行
javac -d . MyPackageClass.java  # 编译package，它被存储在root/mypack/MyPackageClass.java
java mypack.MyPackageClass  # 运行这个package
```

## .NET

```bash
dotnet --version
dotnet new <template> -n <project_name>  # such as: dotnet new console -n MyApp
dotnet restore
dotnet build
dotnet run
dotnet publish -c Release -o <output_folder>
dotnet test
dotnet add package <package_name>
dotnet --list-sdks
dotnet --list-runtimes
nuget install package-name
```

## VSCode

```bash
code .
```

## USB/IP

```bash
usbipd list
usbipd bind --busid 2-5
usbipd attach --wsl --busid=2-5
usbipd detach --busid 2-5
```

## ROS

### ROS1

```bash
source /opt/ros/noetic/setup.zsh  # (.bash)
```

### ROS2

```bash
source /opt/ros/humble/setup.zsh  # (.bash)
```

## WSL2

```bash
unzip, nodejs npm
zsh, oh-my-zsh(git, zsh-autosuggestions, zsh-syntax-highlighting), star, fzf
git, openssh, curl, wget, man-db, docker, docker-compose 
nvim, lazyvim
g++， gdb, bear, cmake, perf, cuda, cuda-tools
jdk-open, maven
dotnet-sdk dotnet-runtime, nuget
openmpi, ffmpeg
xclip usbutils dos2unix net-tools ufw

export LIBGL_ALWAYS_INDIRECT=1
sudo rmdir /tmp/.X11-unix && ln -s /mnt/wslg/.X11-unix /tmp/.X11-unix
# or
sudo nvim /etc/tmpfiles.d/wslg.conf
L+     /tmp/.X11-unix -    -    -    -   /mnt/wslg/.X11-unix
# (rm /etc/tmpfiles.d/wslg.conf)
```

## Terminator

```bash
Ctrl+Alt+T  # 创建新终端。
Ctrl+Shift+E  # 垂直分割当前终端。
Ctrl+Shift+O  # 水平分割当前终端。
Ctrl+Shift+W  # 关闭当前终端。
Ctrl+Shift+Q  # 关闭终端窗口。
F11  # 切换全屏模式。
Ctrl+Shift+T  # 打开新标签页。
Ctrl+Page Up  # 切换到上一个标签页。
Ctrl+Page Down  # 切换到下一个标签页。
Ctrl+Tab  # 在分割的终端之间切换焦点。
Ctrl+Shift+↑ 和 Ctrl+Shift+↓  # 在分割的终端之间上下移动。
Ctrl+Shift+← 和 Ctrl+Shift+→  # 在分割的终端之间左右移动。
```

## Regex Syntax

```regex
a|b  # Matches a or b
gr(a|e)y  # Matches gray or grey
.  # Matches any single character
[abc]  # Matches a single character a, b or c
[^abc]  # Matches any single character except a, b or c
[a-z]  # Matches a single character in the range a to z
[a-zA-Z]  # Matches a single character in the range a to z or A to Z
^  # Matches the start of the filename
$  # Matches the end of the filename
( )  # Defines a marked subexpression
\n  # Matches what the nth marked subexpression matched, where n is a digit from 1 to 9
\b  # Match word boundaries
*  # Matches the preceding element zero or more times
?  # Matches the preceding element zero or one times
+  # Matches the preceding element one or more times
.  # Matches any single character except a newline.
*?  # Lazily matches the preceding element zero or more times
+?  # Lazily matches the preceding element one or more times
{x}  # Matches the preceding element x times
{x,}  # Matches the preceding element x or more times
{x,y}  # Matches the preceding element between x and y times
\  # Escape special character
\d  # Matches any digit, equivalent to [0-9]
\D  # Matches any non-digit character, equivalent to [^0-9]
\w  # Matches any word character (letters, digits, or underscore), equivalent to [a-zA-Z0-9_]
\W  # Matches any non-word character, equivalent to [^a-zA-Z0-9_]
\s  # Matches any whitespace character (including space, tab, newline, etc.), equivalent to [ \t\n\r\f\v]
\S  # Matches any non-whitespace character, equivalent to [^ \t\n\r\f\v].
\A  # Matches the beginning of a string (unlike ^, which only matches the very start position)
\Z  # Matches the end of the string or right before the final newline character (unlike $)
\z  # Matches the very end of the string
(?i)  # Enables case-insensitive matching
(?m)  # Enables multiline mode, where ^ and $ match the start and end of each line
(?s)  # Enables single-line mode, allowing . to match any character including newline
(?x)  # Enables verbose mode, allowing spaces and comments in the regular expression to improve readability
\k<name>  # Matches a named capture group.
(?=...)  # Positive lookahead, asserts that what follows matches ...
(?!...)  # Negative lookahead, asserts that what follows does not match ...
(?<=...)  # Positive lookbehind, asserts that what precedes matches ...
(?<!...)  # Negative backward-looking, asserting that the preceding content is not...
(?P<name>...)  # Define a named capture group
```

## Script

### script.sh

```bash
#!/bin/sh  # A new shell spawn
\  # 也可以作为命令末尾的换行符号
|  # 管道符号，会启用一个新的subshell，注意变量scope
xargs  # 从一个命令的标准输出读取数据流，并将它们作为参数传递给另一个命令（经常与find一起使用）
command &  # 在后台运行前面的命令
grep "mystring" /tmp/myfile
echo Hello World
echo "Hello World"
echo "Hello   \"World\""  # 转义符
x="hello"
expr $x + ...
read MY_NAME && echo "Hello $MY_NAME - hope you're well."
touch -en "${MYVAR}_file"
export MYVAR
. ./myvar.sh  # 等同于source
mv *.txt *.bak  # **/__pycache__/ 通配符
current_date=$(date) or `date`  && echo "Today's date is: $current_date"  # 执行命令，并将其输出结果作为字符串返回
ls -ld {/,/usr,/usr/local}/{bin,sbin,lib}
if [ "$foo" = "bar" ]  # 注意空格！字符串比较可以用=, !=
-eq -ne -lt -gt -le -ge    -e -S -n -z -nt -ot -ef -O -d -f -r -w -x
\  # 换行符号
[ "$X" -nt "/etc/passwd" ] && echo "X is a file which is newer than /etc/passwd"  # &&：前面条件满足才执行后面的
[ $X -ne 0 ] && echo "X isn't zero" || echo "X is zero"  # true执行&&的，false执行||的
> <
... > /dev/null 2>&1  # 将所有输出都丢弃掉
$0  # 程序名
$1 .. $9  # 脚本的第一到第九个参数  /  $@ and $*是所有的参数$1 .. 
$#  # 脚本调用的参数的数量
shift （num）  # 丢弃第一个(前num个)位置参数，并将剩下的参数依次向左移动。
$?  # 上一条命令的返回值，0说明顺利运行
$$  # PID (Process IDentifier) of the currently running shell
$!  # PID of the last run background process
IFS  # Internal Field Separator
echo "Your name is : ${myname:-`whoami`}"  # 不会改变变量本身。 默认参数，这里``的命令在subshell中运行，不会影响当前shell
echo "Your name is : ${myname:=John Doe}"  # 会改变变量本身，如果变量本身为被定义的话
tr, find, grep, expr, cut...  # 可以在sh脚本中使用这些外部程序
大量的函数可以定义在sh文件中构建一个库，在使用的时候source一下这个库就行
一个函数有四种方式来返回值：
	1. Change the state of a variable or variables
	2. Use the exit command to end the shell script
	3. Use the return command to end the function, and return the supplied value to the calling section of the shell script
	4. echo output to stdout, which will be caught by the caller just as c=`expr $a + $b` is caught
```

### For Loops

```bash
#!/bin/sh
for i in hello 1 * 2 goodbye 
do
  echo "Looping ... i is set to $i"
done
```

### While Loops

#### Example 1

```bash
#!/bin/sh
INPUT_STRING=hello
while [ "$INPUT_STRING" != "bye" ]
do
  echo "Please type something in (bye to quit)"
  read INPUT_STRING
  echo "You typed: $INPUT_STRING"
done
```

#### Example 2

```bash
#!/bin/sh
while :
do
  echo "Please type something in (^C to quit)"
  read INPUT_STRING
  echo "You typed: $INPUT_STRING"
done
```

#### Example 3

```bash
#!/bin/sh
while read input_text
do
  case $input_text in
		hello)          echo English    ;;
		howdy)          echo American   ;;
		gday)           echo Australian ;;
		bonjour)        echo French     ;;
		"guten tag")    echo German     ;;
		*)              echo Unknown Language: $input_text;;
   esac
done < myfile.txt	
```

### Test

```bash
if [ ... ]
then
  # if-code
else
  # else-code
fi

if  [ something ]; then  # 注意分号
  echo "Something"
  elif [ something_else ]; then
    echo "Something else"
  else
    echo "None of the above"
fi
```

### Case

```bash
while :
do
  read INPUT_STRING
  case $INPUT_STRING in
	hello)
		echo "Hello yourself!"
		;;
	bye)
		echo "See you again!"
		break
		;;
	*)
		echo "Sorry, I don't understand"
		;;
  esac
done
```

### Function

#### A simple Example

```bash
#!/bin/sh
# A simple script with a function...
add_a_user()
{
  USER=$1
  PASSWORD=$2
  shift; shift;
  # Having shifted twice, the rest is now comments ...
  COMMENTS=$@
  echo "Adding user $USER ..."
  echo useradd -c "$COMMENTS" $USER
  echo passwd $USER $PASSWORD
  echo "Added user $USER ($COMMENTS) with pass $PASSWORD"
}
###
# Main body of script starts here
###
echo "Start of script..."
add_a_user bob letmein Bob Holness the presenter
add_a_user fred badpassword Fred Durst the singer
add_a_user bilko worsepassword Sgt. Bilko the role model
echo "End of script..."
```

#### Scope of Variables

```bash
#!/bin/sh
myfunc()
{
  echo "I was called as : $@"
  x=2
}
### Main script starts here 
echo "Script was called with $@"
x=1
echo "x is $x"
myfunc 1 2 3
echo "x is $x"
```

#### Recursion

```bash
#!/bin/sh
factorial()
{
  if [ "$1" -gt "1" ]; then
	i=`expr $1 - 1`
	j=`factorial $i`
	k=`expr $1 \* $j`
	echo $k
  else
	echo 1
  fi
}
while :
do
  echo "Enter a number:"
  read x
  factorial $x
done
```

#### Libraries

##### common.lib

```bash
# common.lib
# Note no #!/bin/sh as this should not spawn 
# an extra shell. It's not the end of the world 
# to have one, but clearer not to.
#
STD_MSG="About to rename some files..."
rename()
{
  # expects to be called as: rename .txt .bak 
  FROM=$1
  TO=$2
  for i in *$FROM
  do
	j=`basename $i $FROM`
	mv $i ${j}$TO
  done
}
```

##### function.sh

```bash
#!/bin/sh
# function.sh
. ./common.lib
echo $STD_MSG
rename .txt .bak
```

#### Return Codes

```bash
#!/bin/sh
adduser()
{
  USER=$1
  PASSWORD=$2
  shift ; shift
  COMMENTS=$@
  useradd -c "${COMMENTS}" $USER
  if [ "$?" -ne "0" ]; then
	echo "Useradd failed"
	return 1
  fi
  passwd $USER $PASSWORD
  if [ "$?" -ne "0" ]; then
	echo "Setting password failed"
	return 2
  fi
  echo "Added user $USER ($COMMENTS) with pass $PASSWORD"
}
## Main script starts here
adduser bob letmein Bob Holness from Blockbusters
ADDUSER_RETURN_CODE=$?
if [ "$ADDUSER_RETURN_CODE" -eq "1" ]; then
  echo "Something went wrong with useradd"
elif [ "$ADDUSER_RETURN_CODE" -eq "2" ]; then 
   echo "Something went wrong with passwd"
else
  echo "Bob Holness added to the system."
fi
```

#### Exit Codes

```bash
...
```
