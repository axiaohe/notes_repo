# Linux Cheat Sheet

## 1. 系统管理 (System Administration)

### 1.1. 用户和权限 (User and Permissions)

- `su -`: 切换到 root 用户（需要 root 密码）。
- `sudo su`: 使用 sudo 切换到 root 用户（可能需要当前用户密码）。
- `sudo visudo`: 安全地编辑 sudoers 文件，配置 sudo 权限。
- `exit`: 退出当前 shell 或用户。
- `chmod 777 (-R) example.txt`: 更改文件权限为所有用户可读、可写、可执行 (-R 递归修改目录)。
- `chmod ugo+rwx example.txt`: 为 User, Group, Other 添加读、写、执行权限。
- `usermod`: 修改用户信息。
- `gpasswd`: 管理用户组。
- `chsh -s /bin/bash(zsh)`: 更改默认 shell。
- `ufw`: 管理防火墙

### 1.2. 进程管理 (Process Management)

- `h/top`: 交互式进程查看器。
- `kill <PID>`: 发送信号给进程（默认 TERM 终止）。
- `killall <process_name>`: 按名称杀死进程。
- `pkill <pattern>`: 按模式杀死进程。

### 1.3. 系统信息 (System Information)

- `dmesg`: 显示内核消息。
- `free -h`: 显示内存使用情况（-h 人类可读）。
- `df -h`: 显示磁盘空间使用情况（-h 人类可读）。
- `du`: 显示目录大小。
- `ip`: 显示和操作路由、网络设备、策略路由和隧道。
- `ifconfig`: 配置网络接口（较旧的命令，建议使用 ip）。
- `netstat -tuln`: 显示网络连接、路由表、接口统计等。
- `ping <host>`: 测试网络连接。
- `lsusb`: 显示 USB 设备信息。
- `stat file_name`: 显示文件的状态
- `env`: 显示当前环境变量
- `history`: 显示执行的历史
- `!10`: 执行历史列表的第10条命令
- `export MY_VAR="Hello"`: 设置环境变量

### 1.4. 软件包管理 (Package Management)

#### 1.4.1. Debian/Ubuntu (APT)

- `sudo apt purge package-name`: 卸载软件包并删除配置文件。
- `sudo apt --purge autoremove`: 卸载孤立的依赖包并删除配置文件。
- `apt list --installed`: 列出已安装的软件包。
- `dpkg -i package.deb`: 安装 .deb 包。
- `dpkg -r package.deb`: 卸载 .deb 包。
- `sudo dpkg -P package_name`: 卸载软件包并删除配置文件。
- `dpkg -l package_name`: 查询软件包信息。
- `dpkg -l`: 查询已安装的软件包列表。
- `dpkg -l | grep python3.8`: 管道符号

#### 1.4.2. Arch Linux (Pacman)

- `pacman -Ss package_name`: 搜索软件包。
- `sudo pacman -S package_name`: 安装软件包。
- `sudo pacman -R package_name`: 卸载软件包。
- `sudo pacman -Rs package_name`: 卸载软件包并删除相关依赖。
- `sudo pacman -Sy`: 更新软件包列表。
- `sudo pacman -Syu`: 升级系统中的所有软件包。
- `sudo pacman -Sc`: 清理已下载的软件包。
- `sudo pacman -Qdt`: 列出系统中未使用的依赖。
- `sudo pacman -Rns $(pacman -Qdtq)`: 清理系统中未使用的依赖。
- `pacman -Q`: 查看已安装的软件包。
- `pacman -Qi package_name`: 查看特定软件包的详细信息。
- `pacman -Qo /path/to/file`: 查看文件属于哪个软件包。

#### 1.4.3. Arch Linux (Yay - AUR Helper)

- `yay -Ss package_name`: 搜索 AUR 中的软件包。
- `yay -S package_name`: 安装 AUR 中的软件包。
- `yay -R package_name`: 卸载 AUR 中的软件包。
- `yay -Rs package_name`: 卸载 AUR 中的软件包并删除相关依赖。
- `yay -Syu`: 更新 AUR 中的软件包。
- `yay -Sc`: 清理已下载的 AUR 软件包。
- `yay -Yc`: 清理系统中未使用的 AUR 依赖。
- `yay -Q`: 查看已安装的 AUR 软件包。
- `yay -Si package_name`: 查看特定 AUR 软件包的详细信息。

## 2. 文件和目录操作 (File and Directory Operations)

