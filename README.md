# OPIXray-v2 X 光违禁品检测：统一基准与 DOAM 实验

这是一个基于 OPIXray-v2 的团队课程项目，比较 Faster R-CNN、YOLO26 与 RT-DETR，并评估 De-occlusion Attention Module（DOAM）。公开仓库中的 notebook 已清除单元格输出和数据集图片，并移除队友姓名、学号等个人信息。

> **个人贡献说明：** 原始 notebook 将 YOLO26 pipeline 归属为本人负责部分。Faster R-CNN、RT-DETR、DOAM 设计及统一评估属于团队项目的其他协作部分。此仓库展示的是团队成果背景及个人负责的 YOLO26 部分，不声称独立完成全部模型与实验。

## 项目内容

- 对比 Faster R-CNN、YOLO26n 与 RT-DETR-L。
- 通过固定随机种子和统一数据划分，生成 COCO 与 YOLO 格式；官方测试集保持不变，并从官方训练集划出验证集。
- 在统一 COCO evaluator 下汇总 mAP、Recall、参数量与推理速度。
- 评估 DOAM 输入前端，并分析遮挡等级 OL1、OL2、OL3 下的表现。
- 提供 EDA、误差分析、训练/验证曲线、按类别结果与可视化分析代码。

## 技术栈

Python · PyTorch · Ultralytics · torchvision · COCO / `pycocotools` · pandas · NumPy · Matplotlib · Pillow

## notebook 中记录的结果

以下数字来自团队 notebook 的讨论部分；本作品集包不含模型权重、原始数据和已执行输出，因此尚未在公开包中独立重跑验证。

| 实验记录 | notebook 中的结果 |
|---|---:|
| RT-DETR-L mAP@0.5 | 89.55% |
| YOLO26n mAP@0.5:0.95 / 参数量 / 推理速度 | 0.388 / 2.38M / 约 101 FPS |
| Faster R-CNN mAP@0.5 | 85.11% |
| YOLO26 + DOAM 总体 mAP@0.5 变化 | +0.63 个百分点 |
| YOLO26 + DOAM 在 OL3 的 mAP@0.5 变化 | +1.99 个百分点 |
| DOAM 在 RT-DETR / Faster R-CNN 的总体变化 | -1.46 / -0.53 个百分点 |

不同检测器使用的训练框架、优化器、增强和批量大小并不完全相同，所以跨架构比较不应解释为严格控制的架构因果比较。团队 notebook 记录 YOLO26 的 DOAM 增益集中在高遮挡样本；此结果仅说明该数据集与该实验配置下的观察，不代表部署效果保证。单随机种子实验也没有量化种子间波动。

## 文件

- `OPIXray_Unified_Detection.ipynb`：经过公开化处理的统一实验 notebook。代码和 markdown 已保留；单元格执行结果、内嵌 X 光图片、队友姓名/学号已移除。
- `.gitignore`：排除数据集、模型权重、输出文件、环境和缓存。

## 数据与复现

notebook 默认从项目工作目录读取以下原始目录，并在需要时生成数据格式和划分文件：

```text
OPIXray/
  train/
    train_image/
    train_annotation/
    train_knife.txt
  test/
    test_image/
    test_annotation/
    test_knife.txt
```

Notebook 还会在项目目录下生成 `coco_format/`、`yolo_dataset/`、`outputs/` 和 `train_ids.txt`、`val_ids.txt`、`test_ids.txt`。这些均由 `.gitignore` 排除，不应提交到公开仓库。应在 notebook 所在项目根目录作为工作目录运行。

完整训练依赖原始数据集、相应预训练权重及 Python 环境。当前包没有锁定依赖版本，也不含数据和 checkpoint；`RUN_TRAINING` 开关默认关闭，未准备模型 checkpoint 时无法直接完成后续评估。请先按实际环境补充依赖版本、官方数据集获取链接、权重来源/许可和经过实跑确认的操作步骤。这里不提供未经验证的安装或训练命令。

- 数据集官方页面 / 条款：[OPIXray 官方仓库](https://github.com/OPIXray-author/OPIXray)。官方仓库注明图像与对应标注仅供学术用途、不可用于商业用途；不要在个人仓库中再分发数据集文件。
- 实际环境版本：`[待补充：Python、PyTorch、CUDA、Ultralytics、torchvision、pycocotools]`
- 可公开的完整复现流程：`[待补充：仅加入实测命令与结果]`

## 图片、数据与许可

OPIXray 官方仓库注明其图像与对应标注仅能用于学术用途，不可用于商业用途。公开作品集时仅链接官方数据来源，不附数据文件。

此仓库不包含 OPIXray 图像、标注、训练权重或 notebook 输出图片。公开前请核对 OPIXray-v2 的来源条款、第三方模型权重许可及所有依赖许可；不要将数据或 checkpoint 提交到仓库，除非其条款明确允许再分发。尚未确认代码发布权和依赖兼容性前，不附加开源许可证文件。请另外确认学校/团队项目的公开规则。
