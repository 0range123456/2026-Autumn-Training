## Linux 操作系统

### 1. 搭建 Linux 环境

已成功下载并安装 VMware + Kali。

<!-- 这是一张图片，ocr 内容为： -->
![](https://github.com/user-attachments/assets/1effce01-3a49-4c35-9396-939e9b327cab)

#### 设置 root 账户与国内镜像源

```bash
# 设置 root 密码
sudo passwd root

# 切换到 root 用户
su root
```

配置国内镜像源：

1. 更新软件包索引：

   ```bash
   apt update
   ```

2. 编辑软件源列表：

   ```bash
   vim /etc/apt/sources.list
   ```

   写入以下内容：

   ```text
   deb https://mirrors.aliyun.com/kali kali-rolling main non-free contrib non-free-firmware
   ```

3. 清理下载包和缓存，然后再次更新：

   ```bash
   apt clean
   apt update
   ```

#### 安装、卸载与查看软件

| 操作 | 命令 |
| --- | --- |
| 安装软件 | `apt install <软件名>` |
| 卸载软件 | `apt remove <软件名>` |
| 查看软件 | `apt show <软件名>` |

#### 常用快捷键

| 快捷键 | 作用 |
| --- | --- |
| `Ctrl+Shift+C` | 复制 |
| `Ctrl+Shift+V` | 粘贴 |
| `Ctrl+L` | 清屏 |
| `Ctrl+C` | 停止运行 |
| `Ctrl+E` | 光标移至行尾 |
| `Ctrl+A` | 光标移至行首 |
| `Ctrl+U` | 清除光标至行首的内容 |
| `Tab` | 补全命令 |
| `↑` / `↓` | 上一条 / 下一条命令 |
| 右键 | 补全提示 |
| `Ctrl+D` | 关闭 shell 会话 |

### 2. 学习基本 Linux 命令

命令手册：[Linux 命令手册（菜鸟教程）](https://www.runoob.com/linux/linux-command-manual.html)

#### 帮助与目录查看

- `man <命令>`：查看完整手册（最全）
- `<命令> --help`：查看简要帮助
- `whatis <命令>`：查看命令的核心说明

查看目录内容：

| 命令 | 作用 |
| --- | --- |
| `ls` | 列出当前目录内容 |
| `ls -a` | 显示隐藏文件 |
| `ls -t` | 按最近修改时间排序 |
| `ls -l` | 详细展示文件信息 |
| `ls -lh` | 以易读单位（KB/MB）展示文件大小 |

#### 目录进出

| 命令 | 作用 |
| --- | --- |
| `cd <目录>` | 进入目录 |
| `cd ./` | 进入当前目录 |
| `cd ..` | 返回上一级目录 |
| `cd /` | 返回根目录 |

#### 查看文件内容

| 命令 | 作用 |
| --- | --- |
| `file <文件>` | 查看文件类型 |
| `cat <文件>` | 查看小文件 |
| `more <文件>` | 分页查看；空格键翻页，按 `q` 退出 |
| `less <文件>` | 可上下滚动，不会一次性加载全部内容；按 `/` 搜索，`n` 下一个，`N` 上一个 |
| `head <文件>` | 查看前 10 行 |
| `tail <文件>` | 查看后 10 行 |

#### 创建、复制、移动与删除

- 创建目录：`mkdir <目录>`
- 创建多级目录：`mkdir -p b/c/d`
- 创建文件：`touch <文件>`
- 复制文件：`cp 1.txt <目标目录>`
- 复制目录：`cp -r a b`（`-r` 表示递归）
- 移动或重命名：`mv <文件> <目标目录>`；`mv <旧名字> <新名字>`
- 删除文件：`rm <文件>`
- 强制删除：`rm -f`
- 递归删除文件夹：`rm -rf <目录>`

#### 通配符与管道

- `*`：匹配任意字符
- `?`：匹配单个字符
- `[ ]`：匹配指定范围，例如 `[0-9]`、`[a-z]`
- `|`：管道，把前一个命令的输出作为后一个命令的输入

#### 查找：find 与 grep

按文件名查找：

```bash
find . -name "*.txt"   # . 表示当前目录
```

按文件类型查找：

```bash
find . -type d   # -type d 表示查找目录
```

按文件大小查找：

```bash
find . -size +1M   # 查找当前目录下大于 1M 的文件
```

在文件内容中查找：

```bash
grep "关键字" 文件名
```

#### 编辑文件：vim

```bash
vim 文件名
```

| 操作 | 按键 |
| --- | --- |
| 进入编辑模式 | `i` |
| 退出编辑模式 | `Esc` |
| 保存并退出 | `:wq` |
| 强制退出 | `:q!` |

#### 文件压缩与解压

**zip（兼容性好）**

压缩：

```bash
zip -r 压缩包名.zip 文件或目录   # -r 表示递归压缩目录；多个文件用空格隔开
```

解压：

```bash
unzip 压缩包名.zip           # 解压到当前目录
unzip 压缩包名.zip -d /a/b   # 解压到指定目录
```

**tar（最常用）**

`-v` 表示显示压缩 / 解压过程；多个文件用空格隔开。

| 操作 | 命令 |
| --- | --- |
| 压缩 | `tar -czvf 压缩包名.tar.gz 文件或目录` |
| 解压 | `tar -xzvf 压缩包名.tar.gz` |
| 解压到指定目录 | `tar -xzvf 压缩包名.tar.gz -C ./output`（也可写 `/home/ctf/output`） |
| 查看压缩包内容 | `tar -tzvf 压缩包名.tar.gz` |

#### Linux 网络管理

| 操作 | 命令 |
| --- | --- |
| 查看网络信息 | `ifconfig`（未安装时执行 `apt install net-tools -y`） |
| 查看所有网卡信息 | `ip addr` |
| 测试网络连通性 | `ping www.baidu.com` |
| 指定 ping 4 次后停止 | `ping -c 4 www.baidu.com` |
| 测试 HTTP 访问（返回 HTML 内容） | `curl https://www.baidu.com` |
| 查看防火墙状态 | `ufw status` |
| 开启防火墙 | `ufw enable` |
| 关闭防火墙 | `ufw disable` |

#### Linux 系统资源与进程管理

| 操作 | 命令 |
| --- | --- |
| 查看进程 | `ps aux` |
| 交互式查看进程（更好用） | `htop`（需安装：`apt install htop`） |
| 启动服务 | `systemctl start nginx` |
| 停止服务 | `systemctl stop nginx` |
| 重启服务 | `systemctl restart nginx` |
| 查看服务状态 | `systemctl status nginx` |