- `cd`: 改变目录。
- `ls -a/la`: 列出文件（-a 包括隐藏文件，-l 详细列表）。
- `pwd`: 显示当前工作目录。
- `cp`: 复制文件或目录。
- `rm -r`: 删除文件或目录 (-r 递归删除目录)。
- `mkdir (-p)`: 创建目录 (-p 创建多级目录)。
- `mv`: 移动或重命名文件/目录。
- `ln`: 创建链接。
- `ln [源文件] [目标文件]`: 创建硬链接。
- `ln -s [源文件] [目标文件]`: 创建软链接（符号链接）。
- `find`: 查找文件。
- `file`: 确定文件类型。
- `cat`: 显示文件内容。
- `grep -r "#include" .`: 在文件中搜索文本 (-r 递归搜索目录)。
  - `-i`: 不区分大小写。
  - `-v`: 反向匹配（显示不匹配的行）。
- `rg hello`: 类似于grep
- `sed`: 流编辑器，用于文本替换、删除等。
- `cut -d: -f5 /etc/passwd`: 提取文件中的字段 (-d 分隔符, -f 字段)。
- `man <command>`: 查看命令的手册页。
- `whatis <command>`: 显示命令的简短描述。
- `type <command>`: 显示命令的类型（内置、外部、别名等）。
- `test [: 条件表达式（与 [ 命令相同）。
- `echo "text"`: 显示文本。
- `echo "text" >> <file_name>`: 追加文本到文件。
- `echo #RANDOM`: 生成随机数。
- `echo #RANDOM | exec_file`: 运行程序并输入随机数
- `echo -en "...": 在同一行输出而不添加换行符号。
- `command | tee file`: 输出到屏幕，同时又存入到文件。
- `tr`: 转换或删除字符。
- `diff`: 比较文件差异。
- `which <command>`: 显示命令的完整路径。
- `where <command>`: 类似于which
- `whereis <command>`: 查找命令、源码和手册页的位置。
- `wc`: 统计文件的行数、单词数、字节数。
- `wget url -P path`: 下载文件
- `curl -o path/filename url`: 类似于wget
- `tree`: 显示目录结构
- `fzf`: 查找文件
- `extract`: 解压文件
- `viewnior . &`: 图片浏览器
- `gedit`: 文本编辑器
- `&&`: 逻辑与
- `||`: 逻辑或
- `tar -xvf file.tar`: 解包文件
- `tar -xzvf file.tar.gz`: 解压并解包文件

## 3. Shell 技巧 (Shell Tips)

- `bash -c 'command'`: 在 bash 中执行命令。
- `fzf`: 模糊查找器（ctrl-R 搜索历史, ctrl-T 粘贴文件路径, alt-C 进入目录）。
- `telnet`: 远程登录

## 4. Vim

- `~/.vimrc`: Vim 配置文件。
- `ctags -R /opt/XMClib/XMC_Peripheral_Library_v2.1.16/`: 生成 C/C++ 代码的 tags 文件。
- `TlistOpen; TlistClose; TlistToggle(f8)`: Taglist 插件命令（打开、关闭、切换）。
- `ctrl-w/r`: 窗口切换。
- `:set path?`: 查看 Vim 的搜索路径。
- `:find filename`: 在 path 中查找文件。
- `ctrl-^`: 在最近编辑的两个文件之间切换。
- `:term`: 打开终端
- `gc/gcc`: 注释/取消注释
- `ctrl-E`: 打开/关闭tag
- `F9`: 打开/关闭tree
- `\fif \fo`: 自动补全if/for循环

## 5. SSH

- `eval $(ssh-agent)`: 启动 SSH 代理。
- `ssh-add /path/to/your/private/key`: 添加私钥到 SSH 代理。
- `ssh-add -d /path/to/your/private/key`: 从 SSH 代理中删除私钥。
- `ssh-add -l`: 列出已添加的密钥。
- `ssh username@hostname_or_ip`: SSH 连接到远程主机。
- `ssh -T git@github.com`: 测试 GitHub SSH 连接。
- `ssh-keygen`: 生成 SSH 密钥对。
- `ssh-keygen -t rsa -b 4096 -C "9xiongxiao9@gmail.com" ~/.ssh/my_rsa_key`: 生成 RSA 密钥对并指定注释。

## 6. Python 虚拟环境 (Python Virtual Environments)

- `cd ~/.venv`: 进入虚拟环境目录。
- `python3.8 -m venv myenv38`: 创建 Python 3.8 虚拟环境。
- `source myenv38/bin/activate`: 激活虚拟环境。
- `deactivate`: 退出虚拟环境。

## 7. Docker

- `sudo service docker start/stop`: 启动/停止 Docker 服务。
- `sudo usermod -aG docker your_username`: 将用户添加到 docker 组（无需 sudo 使用 Docker）。
- `docker pull ubuntu`: 拉取镜像。
- `docker push <镜像名:tag版本>`: 上传镜像。
- `docker images`: 列出本地镜像。
- `docker search <名字关键字>`: 搜索镜像仓库。
- `docker rmi <镜像ID或名称>`: 删除镜像。
- `docker build -t <镜像名称>:<标签> <Dockerfile目录>`: 构建自定义镜像。
- `docker save -o <输出文件路径><镜像名：tag版本>`: 打包本地镜像文件。
- `docker load -i <加载文件路径>`: 导入本地镜像文件。
- `docker run -it --name my_container ubuntu /bin/bash`: 创建并启动新的容器。
- `docker run -it --rm ubuntu /bin/bash`: 运行容器并在停止后立刻删除它。
- `docker ps`: 查看正在运行的容器。
- `docker ps -a`: 查看所有容器（包括已停止的）。
- `docker start/stop <容器ID或名称>`: 启动或停止已经创建的容器。
- `docker exec -it my_container /bin/bash`: 进入已经运行的容器。
- `docker rm <容器ID或名称>`: 删除容器。
- `docker rename <旧容器名称> <新容器名称>`: 容器重命名。
- `docker commit -a "作者信息" -m “log信息” <容器id> <目标镜像名称：tag版本>`: 容器打包成镜像。
- `docker cp <文件目录> <容器id>：<目标目录>`: 拷贝文件到容器。
- `docker cp <容器id>：<文件目录> <宿主机目标目录>`: 拷贝容器文件到宿主机。
- `docker upadate <容器id><相关设置>`: 更新容器设置。
- `docker-compose up`: 根据 docker-compose.yml 来启动所有的服务。
- `docker-compose down`: 停止并删除所有的服务。

## 8. Git

- `git config --global init.defaultBranch main`: 设置默认分支为 main。
- `git init`: 在当前目录创建 Git 仓库。
- `git config --global user.email "xiao.xiong@tum.de"`: 设置全局用户邮箱。
- `git config --global user.name "xiaoxiong"`: 设置全局用户名。
- `git config --global credential.helper store`: 配置 Git 凭据帮助器
- `git clone https://github.com/user2/repository.git`: 克隆远程仓库。
- `git remote -v`: 查看远程仓库状态。
- `git remote add origin https://github.com/user1/repository.git`: 添加远程仓库。
- `git remote add upsteram https://github.com/user2/repository.git`
- `git remote remove origin`: 删除远程仓库。
- `git remote set-url origin git@github.com:axiaohe/AP.git`: 设置远程仓库地址
- `git remote rename <old name> <new name>`
- `git add . / git add <file name>`: 添加所有更改/添加指定文件到暂存区。
- `git restore --staged <file>`: 将文件从暂存区移除（不影响工作目录）。
- `git restore <file>`: 将暂存区的版本恢复到工作目录，覆盖当前工作目录中的文件。
- `git restore --source=<commit id> <file>`: 恢复指定提交的版本到工作目录。
- `git rm <file>`: 从 Git 中删除文件（并从工作目录删除）。
- `git rm --cached file1 file2 file3`: 从 Git 中删除文件跟踪（不删除工作目录文件）。
- `git mv <current path> <new path>`: 移动文件
- `git reset <file>`: 将文件从暂存区中移除，但不会修改工作目录中的文件。
- `git reset HEAD <file>`: 类似于git reset <file>
- `git reset --hard/soft/mixed <commit id>`: 回退到某次提交的版本,删除该版本之后的commit信息。
  - `--soft`: 只重置 HEAD 指针，不改变暂存区和工作目录。
  - `--mixed`: 重置 HEAD 和暂存区，不改变工作目录（默认）。
  - `--hard`: 重置 HEAD、暂存区和工作目录（危险操作，会丢失未提交的更改）。
