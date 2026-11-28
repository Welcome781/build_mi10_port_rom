# build_mi10_port_rom

基于 GitHub Actions 的 HyperOS 移植仓库，面向小米 10 系列（代号 umi，小米 10；cmi，小米 10 Pro）。

本仓库 fork 自 hulove/build_mi10_port_rom，移植流程使用 toraidl/hyperos_port（ext 分支）。

---

## 当前产物

| Release | 系统版本 | 机型 | Android | 文件系统 | 分卷 |
| --- | --- | --- | --- | --- | --- |
| port-7 | HyperOS 3.0.302.0.UJBCNXM | 小米 10（umi） | 16 | EROFS | 4 卷（zip00–03） |

### 包信息（来自产物文件名）

- 机型代号：umi（小米 10）
- 系统：HyperOS 3.0.302.0.UJBCNXM（国内版本号命名）
- Android：16
- 移植来源代号：nuwa
- 打包类型：EROFS
- 产物文件名标注：ROOT
- 分片大小：1536 MB / 卷
- 合并后完整包大小：6,268,358,087 字节（约 5.84 GiB / 6.27 GB）

### 底包与移植包（Actions 默认参数）

- 底包（STOCK_URL）：UMI MIUI 14 V14.0.4.0.TJBCNXM（Android 13 官方整包）
- 移植包（PORT_URL）：示例为 nuwa EEA 全球版 OS3.0.302.0.WMBEUXM（Android 16 整包）；实际产物版本号以 Release 页为准

---

## 下载

Release 页：https://github.com/Welcome781/build_mi10_port_rom/releases/tag/port-7

产物前缀：

hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip

需要把 4 个分卷全部下载，缺一不可：

| 分卷 | 大小 | 链接 |
| --- | --- | --- |
| zip00 | 1.5 GB | https://github.com/Welcome781/build_mi10_port_rom/releases/download/port-7/hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip00 |
| zip01 | 1.5 GB | https://github.com/Welcome781/build_mi10_port_rom/releases/download/port-7/hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip01 |
| zip02 | 1.5 GB | https://github.com/Welcome781/build_mi10_port_rom/releases/download/port-7/hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip02 |
| zip03 | 1.34 GB | https://github.com/Welcome781/build_mi10_port_rom/releases/download/port-7/hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip03 |

> 直连 GitHub 较慢或打不开时，可在 github.com 前套一层可用的 GitHub 加速代理；代理可用性不稳定，失败就换一个。

## 合并分卷

### Linux / Termux

确保 4 个分卷在同一目录，按后缀顺序合并：

cat hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip00 \
    hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip01 \
    hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip02 \
    hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip03 \
  > rom.zip

### Windows（cmd）

copy /b hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip00+hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip01+hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip02+hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip03 rom.zip

## 校验

合并为完整 zip 后，先校验哈希一致再刷入。以下为当前 port-7 完整包（合并后文件名见上方「产物前缀」）实测值：

| 算法 | 哈希值 |
| --- | --- |
| MD5 | 37329564fa7d74763332d0494b7b152b |
| SHA256 | 1bbaf0648da25d4cfa36a6cd5c576691ac1a9c81d36a1923521bc5762fb6a7a3 |

- Linux / Termux：md5sum rom.zip、sha256sum rom.zip
- Windows：certutil -hashfile rom.zip MD5、certutil -hashfile rom.zip SHA256
- 仓库附带 CHECKSUMS.md 以及 *.md5 / *.sha256 校验文件，可直接用 md5sum -c *.md5 / sha256sum -c *.sha256 校验。
- 哈希不一致说明分卷下载不全或损坏，请重新下载对应分卷后再合并。

## 包内结构（合并后的 rom.zip）

- super.zst：超级分区压缩包，刷入脚本会先解压为 super.img
- boot_official.img：boot 镜像
- firmware-update/：dtbo.img、vbmeta.img、vbmeta_system.img、logo.img 及 xbl / tz / abl / modem / hyp 等固件镜像
- windows_flash_script.bat：Windows 刷入脚本
- mac_linux_flash_script.sh：macOS / Linux 刷入脚本
- bin/windows/：fastboot.exe、adb.exe、zstd.exe 等 Windows 工具

## 刷入前提与注意事项

- 必须已解锁 BL；未解锁 BL 无法刷入。
- 本包为 fastboot 线刷包（不是 Recovery 卡刷包），需在电脑上通过脚本刷入。
- 刷入前务必备份重要资料；选择清除数据时会清空 userdata 与 metadata（/data/media 内部存储保留）。
- 脚本会校验机型 product 必须为 umi；机型不匹配时会提示是否仍继续。
- 这是第三方移植体验包，不支持小米服务器在线更新（OTA）。
- 第三方移植可能存在指纹支付、相机、蓝牙、传感器等功能不全或异常，具体以实机测试为准；本仓库未单独列出已知 bug。
- 包内标注 ROOT，但 root 管理器是否完整可用以实机为准。

## 刷入步骤（fastboot 线刷）

1. 将合并并校验通过的 rom.zip 解压到电脑上的一个目录。
2. 手机解锁 BL 后关机，进入 bootloader / fastboot 模式（adb reboot bootloader，或关机后音量下 + 电源键）。
3. Windows 运行 windows_flash_script.bat；macOS / Linux 运行 mac_linux_flash_script.sh。
4. 脚本先用自带 zstd 把 super.zst 解压成 super.img，并校验当前设备机型为 umi。
5. 选择刷入方式：
   - 1：保留用户数据刷入；
   - 2：清除 userdata + metadata（保留内部存储 /data/media）。
6. 脚本依次刷入 boot、dtbo、固件分区，重启回 bootloader 后刷入 super，最后重启手机。
7. 首次开机较慢，若长时间未重启可手动长按电源键重启。

## 用 GitHub Actions 重新构建

工作流：.github/workflows/actions_build_hyperos_port_mi10.yml

在仓库 Actions → 对应工作流 → Run workflow，可填写以下参数：

| 参数 | 说明 | 默认值 |
| --- | --- | --- |
| STOCK_URL | 底包直链（umi MIUI14 官方整包，含 payload.bin） | V14.0.4.0.TJBCNXM 整包 |
| PORT_URL | 移植包直链（别机型 OS3 / A16 官方或 eu 整包，含 payload.bin） | nuwa EEA OS3.0.302.0.WMBEUXM |
| PACK_TYPE | 打包类型：EROFS / EXT（EXT 可解锁 system 读写） | EROFS |
| ENCRYPTION | 是否解密 data 分区：TRUE / FALSE | FALSE |
| GITHUBRELEASE | 产物是否上传到本仓库 GitHub Release | TRUE |

产物由 Actions 自动按 1536 MB 分片并上传到 Release，Release tag 为 port-<run_number>。

## 致谢

- 上游模板：hulove/build_mi10_port_rom
- 移植工具：toraidl/hyperos_port

## 免责声明

刷机存在变砖、数据丢失、失去保修等风险。本仓库仅提供移植打包流程与第三方产物，使用者需自行承担刷机后果；请在操作前完整备份，并确认自己已了解对应机型的刷机流程。
