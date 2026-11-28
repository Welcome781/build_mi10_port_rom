# build_mi10_port_rom

基于 GitHub Actions 的 HyperOS 移植仓库，面向小米 10 系列（代号 `umi`，小米 10；`cmi`，小米 10 Pro）。

本仓库 fork 自 [hulove/build_mi10_port_rom](https://github.com/hulove/build_mi10_port_rom)，移植流程使用 [toraidl/hyperos_port](https://github.com/toraidl/hyperos_port)（`ext` 分支）。

---

## 当前产物

| Release | 系统版本 | 机型 | Android | 文件系统 | 分卷 |
| --- | --- | --- | --- | --- | --- |
| [port-7](https://github.com/Welcome781/build_mi10_port_rom/releases/tag/port-7) | HyperOS 3.0.302.0.UJBCNXM | 小米 10（umi） | 16 | EROFS | 4 卷（zip00–03） |

### 包信息（来自产物文件名）

- 机型代号：`umi`（小米 10）
- 系统：HyperOS 3.0.302.0.UJBCNXM（国内版本号命名）
- Android：16
- 移植来源代号：`nuwa`
- 打包类型：EROFS
- 产物文件名标注：`ROOT`
- 分片大小：1536 MB / 卷

### 底包与移植包（Actions 默认参数）

- 底包（STOCK_URL）：UMI MIUI 14 `V14.0.4.0.TJBCNXM`（Android 13 官方整包）
- 移植包（PORT_URL）：示例为 `nuwa` EEA 全球版 `OS3.0.302.0.WMBEUXM`（Android 16 整包）；实际产物版本号以 Release 页为准

---

## 下载

Release 页：<https://github.com/Welcome781/build_mi10_port_rom/releases/tag/port-7>

产物前缀：

```
hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip
```

需要把 4 个分卷全部下载，缺一不可：

| 分卷 | 大小 | 链接 |
| --- | --- | --- |
| zip00 | 1.5 GB | <https://github.com/Welcome781/build_mi10_port_rom/releases/download/port-7/hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip00> |
| zip01 | 1.5 GB | <https://github.com/Welcome781/build_mi10_port_rom/releases/download/port-7/hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip01> |
| zip02 | 1.5 GB | <https://github.com/Welcome781/build_mi10_port_rom/releases/download/port-7/hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip02> |
| zip03 | 1.34 GB | <https://github.com/Welcome781/build_mi10_port_rom/releases/download/port-7/hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip03> |

> 直连 GitHub 较慢或打不开时，可在 `github.com` 前套一层可用的 GitHub 加速代理；代理可用性不稳定，失败就换一个。

## 合并分卷

### Linux / Termux

确保 4 个分卷在同一目录，按后缀顺序合并：

```bash
cat hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip00 \
    hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip01 \
    hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip02 \
    hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip03 \
  > rom.zip
```

### Windows（cmd）

```cmd
copy /b hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip00+hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip01+hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip02+hyperos_UMI_OS3.0.302.0.UJBCNXM_37329564fa_16_miproduct_nuwa_11221503_ROOT_EROFS.zip03 rom.zip
```

## 校验

- 发布说明要求：合并为完整 zip 后**校验 MD5 再刷入**。
- 当前 Release 页未附带独立的 MD5 / SHA 校验文件；如后续作者补充，以 Release 页附件为准。合并后若能和发布页给出的校验值比对，务必一致再刷。

## 刷入前提与注意事项

- 必须**已解锁 BL**；未解锁 BL 无法刷入。
- 刷入前务必备份 `data` 与重要资料，移植包刷入会清空数据。
- 这是第三方移植体验包，**不支持小米服务器在线更新（OTA）**。
- 第三方移植可能存在指纹支付、相机、蓝牙、传感器等功能不全或异常，具体以实机测试为准；本仓库未单独列出已知 bug。
- 包内标注 `ROOT`，但 root 管理器是否完整可用以实机为准。
- 刷入方式以包内说明为准：通常为已解 BL 后通过自定义 Recovery 刷入合并后的 `rom.zip`；若包内附带 fastboot 脚本，则按脚本说明操作。

## 用 GitHub Actions 重新构建

工作流：`.github/workflows/actions_build_hyperos_port_mi10.yml`

在仓库 **Actions → 对应工作流 → Run workflow**，可填写以下参数：

| 参数 | 说明 | 默认值 |
| --- | --- | --- |
| `STOCK_URL` | 底包直链（umi MIUI14 官方整包，含 `payload.bin`） | `V14.0.4.0.TJBCNXM` 整包 |
| `PORT_URL` | 移植包直链（别机型 OS3 / A16 官方或 eu 整包，含 `payload.bin`） | `nuwa` EEA `OS3.0.302.0.WMBEUXM` |
| `PACK_TYPE` | 打包类型：`EROFS` / `EXT`（EXT 可解锁 system 读写） | `EROFS` |
| `ENCRYPTION` | 是否解密 data 分区：`TRUE` / `FALSE` | `FALSE` |
| `GITHUBRELEASE` | 产物是否上传到本仓库 GitHub Release | `TRUE` |

产物由 Actions 自动按 1536 MB 分片并上传到 Release，Release tag 为 `port-<run_number>`。

## 致谢

- 上游模板：[hulove/build_mi10_port_rom](https://github.com/hulove/build_mi10_port_rom)
- 移植工具：[toraidl/hyperos_port](https://github.com/toraidl/hyperos_port)

## 免责声明

刷机存在变砖、数据丢失、失去保修等风险。本仓库仅提供移植打包流程与第三方产物，使用者需自行承担刷机后果；请在操作前完整备份，并确认自己已了解对应机型的刷机流程。
