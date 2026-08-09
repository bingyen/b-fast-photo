# B-FAST PHOTO EDIT STUDIO

<p align="center">
  面向活动摄影与大批量交付场景的 Windows 本地 AI 修图工作站
</p>

<p align="center">
  <img alt="Platform" src="https://img.shields.io/badge/platform-Windows-0078D6">
  <img alt="Python" src="https://img.shields.io/badge/Python-3.11%2B-3776AB">
  <img alt="Version" src="https://img.shields.io/badge/version-v47-45D6A6">
</p>

B-FAST PHOTO EDIT STUDIO 将 AI 自动修图、人脸分析、LED 屏幕处理、天空保护、RAW 解码、LUT、美颜、降噪、裁切与批量导出整合在一个桌面工作流程中。软件在本机处理照片，适合婚礼、颁奖礼、企业活动、舞台演出与人像批量交付。

> 当前仓库没有根目录 `LICENSE`，因此 B-FAST 自有源代码并未自动取得开源许可。第三方组件仍分别遵循其原始许可证；详见 [开源组件与许可证清单](开源组件与许可证清单.md)。

## 主要功能

### AI 与智能分析

- PPR10K 三位修图专家 A／B／C，以及 GLC 组照一致性模型。
- 全图分析与人脸优先两种调色依据。
- YuNet 人脸检测、列表人脸状态、人工标记与 LED 屏幕可疑人脸过滤。
- LED 屏幕高光保护，降低舞台大屏对人物曝光的干扰。
- AI 天空分割：先保存 AI 修图前的天空细节，再在天空 MASK 内融合处理。
- 自动校正角度：建筑线条、文字基线、智能与手动角度模式，只旋转及安全裁切，不拉伸透视。

### 调色与画质

- 一键白平衡，并可调整混合强度。
- 内置 3 款 `.cube` LUT，也支持加载本地 3D LUT。
- 肌肤亮白、磨皮与画质增强。
- 曝光、对比度、饱和度、色温、阴影、高光、暗角与颗粒。
- FFDNet 彩色图像降噪。
- 天空颜色、亮度、对比度、AI 强度及 MASK 羽化。
- 所有关键效果同时支持全局批量设置与“仅修此图”。

### 工作流程

- 导入单张照片或整个文件夹；RAW 与同名 JPEG 同时存在时可明确选择导入类型。
- 支持常见相机 RAW，包括 CR2、CR3、ARW、NEF、NRW、RAF、RW2、ORF、DNG 等。
- 左侧列表支持搜索、多选、缩略图、评分、人脸状态与排序。
- 可选择性复制调色设置，并批量粘贴到多张照片；不会复制人脸框或识别结果。
- 支持撤销（`Ctrl+Z`）、项目保存／打开，以及可选的逐张自动保存。
- 中央画布支持滚轮缩放、拖动、画质／速度优先、左右对比、滑动分割对比与快速查看原图。
- 直接在预览区裁切，支持原图比例、自由比例、1:1、3:2、4:5、16:9、9:16 与角度调整；按 `Enter` 应用。

### 导出

- 导出当前照片、全部照片，或按 1–5 星评分筛选。
- 每次导出都会询问保存位置。
- 默认保留原文件名；目标目录存在同名文件时自动使用 `_edited`，不会覆盖原文件。
- 根据电脑配置自动决定并发数量，并显示当前张数、总数、进度与预计剩余时间。
- 预览可以降采样加速，正式导出始终走完整分辨率处理链。

## 支持格式

| 类型 | 格式 |
| --- | --- |
| 常规图片 | JPG、JPEG、PNG、TIFF、WEBP、BMP |
| 主流 RAW | CR2、CR3、CRW、ARW、SR2、SRF、NEF、NRW、RAF、RW2、ORF、DNG、PEF、SRW、X3F 等 LibRaw 支持格式 |
| 3D LUT | `.cube` |
| 项目文件 | `.bfastproj` |

