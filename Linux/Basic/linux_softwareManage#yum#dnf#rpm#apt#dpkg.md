# Linux 软件管理（yum/dnf/rpm，apt/apt-get/dpkg）

</br>
</br>


## 一、yum/dnf

RPM 系（红帽、CentOS、Rocky、Fedora，`.rpm`包）

1. **YUM**，**Yellowdog Updater, Modified** ：修改版 Yellowdog 更新器（CentOS7/RHEL7 默认）。YellowDog 是早年一个 Linux 发行版，YUM 基于它的更新工具二次修改。

2. **DNF**，**Dandified YUM** ：华丽版 / 升级版 YUM（RHEL8+/CentOS8+/Fedora 默认；`yum`命令只是 dnf 的软链接）

3. **RPM**，**RPM Package Manager**（原先叫 RedHat Package Manager）：RPM 软件包管理器。yum/dnf 的底层工具。

| 操作   | yum/dnf命令                                                  | rpm命令                        |
| ---- | ---------------------------------------------------------- | ---------------------------- |
| 更新源  | yum/dnf clean all && yum/dnf makecache                     |                              |
| 在线安装 | yum/dnf install <package_name>[-<version\>]                |                              |
| 本地安装 | yum/dnf localinstall <rpm_name>                            | rpm -ivh <rpm_name>          |
| 升级包  | yum/dnf upgrade <package_name> </br> 升级所有：  yum/dnf update | rpm -Uvh <rpm_name>          |
| 卸载包  | yum/dnf remove <package_name>                              | rpm -e [--nodeps] <rpm_name> |
| 降级包  | yum/dnf downgrade <package_name>-<version\>                |                              |
| 搜索包  | yum/dnf search <package_key>                               |                              |
| 已安装  | yum/dnf list installed [<package_name>]                    | rpm -qa [<rpm_name>]         |
| 包详情  | yum/dnf info <package_name>                                | rpm -qi <rpm_name>           |
| 包依赖  | yum/dnf deplist <package_name>                             | rpm -qR <rpm_name>           |
| 可用版本 | yum/dnf list <package_name> --showduplicates               |                              |
| 锁定版本 | yum/dnf versionlock add <package_name>[-version]           |                              |
| 解锁锁定 | yum/dnf versionlock delete <package_name>                  |                              |
| 清空锁定 | yum/dnf versionlock clear                                  |                              |
| 查看锁定 | yum/dnf versionlock list                                   |                              |

tip: 锁定版本需要自己安装  yum/dnf install yum-plugin-versionlock

</br>

## 二、apt/apt-get

DEB 系（Debian、Ubuntu，`.deb`包）

1. **APT**，**Advanced Package Tool** ：高级包工具。底层依赖`dpkg`，apt /apt-get/apt-cache 都属于 APT 工具集

2. **dpkg**，**Debian Package**。apt 的底层工具

| 操作   | apt/apt-get命令                                                                    |
| ---- | -------------------------------------------------------------------------------- |
| 编辑源  | apt edit-sources [<source_name>]                                                 |
| 更新源  | apt/apt-get update                                                               |
| 安装包  | apt/apt-get install <package_name>[=<version\>]                                  |
| 升级包  | apt/apt-get upgrade <package_name>                                               |
| 卸载包  | apt/apt-get remove <package_name>                                                |
| 清除包  | apt/apt-get purge <package_name>                                                 |
| 已安装  | apt list [<package_name>] -i/--installed                                         |
| 查找包  | apt/apt-cache search <package_name> --names-only                                 |
| 安装细节 | apt/apt-cache show <package_name>                                                |
| 修复依赖 | apt-get -f install                                                               |
| 卸载依赖 | apt-get autoremove                                                               |
| 历史版本 | apt-cache policy <package_name>                                                  |
| 可用版本 | apt list <package_name> -a/--all-versions </br> apt-cache madison <package_name> |
| 锁定版本 | apt-mark hold <package_name>                                                     |
| 解锁锁定 | apt-mark unhold <package_name>                                                   |
| 查看锁定 | apt-mark showhold                                                                |

</br>

## 三、dpkg/rpm

| 操作  | dpkg                                               | rpm                |
| --- | -------------------------------------------------- | ------------------ |
| 安装  | dpkg -i <deb_name> [--force-depends]               |                    |
| 升级  |                                                    |                    |
| 卸载  | dpkg -r <deb_name>                                 |                    |
| 清除  | dpkg -P/--purge <deb_name>                         |                    |
| 查找  | dpkg -l <deb_name> </br>  dpkg-query -W "*chrome*" |                    |
| 包信息 | dpkg -s <deb_name>                                 | rpm -qi <rpm_name> |
| 包内容 | dpkg -L <deb_name>                                 | rpm -ql <rpm_name> |
| 查包名 | dpkg -S <file_name>                                |                    |

</br>

## 源码编译安装

> 以 nginx 安装为例：

1. 准备源代码包

    ```sh
    #从项目的官方网站或代码仓库（如 GitHub）下载源代码
    wget https://nginx.org/download/nginx-1.24.0.tar.gz     #下载
    tar -xzvf nginx-1.24.0.tar.gz       #解压
    cd nginx-1.24.0
    ```

2. 准备编译环境

    ```sh
    #安装编译工具和依赖，比如 gcc（GNU 编译器集合）和 make。
     yum install pcre-devel zlib-devel # 安装 nginx 依赖包
    ```

3. 安装（3步曲）

    ```sh
    #配置构建环境
    ./configure --prefix=/usr/local/nginx  #--prefix指定了Nginx的安装目录，其他的配置项按需添加

    make    #编译

     make install    #安装
    ```

4. 验证

    ```sh
    #启停验证
    /usr/local/nginx/sbin/nginx -version
     /usr/local/nginx/sbin/nginx
     /usr/local/nginx/sbin/nginx -s stop
    ```

5. 软链接（可选）

    ```sh
    #查看 PATH 环境
    echo $PATH

    #创建软链接
     ls -s /usr/local/nginx/sbin/nginx /usr/sbin/nginx

    #测试软链接
    nginx -version
     nginx
     nginx -s stop
    ```

6. 卸载

    ```sh
    #1. 删除软链接
     rm -f /usr/sbin/nginx

    #2. 删除安装目录
     rm -rf /usr/local/nginx

    #2. 执行make自带的卸载程序卸载（部分软件支持）
    # make uninstall
    ```

    note：因为没有使用包管理器安装，所以需要手动删除安装的文件。如果你在配置时指定了 --prefix，只需删除该目录即可。

</br>

## 结论

- 方便性：“apt-get机制”最优，“dpkg机制”次之，“从源码编译安装机制”最末。“apt-get机制”已经预先解决依赖问题，“从源码编译安装机制”需要我们自己解决依赖问题
- 可定制性：“从源码编译安装机制”最优，“dpkg机制”次之，“apt-get机制”最末。“从源码编译安装机制”允许我们自定义安装参数，“apt-get机制”几乎完全采用默认的安装参数
- 所需权限：“从源码编译安装机制”所需权限可以是最小，“dpkg机制”次之，“apt-get机制”所需权限最大。在我们不拥有较高权限（比如root权限）的情况下，只能采用“从源码编译安装机制”

</br>
</br>

Via

- <http://c.biancheng.net/view/2952.html>
- <https://rqsir.github.io/2019/04/13/linux-make-install%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E5%8D%B8%E8%BD%BD/>
- <https://blog.csdn.net/liudsl/article/details/79200134>
