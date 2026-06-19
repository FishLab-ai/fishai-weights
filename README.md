# FishAI Weights

> FishLab-ai 自研 AI 权重仓库

## 架构

- **领域块 + 联想激活**（非 MoE 专家）
- 每个领域块是独立的 `.bin` 文件，单文件 50-60MB
- 平均同时激活 Top-K = 5-6 个块
- CPU 也能跑，不需要 GPU
- **不使用 Git LFS**，直接 git push 即可

## 文件命名

```
weights/
├── common.bin              # 通用语言能力（必激活）
├── code.bin                # 编程领域
├── math.bin                # 数学推理
├── chinese.bin             # 中文优化
├── english.bin             # 英文优化
├── science.bin             # 科学常识
├── history.bin             # 历史人文
├── medical.bin             # 医学
├── law.bin                 # 法律
└── ...                     # 50-60 个领域块
```

## 加载策略

- 内存映射（mmap）按需加载
- LRU 缓存最近激活的领域块
- 激活由 query 的 embedding 相似度决定

## 用法

```bash
# clone 整个仓库（约 3GB，但 git 不会一次性下载历史）
git clone https://github.com/FishLab-ai/fishai-weights.git

# 或只拉取需要的块（partial clone）
git clone --filter=blob:none https://github.com/FishLab-ai/fishai-weights.git
cd fishai-weights
git sparse-checkout set weights/common.bin weights/code.bin
```

## License

FishLab-ai 内部使用，未授权禁止商用。
