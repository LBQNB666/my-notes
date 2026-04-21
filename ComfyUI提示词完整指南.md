# 🎨 ComfyUI 提示词精通指南

> 专门针对叶秋版 ComfyUI (ComfyUI-aki-v3) 的提示词教程

## 📂 目录

1. [你的配置分析](#你的配置分析)
2. [提示词基础语法](#提示词基础语法)
3. [权重调整技巧](#权重调整技巧)
4. [模型专用提示词](#模型专用提示词)
5. [LoRA 使用技巧](#lora-使用技巧)
6. [负面提示词](#负面提示词)
7. [实用工作流](#实用工作流)

---

## 🔧 你的配置分析

### 叶秋版 ComfyUI-aki-v3 配置

```
📦 模型位置: D:\ComfyUI-aki-v3\ComfyUI-aki-v3\ComfyUI\models\
```

#### Checkpoints (大模型)

| 模型 | 类型 | 用途 |
|------|------|------|
| `animagine-xl-3.1.safetensors` | SDXL 动漫 | 高质量动漫图像 |
| `anything-v5.safetensors` | 动漫 | 通用动漫风格 |
| `GuoFeng3.4.safetensors` | 国风 | 中国古风 |
| `majicmixRealistic_v7.safetensors` | 写实 | 写实人像 |
| `v1-5-pruned-emaonly.safetensors` | SD 1.5 | 基础模型 |

#### LoRA 模型

| LoRA | 类型 | 用途 |
|------|------|------|
| `麦橘超美_majicFlus_v1.safetensors` | 美颜 | 增强人像美感 |
| `YFilter_PortraitEnhance_V1.safetensors` | 人像增强 | 提升人像质量 |
| `极致人物修复_肤质修复+手部修复_V2.0.safetensors` | 修复 | 皮肤和手部修复 |
| `Canopus-LoRA-Flux-UltraRealism.safetensors` | 写实增强 | 超写实风格 |
| `皮肤细节增强.safetensors` | 细节 | 皮肤质感 |

#### 已安装插件

| 插件 | 功能 |
|------|------|
| ComfyUI-Manager | 管理器，简化安装 |
| ComfyUI-Impact-Pack | 增强节点 |
| ComfyUI-GGUF | GGUF 模型支持 |
| ComfyUI-nunchaku | 性能优化 |
| ComfyUI-KJNodes | 实用工具节点 |
| ComfyUI-IPAdapter_plus | IPAdapter 增强 |
| ComfyUI-RMBG | 背景移除 |

---

## 📝 提示词基础语法

### 1. 关键词分隔

✅ **正确格式** - 用逗号分隔关键词
```
1girl, solo, long hair, blue eyes, smile, white dress, outdoors
```

❌ **错误格式** - 不要写完整句子
```
A girl with long hair and blue eyes is smiling wearing white dress outside
```

### 2. 元素顺序规则

**越靠前的元素权重越高！**

```
# 正确：强调哪个就放前面
1girl, (blue eyes:1.5), long hair, smile

# 错误：分散且不明确
1girl, smile, long hair, blue eyes
```

### 3. 常用基础标签

#### 人物
```
1girl          # 一个女孩
1boy           # 一个男孩
solo           # 单独
multiple girls # 多个女孩
```

#### 视角/构图
```
solo, upper body      # 上半身
solo, full body       # 全身
solo, close-up        # 特写
from above            # 俯视
from below            # 仰视
from side             # 侧面
```

#### 场景
```
outdoors           # 户外
indoors           # 室内
cityscape         # 城市
landscape         # 风景
studio            # 工作室
```

#### 时间/光线
```
daytime          # 白天
night            # 夜晚
sunset           # 日落
sunrise          # 日出
soft lighting    # 柔和光线
dramatic lighting # 戏剧光线
```

---

## ⚖️ 权重调整技巧

### 括号语法

| 语法 | 效果 |
|------|------|
| `(word)` | 1.1 倍权重 |
| `(word:1.5)` | 1.5 倍权重 |
| `((word))` | 1.21 倍权重 (1.1×1.1) |
| `[word]` | 0.9 倍权重 |
| `[word:0.5]` | 0.5 倍权重 |

### 示例

```python
# 强调眼睛颜色
1girl, (blue eyes:1.3), long hair, smile

# 弱化背景
1girl, solo, ([outdoors]:0.5), garden

# 双重强调
1girl, ((masterpiece)), best quality, ultra detailed
```

### 渐进式强调

```python
# 普通 → 强调 → 极度强调
1girl, smile
1girl, (smile), happy
1girl, ((smile)), ((happy)), best expression
```

---

## 🎯 模型专用提示词

### Animagine XL 3.1 (动漫)

#### 推荐参数
- **采样器**: Euler, Euler a
- **Steps**: 28-35
- **CFG Scale**: 7-8
- **Clip Skip**: 2

#### 提示词模板
```
# 正面
1girl, arima kana, oshi no ko, solo, upper body, smile, looking at viewer, outdoors, night, anime style, best quality, masterpiece

# 负面
nsfw, lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, artist name
```

#### 特殊标签 (Animagine XL 3.1 专有)
```
rating:safe         # 安全评级
source_anime        # 来源动画
source_furry       # 来源福瑞
source_pony         # 来源小马
```

### Majicmix Realistic v7 (写实)

#### 推荐参数
- **采样器**: Euler a, DPM++2M/SDE Karras
- **Steps**: 20-40
- **CFG**: 7-8 (建议开启 Dynamic Thresholding 1-20)
- **Clip Skip**: 2

#### 提示词模板
```python
# 正面
1girl, beautiful Asian woman, sitting on a cozy couch, crossing legs, soft lighting, indoor, modern style, best quality, masterpiece, ultra high res, photorealistic

# 负面
(worst quality:2), (low quality:2), (normal quality:2), lowres, watermark
```

#### 写实风格关键词
```
photorealistic           # 照片级真实
ultra realistic          # 超写实
detailed skin            # 皮肤细节
natural lighting         # 自然光
portrait photography     # 人像摄影
8k, uhd, dslr           # 高清画质
soft focus              # 柔焦
bokeh                   # 背景虚化
```

### 国风 (GuoFeng3.4)

#### 提示词模板
```python
# 正面
1girl, Chinese traditional clothing, hanfu, elegant posture, traditional Chinese architecture, garden, spring, cherry blossoms, Chinese painting style, masterpiece

# 负面
nsfw, lowres, bad anatomy, western clothing, modern buildings
```

---

## 🔮 LoRA 使用技巧

### 基础用法

```
1. 加载 Checkpoint 大模型
2. 连接 LoRA Loader 节点
3. 选择 LoRA 文件
4. 设置 Strength (强度)
```

### LoRA Strength 调整

| LoRA 类型 | 推荐强度 | 说明 |
|-----------|----------|------|
| 风格 LoRA | 0.7-1.0 | 强烈风格效果 |
| 美颜 LoRA | 0.3-0.6 | 自然美颜 |
| 细节增强 | 0.2-0.4 | 微妙增强 |
| 修复 LoRA | 0.5-0.8 | 修复效果 |

### 你的 LoRA 组合建议

#### 动漫风格
```
Checkpoint: animagine-xl-3.1
LoRA: (不需要额外 LoRA，模型本身很强)
提示词: 1girl, solo, anime style, best quality
```

#### 写实人像
```
Checkpoint: majicmixRealistic_v7
LoRA: 麦橘超美_majicFlus_v1.safetensors (强度 0.5)
LoRA: 极致人物修复_肤质修复+手部修复_V2.0.safetensors (强度# 续：ComfyUI 提示词精通指南

## LoRA 组合示例

#### 写实人像完整配置
```
Checkpoint: majicmixRealistic_v7
LoRA: 麦橘超美_majicFlus_v1.safetensors (强度 0.5)
LoRA: 极致人物修复_肤质修复+手部修复_V2.0.safetensors (强度 0.5)

提示词: 1girl, beautiful woman, soft lighting, best quality, masterpiece
```

#### 皮肤质感增强
```
Checkpoint: majicmixRealistic_v7
LoRA: 皮肤细节增强.safetensors (强度 0.3)
LoRA: YFilter_PortraitEnhance_V1.safetensors (强度 0.4)

提示词: 1girl, detailed skin texture, natural lighting, portrait photography
```

---

## 🚫 负面提示词

### 通用负面提示词

```python
# 基础版
lowres, bad anatomy, bad hands, text, error, missing fingers, 
extra digit, fewer digits, cropped, worst quality, low quality

# 进阶版
nsfw, lowres, bad anatomy, bad hands, text, error, missing fingers, 
extra digit, fewer digits, cropped, worst quality, low quality, 
normal quality, jpeg artifacts, signature, watermark, username, 
blurry, artist name, bad proportions, missing arms, missing legs, 
extra arms, extra legs, fused fingers, too many fingers, 
mutated hands, malformed limbs
```

### 写实专用负面提示词

```python
# Majicmix Realistic 专用
(worst quality:2), (low quality:2), (normal quality:2), 
lowres, watermark, signature, jpeg artifacts, blurry, 
incomplete hands, bad proportions, gross proportions, 
malformed limbs, missing arms, missing legs, extra arms, 
extra legs, fused fingers, too many fingers, bad hands, 
bad fingers, mutated hands and fingers, bad_prompt_version2
```

---

## ⚙️ 实用工作流

### 工作流 1: 快速动漫人像

```
LoadCheckpoint (animagine-xl-3.1) → CLIPTextEncode → KSampler → VAEDecode → SaveImage
```

**提示词:**
```
1girl, solo, upper body, beautiful detailed eyes, long flowing hair, 
smile, looking at viewer, best quality, masterpiece, anime style
```

**参数:** Steps 28, CFG 7-8, Sampler Euler a

---

### 工作流 2: 写实人像 (带 LoRA)

```
LoadCheckpoint → LoadLora × 2 → CLIPTextEncode → KSampler → VAEDecode → SaveImage
```

**提示词:**
```
1girl, beautiful Asian woman, sitting, soft lighting, indoor, 
modern apartment, best quality, masterpiece, ultra high res, photorealistic
```

---

## 📊 采样器对比

| 采样器 | 速度 | 质量 | 适合风格 |
|--------|------|------|----------|
| Euler | 快 | 中 | 快速草图 |
| Euler a | 中 | 高 | 动漫/写实 |
| DPM++ 2M Karras | 中 | 高 | 精细细节 |

---

## 🔗 学习资源

- [Civitai](https://civitai.com/) - 模型和 LoRA
- [Hugging Face](https://huggingface.co/) - 模型下载
- [LiblibAI](https://www.liblib.art/) - 国内模型站
- B站: 秋葉 aaaki

---

**2026-04-21**: 创建叶秋版 ComfyUI 提示词指南
