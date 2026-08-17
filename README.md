# photo-to-gouache

> 转水粉绘画风格 — 上传一张照片（风景 / 城市 / 人物均可），一键转成水粉绘本插画风格，同时保留原图的意境与氛围。

## 这是什么

一套**照片转水粉（gouache）绘本插画**的风格方案。给定任意一张照片，配合支持「图生图（image-to-image）」的 AI 绘图工具，就能把它重绘成 **gouache（不透明水粉）绘本插画** 风格。

风格只迁移「画法」（大色块、可见笔触、简化造型、手绘肌理），**不搬运参考图的具体场景元素**，所以无论原图是阳光海滩还是阴雨小巷，都能保住原本的情绪和氛围。

## 特性
- 交互式流程：选尺寸 → 确认 → 生成（不会一上来就出图）
- 4 种尺寸可选：原图比例 / 1:1 / 3:4 / 4:3
- 风格只改画法，不改原图的情绪与主题

## 核心提示词（Prompt）

把下面这段提示词和你的照片一起提交给支持「图生图」的 AI 绘图工具即可：

> Transform this photo into a gouache painting illustration. Bold flat color blocks with expressive visible brush strokes, hand-painted gouache texture, simplified shapes, storybook picture-book style. Use colors drawn from the original scene's palette, applied in bold harmonious blocks with warm-cool contrast. No sharp black outlines—forms defined by color and brushwork. No photorealistic details, no photographic texture. Small stylized human figures if present, with posture but no facial details. Preserve the original photo's mood, lighting, and atmosphere. Keep the original composition and main subjects clearly recognizable.

完整提示词与风格拆解见 `references/style-presets.md`。

## 目录结构
```
photo-to-gouache/
├── references/
│   └── style-presets.md     # 完整提示词 + 风格拆解
└── assets/
    └── refs/                # 风格参考图（视觉锚点）
```

## 风格要点
- 媒介：不透明水粉，大色块 + 可见笔触
- 造型：色块起形、不勾黑边、人物简化为姿态小人
- 色彩：从原图取色再块面化，冷暖对撞，阴影带颜色
- 铁律：只改画法，不改原图的情绪与主题
