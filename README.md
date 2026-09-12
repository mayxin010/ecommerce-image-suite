# ecommerce-image-suite

面向电商商品套图生产的 AI 工具。输入同一商品的 1～5 张图片，完成商品视觉分析、卖点提炼、Prompt 构建和电商套图生成。

当前代码库提供 MVP 核心生成能力，后续将在 `dev` 分支上逐步改造成包含上传、异步任务、进度展示、失败重试和结果下载的 Web 系统。

## 核心能力

1. 将商品原图分析为结构化商品信息。
2. 根据商品特征生成中英文卖点。
3. 调用图像生成供应商批量生成电商套图。
4. 支持商品参考图、模特参考图、视觉模板和中英文图片文案。

支持的图片类型：

| 类型 | 说明 |
| --- | --- |
| 白底主图 | 商品全貌和标准白底展示 |
| 核心卖点图 | 集中呈现多个核心卖点 |
| 卖点图 | 深入呈现单项卖点 |
| 材质图 | 展示面料、纹理和工艺细节 |
| 场景展示图 | 将商品置于使用场景中 |
| 模特展示图 | 服装商品的上身效果 |
| 多场景拼图 | 多个使用场景组合展示 |
| 电商详情图 | 详情页长图物料 |
| 三角度拼图 | 正面、侧面和背面组合展示 |

## 目录结构

| 路径 | 说明 |
| --- | --- |
| `SKILL.md` | 现有 Agent 工作流和业务规则 |
| `scripts/analyze.py` | 商品图片视觉分析 |
| `scripts/generate.py` | 电商套图生成核心脚本 |
| `scripts/check_providers.py` | 图像供应商配置检测 |
| `references/` | Prompt、平台、图型和供应商参考资料 |
| `assets/models.json` | 内置模特元数据 |
| `assets/models/` | 内置模特参考图片 |

## 环境要求

- Python 3.8+
- `openai`
- `requests`
- 至少一个图像生成供应商的 API Key

支持的供应商：

| 供应商 | 环境变量 |
| --- | --- |
| 阿里云通义 | `DASHSCOPE_API_KEY` |
| 字节豆包 | `ARK_API_KEY` |
| OpenAI | `OPENAI_API_KEY` |
| Google Gemini | `GEMINI_API_KEY` |
| Stability AI | `STABILITY_API_KEY` |

检查供应商配置：

```bash
python scripts/check_providers.py
```

## 基础使用

分析商品图片：

```bash
python scripts/analyze.py ./product.jpg --output ./output/product.json
```

根据分析结果生成套图：

```bash
python scripts/generate.py \
  --product '{"product_description_for_prompt":"white cotton T-shirt","selling_points":[]}' \
  --provider tongyi \
  --product-image ./product.jpg \
  --types white_bg,key_features,material,lifestyle,model \
  --output-dir ./output
```

## 当前开发方向

MVP 将围绕以下闭环开发：

```text
上传 1～5 张同款商品图
→ 图片检查与商品分析
→ 自动生成套图任务
→ 查看生成进度
→ 单张失败重试
→ 预览和打包下载
```

## License

详见 [LICENSE](./LICENSE)。
