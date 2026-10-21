# OPIXray-v2 X 光违禁品检测：统一基准与 DOAM 实验

这是一个基于 OPIXray-v2 的团队课程项目，比较 Faster R-CNN、YOLO26 与 RT-DETR，并评估 De-occlusion Attention Module（DOAM）。公开仓库中的 notebook 已清除单元格输出和数据集图片，并移除队友姓名、学号等个人信息。

> **个人贡献说明：** YOLO26 pipeline 归属为本人负责部分。Faster R-CNN、RT-DETR、DOAM 设计及统一评估属于团队项目的其他协作部分。此仓库展示的是团队成果背景及个人负责的 YOLO26 部分，不声称独立完成全部模型与实验。

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


- 数据集官方页面 / 条款：[OPIXray 官方仓库](https://github.com/OPIXray-author/OPIXray)


