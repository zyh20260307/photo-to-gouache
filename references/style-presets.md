# Style preset for photo-to-gouache

> 当前 skill 只保留一种目标风格：水粉绘本插画（gouache storybook illustration）。风格必须与 user's photo 的意境/光线/氛围解耦——只迁移笔触、色块、简化手法，不迁移参考图的具体场景元素。

## Style analysis — 风格拆解（从参考图抽象出的可迁移维度）

**媒介与技法**
不透明水粉 / gouache / poster paint 质感。画面由**大色块**构成，色块之间有清晰的交界但边缘不完全锐利；**笔触可见**，能感知笔毛方向和颜料厚薄；不是照片写实，也不是矢量扁平。

**造型语言**
- 用**色块起形**，不靠黑色轮廓线
- 形状**简化**，细节被合并（例如窗户、树叶、人物都归纳为几何色块）
- 人物画成**小色块人形**：有姿态、有动作，无五官、无精细衣纹
- 透视和空间感靠**色彩冷暖/明度**推，不靠精确素描

**色彩规律**
- 颜色从**原照片的氛围**中提取，再 bold 化、块面化
- 善于做**冷暖对撞**（暖建筑 vs 冷海水 / 暖地面 vs 绿树）
- 饱和度偏高但分区明确，整体协调不脏
- 阴影不是灰色，而是**有颜色的投影**（偏紫、偏蓝或偏暖）

**氛围保留原则（关键）**
- 原图是**阳光明媚** → 用高明度暖色块 + 清晰光影
- 原图是**傍晚/阴天** → 保留灰调天空、柔和光线、静谧感
- 原图是**室内/城市/医疗场景** → 不强行加海水、藤蔓、建筑屋顶
- **风格只改变画法，不改变原图的情绪和主题**

## gouache-storybook —— 水粉绘本插画

Best for: any uploaded photo where the user wants the gouache/storybook painting treatment while preserving the original mood and atmosphere.

```
Transform this photo into a gouache painting illustration. Bold flat color blocks with expressive visible brush strokes, hand-painted gouache texture, simplified shapes, storybook picture-book style. Use colors drawn from the original scene's palette, applied in bold harmonious blocks with warm-cool contrast. No sharp black outlines—forms are defined by color and brushwork. No photorealistic details, no photographic texture. Small stylized human figures if present, with posture but no facial details. Preserve the original photo's mood, lighting, and atmosphere. Keep the original composition and main subjects clearly recognizable.
```

## Usage notes

- **This is the only preset.** Do not add scene-specific Mediterranean keywords (pink buildings, terracotta roofs, turquoise water) unless the user's photo actually is a Mediterranean scene.
- If the output looks too realistic, add: `no photorealistic details, no photographic texture, painted texture only`.
- If the output strays from the original composition, raise `input_fidelity` to `high`.
- If the output loses the original mood (e.g., a dusk photo becomes too sunny), explicitly append the mood description from the user's photo to the prompt.
