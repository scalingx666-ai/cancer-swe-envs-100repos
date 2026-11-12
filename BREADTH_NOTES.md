# 100-Repo Breadth Edition — 补充说明

本数据集强调**仓库覆盖广度**：精选 100 个高 star / 高价值 / 近两年活跃的 cancer 与
肿瘤基因组学开源代码库，每个 repo 最多 3 个 `pr_diff` 任务。

- 目标 repo：**100**
- 实际产出任务的 repo：**90**
- 任务总数：**210**（每 repo ≤ 3，取 diff 规模最大的 3 个）
- 静态校验：`validate --deep --oracle` → 210/210 通过

## 与 v1 的关系

v1 深度版 `scalingX/cancer-swe-envs`（14 repo / 135 task，单 repo 可多达 21 task）保留不动。
本版是独立的广度版，因「每 repo ≤3」约束无法与 v1 合并为同一数据集，故新建独立发布。

## 筛选口径

GitHub 多组查询（topic:cancer / oncology / cancer-genomics / radiomics / TCGA / neoantigen /
single-cell / variant calling / immunotherapy 等）+ 具名高价值工具清单，按 star 与 push
活跃度排序，剔除 awesome-list、教程、notebook、网页组件等噪声（如 canvas-nest.js、
TSBattery）。覆盖 cBioPortal、GATK、Picard、htslib/samtools/bcftools、nf-core 系列、
pVACtools、maftools、Seurat/Scanpy、STAR/BWA/minimap2、MONAI/nnUNet、DeepSomatic、
DepMap、pyradiomics/SlicerRadiomics、pySCENIC/ArchR、SigProfiler 等。

## 10 个零产出 repo

以下 repo 在放宽过滤（max_files=25、min_loc=2、limit=15）后仍无合适的 merged PR，
多为论文复现代码 / 小项目 / 无 PR：

- MrGiovanni/SyntheticTumors
- MohamedAliHabib/Brain-Tumor-Detection
- google/deepsomatic
- XiaojianDing/TCBB-SIMKF
- MrGiovanni/DiffTumor
- Ahthe/Brain_Tumor_Prediction-ML
- ShujiaHuang/ilus
- mdozmorov/TCGAsurvival
- Swarbricklab-code/BrCa_cell_atlas
- CHB-learner/NeoBert

## 判分与边界

判分为 agent diff 与 merged diff 的 6 组件加权相似度（含 llm_judge 0.50）。
生成阶段不需要 Docker/LLM；grading 需 1 个 LLM judge key，否则 verifier 写 `ungraded`。
任务为生成阶段产物，evaluation 标签为 `unverified`；deep validate 仅为静态结构校验，
不等于在 Harbor 里用真实 agent 跑出过 reward。

完整 100 repo 清单见 `repo_list.txt`，逐 repo 任务数见 `dataset_stats.json`。