- `git reset --sort <commit id>`: 撤销当前提交并回到暂存区。
- `git reset --mixed <commit id>`: 撤销当前提交和暂存区，但不修改工作区。
- `git reset --hard <commit id>`: 撤销当前提交，暂存区和工作区。
- `git revert HEAD`: 撤销最新的提交（创建一个新的提交来反转更改）。
- `git revert <commit_hash>`: 为每个指定的提交创建一个新的提交，以反转这些提交的更改。
- `git revert --continue`: 撤销提交导致了冲突，在解决完冲突后git add，然后完成撤销过程。
- `git commit -m "commit name"`: 提交更改。
- `git commit --amend -m 'commit name'`: 修改最近一次的提交信息。
- `git rebase -i [start point]`: 修改之前某次的提交信息。
- `git log`: 查看提交历史。
- `git status`: 查看工作目录状态。
- `git diff <id1> <id2>`: 显示两个提交之间的差异。
- `git diff --cached`: 显示暂存区和 HEAD 之间的差异。
- `git blame <file>`: 查看文件每一行的修改历史。
- `git blame -L 10,20 <file>`: 只查看第10行到第20行的修改历史。
- `git fetch origin main`: 从远程仓库获取最新的提交和分支信息。
- `git merge (--no-ff / -squash / --ff-only) <branch name>`: 合并分支。
  - `--no-ff`: 禁用 fast-forward 合并，创建一个合并提交。
  - `-squash`: 将分支上的所有提交压缩成一个提交再合并。
  - `--ff-only`: 只允许 fast-forward 合并。
