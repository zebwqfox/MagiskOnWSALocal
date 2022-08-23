# 子系统 上的 Magisk（包含谷歌全家桶）

## 首先准备

- Ubuntu（可以用 WSL2）

## 特点

- 在几分钟内点击几下即可一键安装 Magisk 和 OpenGApps
- 让每个编译代码保持最新状态
- 支持 ARM64 和 x64架构
- 支持除aroma之外的所有OpenGApps变体（aroma不支持x86_64，请使用super代替）
- 修复 VPN 对话框不显示（使用我们的 [VpnDialogs 应用程序](https://github.com/LSPosed/VpnDialogs)）
- 一键傻瓜式无人值守自动安装
- 在 Windows 11 中自动激活开发者模式
- 更新到新版本，同时通过一键脚本保存数据
- 合并所有语言包
- 支持管理开始菜单图标（手动安装 [WSAHelper](https://github.com/LSPosed/WSAHelper/releases/latest) 使用此功能）

## 文字版指南

1. 点个星（如果你想的话）
1. 将仓库源码克隆到本地
1. 运行 `scripts/run.sh`
1. 选择Magisk的版本，然后选择你喜欢的【OpenGApps变体】（https://github.com/opengapps/opengapps/wiki#variants），选择root解决方案（none表示没有root），选择WSA版本和它的架构（主要是 x64）
1. 等待脚本完成，编译完成后的文件会在 `output` 文件夹存放。

1. 将一键脚本移动到你喜欢的地方
1. 右键单击 `Install.ps1` 并选择 `用 PowerShell运行（管理员）`
    - 如果您之前安装了 MagiskOnWSA，它会自动卸载之前的版本，同时 **会保留所有用户数据** 并安装新的，所以不用担心您的数据会丢失。
    - 如果您已经安装了正式的 WSA ，则应先将其卸载。 （如果你想保留你的数据，你可以在卸载前备份 `%LOCALAPPDATA%\Packages\MicrosoftCorporationII.WindowsSubsystemForAndroid_8wekyb3d8bbwe\LocalCache\userdata.vhdx` 这个路径里的文件，安装后覆盖。）（如果你想恢复图标到开始菜单，请安装并使用 [WSAHelper](https://github.com/LSPosed/WSAHelper/releases/latest)。）
    - 如果弹出窗口消失 **并且也没有和你要管理员权限** ，并且 WSA 没有成功安装，你应该以管理员身份手动运行 `Install.ps1` :
        1. 按 `Win+x` 并选择 `Windows终端（管理员）`
        2. 输入 `cd "{X:\path\to\your\extracted\folder}"` 并 `回车`, 记得替换 `{X:\path\to\your\extracted\folder}` 包含 `{}`的, 打个比方 `cd "D:\wsa"`
        3. 输入 `PowerShell.exe -ExecutionPolicy Bypass -File .\Install.ps1` 并按 `enter`
        4. 脚本将运行并安装 WSA
        5. 如果此解决方法不起作用，则您的 PC 不支持 WSA
1. Magisk/Play store 将会自动启动. Enjoy by installing LSPosed-zygisk with zygisk enabled or Riru and LSPosed-riru

## FAQ

- 我可以删除已安装的文件夹吗？

    不行。
    
- 如何将 WSA 更新到新版本？

    删除“下载”文件夹
    重新运行脚本，替换之前安装的内容并重新运行“Install.ps1”。不用担心，您的数据将被保留。
    
- 如何从 WSA 获取 logcat？

    `%LOCALAPPDATA%\Packages\MicrosoftCorporationII.WindowsSubsystemForAndroid_8wekyb3d8bbwe\LocalState\diagnostics\logcat`
    
- 如何将 Magisk 更新到新版本？

    执行与更新 WSA 相同的操作
    
- 未启用虚拟化？

    如果未启用，`Install.ps1` 会帮助您启用它。重新启动后，重新运行“Install.ps1”以安装 WSA。如果仍然无法正常工作，则必须在 BIOS 中启用虚拟化。这是一个很长的故事，所以向谷歌寻求帮助。
    
- 如何以读写方式重新安装系统？

    在 WSA 中没有办法，因为它是由 Hyper-V 以只读方式安装的。您可以通过制作 Magisk 模块来修改系统。或者直接修改system.img。向谷歌寻求帮助。
    
- 我不能`adb connect localhost:58526`

    确保已启用开发者模式。如果问题仍然存在，请在设置页面检查 WSA 的 IP 地址并尝试 `adb connect ip:5555`。
    
- Magisk 在线模块列表为空？

    Magisk 主动删除在线模块存储库。您可以在本地安装模块，也可以通过 `adb push module.zip /data/local/tmp` 和 `adb shell su -c magisk --install-module /data/local/tmp/module.zip` 安装模块。
    
- 我可以使用 Magisk 23.0 稳定版或更低版本吗？

    不，Magisk 存在阻止自身在 WSA 上运行的错误。 Magisk 24+ 已修复它们。所以你必须使用 Magisk 24 或更高版本。
    
- 我怎样才能摆脱 Magisk？

    选择“无”作为root解决方案。
    
## Credits

- [StoreLib](https://github.com/StoreDev/StoreLib): API for downloading WSA
- [Magisk](https://github.com/topjohnwu/Magisk): The most famous root solution on Android
- [The Open GApps Project](https://opengapps.org): One of the most famous Google Apps packages solution
- [WSA-Kernel-SU](https://github.com/LSPosed/WSA-Kernel-SU) and [kernel-assisted-superuser](https://git.zx2c4.com/kernel-assisted-superuser/): The kernel `su` for debugging Magisk Integration
- [WSAGAScript](https://github.com/ADeltaX/WSAGAScript): The first GApps integration script for WSA
