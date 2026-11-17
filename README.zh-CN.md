# Hisense Q5 屏幕校色

[简体中文](README.zh-CN.md) · [English](README.en.md)

面向 Hisense Q5 RLCD 平板的 Root 屏幕校色工具。当前版本为 `0.1.51`。

## 功能

- 调节面板对比度、面板饱和度、Gamma、锐度和红绿蓝增益。
- 单独调节 Android 系统饱和度。系统饱和度与面板 HSV 饱和度属于不同处理阶段，效果会叠加。
- 在主界面和悬浮面板中使用同步的滑杆和值显示；拖动时应用参数，点击数值可输入绝对值。
- 从主界面打开时悬浮面板默认展开。面板可以移动、收起、重新展开或关闭。
- 爱心按钮应用固定的推荐设置；普通调节不会改写推荐值。
- 恢复操作会关闭本应用启用的面板模块，并恢复应用记录的系统饱和度基线，不会清除设备数据。

## 设备与权限

| 项目 | 当前验证信息 |
|---|---|
| 设备 | Hisense Q5 HITV105C / T610 |
| 最低系统版本 | Android 8.0（API 26）；实机验证为 Android 10 |
| 固件 | `L1721.6.01.02` |
| 面板驱动 | `dpu-r4p0` |
| Root | 必需；通过 Magisk 授权 |
| 悬浮窗 | 首次使用时需授予“显示在其他应用上层”权限 |

面板写入前会检查设备指纹和 DPU 版本。当前仅上述 Q5 固件经过验证；其他机型、固件和 DPU 版本不保证可用。应用没有 Shizuku 调整通道。

面板写入接受的系统指纹：`Hisense/HITV105C/HITV105C:10/QP1A.190711.020/L1721.6.01.02:user/release-keys`。

## 调节范围

| 参数 | 范围 |
|---|---:|
| 对比度 | 0.00–31.99 |
| 面板饱和度 | 0.00–64.00 |
| Gamma | 0.10–113.04 |
| 锐度 | 0.00–1.60 |
| 红、绿、蓝增益 | 各 0.00–31.99 |
| 系统饱和度 | 0.00–2.00 |

面板参数使用非线性滑杆，`1.00` 位于滑杆中点；绝对值输入精度为 `0.01`。范围依据当前厂商表和 DPU 字段编码确定，表示编码边界，不代表建议观感值。过高设置可能造成色彩失真、高光裁切或细节丢失。

## 安装与使用

1. 在已验证的 Q5 上安装 APK。
2. 在 Magisk 弹窗中允许应用获取 Root 权限。
3. 首次使用悬浮面板时，在系统设置中授予悬浮窗权限。
4. 拖动滑杆实时校色，或点击右侧数值输入绝对值；`√` 保存并应用，`×` 取消。
5. 点击爱心应用固定推荐设置。普通滑杆调整不会修改该预设。

## 设备实现说明

面板控制通过 Q5 的 `/sys/class/display/dispc0/PQ/` 接口写入 CM、HSV、Gamma 和 EPF 数据，并检查模块状态。已在目标设备验证的 payload 长度分别为 CM 24 字节、HSV 1440 字节、Gamma 1536 字节、EPF 14 字节。

应用根据随包厂商 XML 构造调节数据，不读取设备的二进制 Gamma 表；在此固件上读取该表会触发内核 `dpu_enhance_get` 错误。系统饱和度通过 Root 下的 SurfaceFlinger transaction `1022` 调整，与面板 DPU 控制分开。Paper7 和 TCL S8 的 MediaTek PQ 接口没有移植到 Q5。

## 从源码构建

需要 Windows PowerShell、JDK 17、Android SDK Platform 35、Build Tools 35.0.0，以及 `~/.android/debug.keystore`。厂商调色 XML 已包含在 `assets/pq/` 中。

```powershell
.\build.ps1 -Sdk "C:\Android\Sdk" -Jdk "C:\Java\jdk-17"
```

构建脚本会生成并签名 `build/Q5-screen-tuner-0.1.51.apk`。连接设备后可使用：

```powershell
adb install -r .\build\Q5-screen-tuner-0.1.51.apk
```