- `git merge --abort`: 放弃合并
- `git rebase <branch name>`: 变基某分支到当前分支。
- `git rebase <master> <feature>`: 将feature变基到master分支的头。
- `git cherry-pick <commit-hash>`: 选择一个提交并应用到当前分支。
- `git cherry-pick <feature>`: 将feature分支中的最新commit提交到master分支。
- `git push (--force) origin main`: 推送更改到远程仓库（--force 强制推送）。
- `git push --set-upstream origin main`: 提交到远程仓库，并将其远程分支设为上游分支。
- `git push origin local_branch:remote_branch`: 推送本地分支到远程
- `git pull origin main`: 从远程仓库拉取更改并合并。
  - `--rebase`: 使用 rebase 而不是 merge。
- `git pull origin local_branch:remote_branch`: 拉取远程
- `git branch <branch name>`: 创建一个分支。
- `git branch --set-upstream master origin/next`: 手动建立追踪关系。
- `git branch -M main`: 将当前分支改名为 main。
- `git branch -d <branch name>`: 删除分支。
- `git branch`: 查看分支。
- `git checkout <branch name>`: 切换分支。
- `git checkout origin/main -- <filename>`: 从某个仓库某个分支检出文件到当前工作目录。
- `git checkout <commit id>`: 恢复到指定的 commit。
- `git checkout <commit id> -- <filename>`: 恢复某个特定文件到指定的 commit 状态。
- `git checkout (-b) branch-name`: （创建并）切换到分支。
- `git switch branch-name`: 切换到分支。
- `git log`: 查看log信息
- `git log --graph --oneline --all`: 可视化查看分支。
- `git clone <repository URL> [branch]`: 克隆远程仓库到本地计算机。
- `git stash`: 暂存当前更改。
- `git stash save “save message”`: 保存带有消息的暂存。
- `git stash list`: 列出所有暂存。
- `git stash show`: 显示最新的暂存内容。
- `git stash apply`: 应用最新的暂存。
- `git stash apply stash@{num}`: 应用指定的暂存。
- `git stash drop`: 删除最新的暂存。
- `git stash drop stash@{num}`: 删除指定的暂存。
- `git stash pop`: 应用最新的暂存并删除。
- `git stash clear`: 清除所有暂存。
- `git stash push --keep-index`: 保存工作目录中的更改而不包括暂存区中的更改。
- `git stash push -u`: 保存未被追踪的文件。
- `git diff`: 显示对未暂存文件的更改。
- `git diff [<options>] [<commit>] [--] [<path>…​]
- `git diff > changes.patch`: 保存补丁文件
- `git apply changes.patch`: 应用补丁文件
- `touch .gitignore`: 建立.gitignore文件
  - `#` 开头的行是注释。
  - `*` 匹配零个或多个字符。
  - `?` 匹配单个字符。
  - `[abc]` 匹配方括号内的任何一个字符。
  - `**` 匹配多级目录。
  - `!` 不忽略
- `lazygit`: Git 命令行 UI。

## 9. CMake

