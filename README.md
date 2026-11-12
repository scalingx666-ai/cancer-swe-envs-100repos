# Cancer SWE Environments — 100-Repo Breadth Edition

由 [Repo2RLEnv](https://github.com/huggingface/Repo2RLEnv) v0.9.6 的 `pr_diff` pipeline 生成的
SWE 类 RL 环境数据集：**精选 100 个高 star / 高价值 / 活跃的 cancer 与肿瘤基因组学开源代码库，
每个 repo 最多 3 个任务**，强调仓库覆盖广度而非单仓库深度。

（另有 v1 深度版 `scalingX/cancer-swe-envs`：14 个 repo / 135 个任务。）

## 任务语义

每个任务对应一个已合并的 Pull Request，回退到 base commit：agent 拿到剥离修复线索后的
PR 标题与描述，在代码库中编辑以复现该修复。判分用 agent diff 与 merged diff（oracle）
做 6 组件加权相似度（size_sanity 0.08 / file_targeting 0.12 / region_overlap 0.20 /
similarity 0.10 / llm_judge 0.50 / format_valid 守卫）。

生成阶段不需要 Docker 与 LLM；grading 需要 1 个 LLM judge key，否则 verifier 写 `ungraded`。

## 仓库筛选

GitHub `topic:cancer` / `topic:oncology` / cancer-genomics / radiomics / TCGA / neoantigen 等
多组查询 + 具名高价值工具清单，按 star 与近两年 push 活跃度排序，剔除 awesome-list、教程、
notebook、网页组件等噪声。覆盖 cBioPortal、GATK、Picard、htslib/samtools/bcftools、
nf-core/sarek、pVACtools、maftools、Seurat/Scanpy、STAR/BWA/minimap2、MONAI/nnUNet/QuPath、
DeepVariant/Strelka/DeepSomatic、DepMap、SlicerRadiomics、pySCENIC/ArchR 等。

## 使用

```bash
repo2rlenv validate <dataset-dir> --oracle
harbor run -p <dataset-dir> -a oracle --env docker
```

每任务含 `task.toml`（`content_hash`、`[metadata.repo2env]` 溯源、reward calibration、
evaluation 标签）、`instruction.md`、`environment/Dockerfile`（`python:3.12-slim` 自包含）、
`solution/patch.diff`+`solve.sh`、`tests/test.sh`+`verifier.py`+`oracle.patch`。

任务为生成阶段产物，evaluation 标签为 `unverified`；`validate --deep --oracle` 仅为静态结构校验。

## 许可

生成工具 Repo2RLEnv 为 Apache-2.0 AND MIT；各源仓库遵循其各自许可。
