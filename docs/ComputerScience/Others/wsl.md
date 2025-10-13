---
comments: true
---

# WSL

- [Windows Subsystem for Linux 文档](https://learn.microsoft.com/zh-cn/windows/wsl/)

## WSL2 VHDX 空间回收

WSL2 通过一个虚拟硬盘文件（`ext4.vhdx`）运行 Linux 发行版，当用户在 Linux 环境中安装软件或创建文件时，这个 `.vhdx` 文件会自动增长。但当用户删除文件时，虚拟硬盘文件并不会自动收缩。它只是在文件系统内部将那些空间标记为“可用”，但从 Windows 的角度看，这个虚拟硬盘文件本身的大小没有变化。因此，Windows 中显示的文件大小，实际上是 `.vhdx` 文件的峰值大小，而不是当前实际使用的大小。  

要解决这个问题，我们需要手动对虚拟硬盘文件进行压缩，将未使用的空间还给 Windows。操作步骤如下：  

1. 找到 `.vhdx` 文件

    以我系统下的路径为例，`ext4.vhdx` 文件位于以下路径处：

    ```
    C:\Users\voilern\AppData\Local\wsl\{e2800d09-529e-46a3-87b6-bec3cfaf8c7a}
    ```

    若有差异，可参考官方文档： [如何查找 Linux 分发版的 .vhdx 文件和磁盘路径](https://learn.microsoft.com/zh-cn/windows/wsl/disk-space#how-to-locate-the-vhdx-file-and-disk-path-for-your-linux-distribution)

2. 关闭 WSL

    ```bash
    wsl --shutdown
    ```

    可通过 `wsl -l -v` 查看虚拟机的运行状态：

    ```bash
    > wsl -l -v
    > wsl -l -v
    NAME              STATE           VERSION
    * Ubuntu-24.04      Stopped         2
    ```

3. 使用 `diskpart` 工具压缩 `.vhdx` 文件

    以管理员身份打开 Powershell，输入 `diskpart` 并回车，进入 `diskpart` 环境，依次执行以下命令：

    ```bash
    # 1. 选择虚拟磁盘文件
    select vdisk file="C:\Users\voilern\AppData\Local\wsl\{e2800d09-529e-46a3-87b6-bec3cfaf8c7a}"

    # 2. 以只读方式附加磁盘
    attach vdisk readonly

    # 3. 压缩虚拟磁盘
    compact vdisk

    # 4. 分离虚拟磁盘
    detach vdisk

    # 5. 退出 diskpart
    exit
    ```

    结束后，`.vhdx` 文件占用的空间即被释放。