- `# comments  or  #[[ block comments ]]`
- `cmake_minimum_required(VERSION 3.0.0)`
- `set(CMAKE_CXX_STANDARD 17)`
- `set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Werror")`
- `project(PROJ_NAME)`
- `set(SRC_LIST add.c  div.c   main.c  mult.c  sub.c)`  # or  `set(SRC_LIST add.c;div.c;main.c;mult.c;sub.c)`
- `list(APPEND SRC_LIST ${SRC_LIST} ${TEMP})`
- `list(REMOVE_ITEM SRC_1 ${PROJECT_SOURCE_DIR}/main.cpp)`
- `include(MyCustomMacros)`
- `add_executable(app  ${SRC_LIST})`
- `target_compile_features(myapp PRIVATE cxx_std_17)`
- `target_compile_options(myapp [PRIVATE|PUBLIC|INTERFACE] -Wall -Wextra -Werror)`
- `add_dependencies(myapp mylib)`
- `aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src SRC_LIST)`
- `file(GLOB/GLOB_RECURSE MAIN_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)`
- `include_directories(${PROJECT_SOURCE_DIR}/include)`
- `find_package(Boost REQUIRED COMPONENTS system filesystem)`
- `add_dependencies(myapp mylib)`
- `add_library(slib STATIC ${SRC_LIST})`
- `add_library(dlib SHARED ${SRC_LIST})`
- `link_directories(${PROJECT_SOURCE_DIR}/lib)`
- `link_libraries(<static lib> [<static lib>...])`
- `target_link_libraries(<target> <PRIVATE|PUBLIC|INTERFACE> <item>... [<PRIVATE|PUBLIC|INTERFACE> <item>...]...)`
- `message([STATUS|WARNING|AUTHOR_WARNING|FATAL_ERROR|SEND_ERROR] "message to display" ...)`
- `add_definitions(-DDEBUG)`
- `add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL])`
- `install(TARGETS my_executable my_library`
  - `RUNTIME DESTINATION bin`
  - `LIBRARY DESTINATION lib`
  - `ARCHIVE DESTINATION lib)`
- `install(DIRECTORY include/ DESTINATION include)`
- `ExternalProject_Add(zlib`
  - `URL https://zlib.net/zlib-1.2.11.tar.gz`
  - `PREFIX ${CMAKE_BINARY_DIR}/zlib`
  - `INSTALL_DIR ${CMAKE_INSTALL_PREFIX}`
  - `CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>`
- `)`
- `ExternalProject_Get_Property(zlib INSTALL_DIR)`
- `enable_testing()`
- `add_executable(my_test_executable test.cpp)`
- `add_test(NAME my_test COMMAND my_test_executable)`
- `set_tests_properties(my_test PROPERTIES TIMEOUT 5)`
- `add_custom_target(run_tests`
  - `COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure`
  - `DEPENDS my_test_executable)`
- `add_custom_command(`
  - `OUTPUT student_mpirun.opt`
  - `COMMAND sed -n -E -e 's/.*!mpirun_option ((--bind-to|--map-by|--rank-by)\\s\\S+).*/\\1/p' ${CMAKE_SOURCE_DIR}/student_submission.cpp > student_mpirun.opt`
  - `DEPENDS student_submission.cpp`
  - `COMMENT "Generating student_mpirun.opt"`
- `)`
- `set_property()`

### CMake 常用变量 (CMake Variables):

- `PROJECT_SOURCE_DIR`, `PROJECT_BINARY_DIR`
- `CMAKE_CURRENT_SOURCE_DIR`, `CMAKE_CURRENT_BINARY_DIR`
- `CMAKE_BINARY_DIR`
- `LIBRARY_OUTPUT_PATH`, `EXECUTABLE_OUTPUT_PATH`
- `RUNTIME_OUTPUT_DIRECTORY`, `LIBRARY_OUTPUT_DIRECTORY`, `ARCHIVE_OUTPUT_DIRECTORY`
- `CMAKE_RUNTIME_OUTPUT_DIRECTORY`, `CMAKE_LIBRARY_OUTPUT_DIRECTORY`, `CMAKE_ARCHIVE_OUTPUT_DIRECTORY`
- `CMAKE_CXX_STANDARD`
- `CMAKE_CXX_FLAGS`, `CMAKE_CXX_FLAGS_DEBUG`, `CMAKE_CXX_FLAGS_RELEASE`
- `PROJECT_NAME`
- `CMAKE_BUILD_TYPE`

