# UO Image Prompt Reference

显式调用的服装模特单步生图提示词与参考图顺序目录。它用于帮助 Codex 选择一个原子任务模板，再根据实际图像改写。

## 基础信息

| 项目 | 内容 |
| --- | --- |
| 名称 | `uo-image-prompt-reference` |
| 类型 | 显式调用的 Codex 参考 Skill |
| 创建时间 | 2026-09-10（源目录时间） |
| 公开发布整理 | 2026-09-14 |
| 版本 | `2026.09.14` |
| 入口 | [SKILL.md](SKILL.md) |
| 提示词目录 | [references/prompt-catalog.md](references/prompt-catalog.md) |
| 状态 | 可用，默认禁止隐式调用 |

## 目标与适用场景

适用于缺失模特角度、动作修改、试穿、服装换色、同人替换和去除手臂纹身等单步编辑。用户必须明确调用 `$uo-image-prompt-reference`，它不会自动介入其他服装任务。

不适用于自动规划整个多步工作流、批量读取 Excel 或直接提交 API 生成。复合任务的顺序仍由用户或上层工作流程决定。

## 严格输入格式

- 提供与任务相关的模特图、平铺图、背面图或身份参考图。
- 图像应足够清晰，便于判断人物身份、服装轮廓、颜色、材质、细节、动作和构图。
- Figure 1、Figure 2 及后续图片的顺序是位置合同，必须按选中条目的 `References` 顺序传入；不能根据文件名或上传时间重新排序。
- 模板中的颜色、服装、配饰、背景和动作只是示例；必须按可见证据改写。
- 没有匹配条目时，只有在用户要求下才可依据同样原则撰写新的简短单步提示词。

此 Skill 不规定固定图片格式、像素尺寸或目录结构；实际限制由所用的图像模型与上层流程决定。

### 各任务的参考图顺序

| Task ID | Figure 顺序 |
| --- | --- |
| `missing-side-view` | Figure 1：正面或接近正面的模特图 |
| `missing-front-view` | Figure 1：其他角度但产品已经准确的模特图 |
| `missing-back-view` | Figure 1：正面或接近正面的产品准确模特图；Figure 2：商品背面图 |
| `missing-back-view-gpt-simple` / `missing-back-view-gemini-simple` | Figure 1：产品准确的正面模特图；Figure 2：正面平铺图；Figure 3：背面平铺图 |
| `adding-pose` / `adding-pose-arms-down` | Figure 1：需要更改动作的模特图 |
| `product-wear` | Figure 1：模特图；Figure 2：与模特可见服装面对应的正确商品图 |
| `change-color` | Figure 1：待换色模特图；Figure 2：同款、正确颜色且可见面匹配的商品图 |
| `same-person` | Figure 1：需要保留动作和构图的目标图；Figure 2：身份参考模特图 |
| `remove-arm-tattoo` | Figure 1：包含手臂纹身的模特图 |

`missing-back-view` 的两图合同与 processor compatibility 的三图合同不能混用；必须先根据调用方工作流选择对应 Task ID。完整提示词和输出角色见 [references/prompt-catalog.md](references/prompt-catalog.md)。

## 环境与依赖

仅需要支持本地 Skill 和图像查看的 Codex 环境；本目录不含可执行脚本、第三方 Python 依赖、模型密钥、插件或硬件要求。真实生图工具及模型由调用者提供。

## 使用方式

将整个目录放入 Codex `skills` 目录，然后明确调用：

```text
使用 $uo-image-prompt-reference 为当前服装模特生图任务选择参考图并撰写单步提示词。
```

交付物是按图像内容改写的单步提示词、参考图顺序和预期输出角色；Skill 本身不保存生成图片。
