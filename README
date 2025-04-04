# Task
**Structure <---model----> MS2**

##### Structure:
* **SMILES**: Commonly used, unfixed (one compound has multiple correct SMILES). [ref](https://en.wikipedia.org/wiki/Simplified_Molecular_Input_Line_Entry_System)
* **sdf**: Coordinates file, can be downloaded from PubChem (or other DB), may also include some other information (like compound name, weight, SMILES ....), therefore some rare case, need extract information first
* **InChIKey**: Fixed, used for find the unique compounds. we only care 2D structure, so if first 14 characters of InChIKey are same, they are same compounds. [ref](https://en.wikipedia.org/wiki/International_Chemical_Identifier#InChIKey)

**SMILES, InChIKey, sdf之间可以通过[RDKit](https://www.rdkit.org/docs/Install.html)互相转化**

##### MS2
`[[mz1, int1], [mz2, int2], [mz3, int3], ...]`

##### 模型类型：
**Forward**：Structure --> MS2(CCC=CCC --> MS2)
**Reverse**: MS2 --> Structure

**Forward和Reverse各跑两个models**


# Workflow
For each model:
1. 确认环境需求（CUDA，Python版本）不好运行的就先跳过
2. 确认任务类型: Forward or Reverse（原始论文 + ChatGPT，一定可以属于其中之一，但是可能需要一些额外的格式转换）
3. 创建环境
4. 使用测试运行模型（单个分子）
5. 批量运行的脚本（多个分子）
6. 结果应该是一个包含SMILES和MS2的table
7. 计算预测结果的准确性

**step 5，6，7暂时不需要**


# Models
### NEIMS
* 代码：https://github.com/brain-research/deep-molecular-massspec
* 论文：https://doi.org/10.1021/acscentsci.9b00085
* task: **Structure --> Fingerprints --> MS2**
* 使用起来最简单的
```
   [Additive ECFP (4096-d)]
             │
      [Shared MLP Layers]
             │
   ┌─────────┴─────────┐
   │                   │
[Forward Output]   [Reverse Output]
   │                   │
   └───────┬───────┬────┘
           │   [Gating]
           ▼
   [Weighted Fusion]
           │
   [Predicted Spectrum]

```


### MIST
* 需要CUDA
* MS2 + formula (opt.) --> fingerprint + PubChem中匹配的分子（structure）
* [code](https://github.com/samgoldman97/mist)
* [MIST-CF](https://pubs.acs.org/doi/abs/10.1021/acs.jcim.3c01082)
```
              ┌────────────────────────┐
              │   MS/MS Spectrum       │
              │ (m/z & intensity pairs)│
              └────────┬───────────────┘
                       ↓
          ┌──────────────────────────────┐
          │ Spectrum Transformer Encoder │
          └──────────────────────────────┘
                       ↓
      ┌────────────────────────────────────────┐
      │   化学先验注入模块：Inductive Biases   │
      │ （Neutral loss & 子结构提示）         │
      └────────────────────────────────────────┘
                       ↓
         ┌────────────────────────────┐
         │ Formula Transformer Module │ ← 输入：分子式
         └────────────────────────────┘
                       ↓
             ┌────────────────────┐
             │ Fingerprint Head   │ → 输出分子指纹向量
             └────────────────────┘

```



### MSNovelist
* task: MS2 --> Fingerprint (CSI:FingerID预测) + Molecular Formula （SIRIUS预测） --> SMILES



### MSBERT
* MS2 --> spectrum embedding vector (后续需要结构匹配)


### CSI:FingerID
* MS2 --> Fingerprint + Ranked Molecular Structures

### CMSSP
* Structure <--> MS2

### PPGB-MS2
* task: 前体离子图（由原始分子结构生成) + 产物离子图（通过分子碎裂反应得到）+ 碎裂边（连接前体和产物中同源原子的边，表示碎裂路径）--> 每个peak的intensity


### MassFormer
* Molecular graph --> MS2
* Model: 构建其碎片树 --> 计算其与训练集中样本的核相似度 --> 由训练好的 SVM 模型预测其指纹（带概率值）

### ICEBERG
* Molecular graph --> MS2

### GRAFF-MS
* Molecular graph --> MS2


### DeepCDM
* Structure --> MS2 (衍生化的)

### FraGNNet 
* 没开源，跑不了
* Molecular graph --> MS2

### SCARF
* Molecular graph --> MS2


### CFM-ID
* 这个应该比较简单
* Molecular graph --> MS2