## 10. 编译和调试 (Compilation and Debugging)

- `mpicxx`: MPI C++ 编译器。
- `nvcc`: CUDA 编译器。
- `make`: 构建工具。
- `make clean`: 清理构建生成的文件。
- `make distclean`: 更彻底的清理。
- `make install`: 安装构建结果。
- `make uninstall`: 卸载
- `make program`: 编译
- `make debug`: debug编译
- `nvim CMakeLists.txt; mkdir build; cd build; cmake ..; make`: CMake 构建流程。
- `bear -- make`: 生成 compile_commands.json 文件（用于 clangd 等工具）。
- `bear -- g++ main.cpp`: 类似于上
- `g++ helloworld.cpp -fsanitize=address -g -o helloworld`: 检查是否有内存泄露，调试，改名。
- `g++ -c -o helper.o helper.cpp`: 编译object文件。
- `g++ -I path/to/eigen-version/ mycode.cpp -o mycode`: 添加eigen库。
- `g++ 'pkg-config --cflags eigen3' matvec.cpp -o matvec`: 使用eigen库。
- `-E`: 预处理
- `-S`: 汇编
  - `-O0`: 不优化
  - `-O1`: 优化
  - `-O2`: 更进一步优化
  - `-Os`: 文件大小优化
- `-c`: 只编译和汇编源代码，而不链接,生成object文件
- `-lld`: 查看链接库文件
- `-Wall`: 开启所有的警告信息
- `g++ -O2 -ftree-loop-vectorize -fopt-info-vec-all mycode.cpp`: 自动矢量化。
- `readelf -h code.o`: 显示elf文件信息
- `nm file_name`: 查看二进制文件的符号表信息。
- `echo $?:` 查看最近执行的命令的退出状态码。
- `clang-format:`
  - `clang-format -style=file -i mycode.cpp`: 格式化单个文件
  - `clang-format -style=file -i src/*`: 格式化src目录下的所有文件
  - `find . \( -iname "*.h" -o -iname "*.cpp" \) -exec clang-format -style=file -i {} \;`:格式化所有.h和.cpp文件
- `clang-tidy main.cpp -checks="cppcoreguidelines-*"`: 代码静态分析
- `clang-tidy main.cpp -checks=cppcoreguidelines-prefer-member-initializer -- -std=c++17`
- `Google Benchmark`: 基准测试
- `Profiling with gprof:`
  - `g++ -pg mycode.cpp -o mycode`
  - `./mycode`
  - `gprof mycode gmon.out > report.txt`
- `Profiling with perf:`
  - `perf record ./your_program`
  - `perf report`
- `Measuring time with the chrono library`
- `GCC: -finline-functions`: 内联所有函数
- `GCC: -ffast-math, -fassociative-math`: 优化flag
- `GCC: -O0 (no opt.), -O1 (optimize), -O2 (optimize more), -O3 (optimize most), -Ofast (optimize most and disregard IEEE/ISO specifications for math functions)`
- `GCC: -funroll-loops`: 展开循环
- `g++ -g your_program.cpp -o your_program && pahole your_program`
- `gdb helloworld:`
  - `b break <line number>`: 设置断点
  - `break line-or-function if (condition)`: 条件断点
  - `watch [watch var; watch(x>y)]`: 监视
  - `r run`: 运行
  - `s step`: 单步
  - `n next`: 下一行
  - `c continue`: 继续执行
  - `p print <variable>`: 打印
  - `l list`: 列出代码
  - `q quit`: 退出
  - `i info breakpoints(i b)/locals...`: 查看断点/局部变量信息
  - `until`: 执行到特定行
- `ulimit -c 0`: 禁止生成核心转储文件
- `ulimit -c unlimited`: 允许生成核心转储文件，不限制大小
- `gdb ./my_program core`: 调试core文件

### task.json:

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

### launch.json:

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
			"miDebuggerPath": "/usr/bin/gdb"
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

## 11. Java

- `java -version`: 查看版本
- `javac <file-name.java>`: 编译。
- `java <file-name>`: 运行。
- `javac -d . MyPackageClass.java`: 编译 package，它被存储在 root/mypack/MyPackageClass.java。
- `java mypack.MyPackageClass`: 运行这个 package。

## 12. C# (.NET)

- `dotnet --version`: 查看版本
- `dotnet new <template> -n <project_name>`: 创建项目。
  - `example：dotnet new console -n MyApp`