## 直接运行

Windows 用户可从 GitHub Releases 下载打包版本，并运行 `B-FAST PHOTO EDIT STUDIO v47.exe`。单文件 EXE 首次启动时需要解压运行组件，因此可能比后续启动稍慢。

> 发布者需要先把 EXE 上传到 GitHub Releases，README 中才会出现可下载的安装包。本仓库目前的本地构建产物位于 `dist/`。

## 从源码运行

建议使用 Python 3.11 或更新版本：

```powershell
python -m venv .venv
.venv\Scripts\python.exe -m pip install --upgrade pip
.venv\Scripts\python.exe -m pip install -r requirements.txt
.venv\Scripts\python.exe -m pip install ncnn
.venv\Scripts\pythonw.exe main.py
```

部分功能还需要合法取得并放入对应目录的模型与 SDK 资源：

- `models/pretrained_models/`：PPR10K 权重。
- `assets/face_detection/`：YuNet 人脸检测模型。
- `assets/white_balance/`：Deep White-Balance 权重。
- `assets/denoise/`：FFDNet 权重。
- `assets/sky_segmentation/`：天空分割模型。
- `pixelfree_native/`：PixelFree SDK、桥接 DLL、模型及授权资源。

缺少上述资源时，对应功能不可用。请勿把没有再分发权、商业授权或包含私密许可证的文件上传到公开仓库。

## 构建 Windows EXE

```powershell
.venv\Scripts\python.exe -m pip install pyinstaller
build_exe_v47.bat
```

构建结果：

```text
dist\B-FAST PHOTO EDIT STUDIO v47.exe
```

## 测试

```powershell
.venv\Scripts\python.exe -m unittest discover -s tests -v
```

## 项目结构

```text
.
├─ main.py                     # 程序入口
├─ pprstudio/                  # UI 与处理管线
├─ assets/                     # 模型、LUT、品牌资源与第三方声明
├─ models/                     # PPR10K 权重
├─ pixelfree_native/           # PixelFree 原生桥接与授权资源
├─ tests/                      # 自动化测试
├─ requirements.txt
└─ B-FAST PHOTO EDIT STUDIO v47.spec
```

## 第三方项目与许可证

本项目使用或整合 PPR10K、Deep White-Balance、FFDNet／KAIR、YuNet、Sky-Segmentation-and-Post-processing、rawpy／LibRaw、OpenCV、PyTorch、NCNN、NumPy、Pillow 与 PyInstaller 等组件。

完整来源、版本、许可证与发布注意事项请查看 [开源组件与许可证清单](开源组件与许可证清单.md)。其中：

- PPR10K 仓库代码为 Apache-2.0，但其数据集页面明确规定数据文件仅限非商业研究；随数据下载的预训练权重应按该限制谨慎处理。
- Deep White-Balance 集成及权重为 CC BY-NC-SA 4.0，仅限非商业用途。
- PixelFree、授权文件和内置 LUT 不属于本项目的开源组件，是否可以公开或再分发取决于各自授权。

## 致谢与引用

如果在研究中使用 PPR10K，请引用：

```bibtex
@inproceedings{jie2021PPR10K,
  title={PPR10K: A Large-Scale Portrait Photo Retouching Dataset with Human-Region Mask and Group-Level Consistency},
  author={Liang, Jie and Zeng, Hui and Cui, Miaomiao and Xie, Xuansong and Zhang, Lei},
  booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition},
  year={2021}
}
```

## 发布前检查

- 为 B-FAST 自有代码选择并添加根目录 `LICENSE`，或明确声明保留所有权利。
- 检查 PPR10K 权重、Deep White-Balance 权重、PixelFree SDK／授权文件和内置 LUT 的公开发布权限。
- 不要提交个人照片、项目文件、导出照片、密钥或许可证文件。
- 建议通过 GitHub Releases 发布 EXE，不要把大型二进制直接加入 Git 历史。

