# design-skin vs design-dna 对比分析

> 两个 skill 都将设计拆成三层，但定位、哲学和输出目标不同。

---

## 1. 架构哲学

| | design-dna | design-skin |
|---|---|---|
| **三层命名** | `design_system` / `design_style` / `visual_effects` | `tokens` / `character` / `spectacle` |
| **字段要求** | "Every field below **must** appear" — 强制全填 | "Every section is **optional**" — 按需填充 |
| **扩展性** | 封闭字段清单，没有声明开放扩展 | 末尾有 Open Extension 章节，明确可加新维度 |
| **风格** | 枚举清单，像表格 | 描述性指引 + 示例，像文档 |

design-dna 的强制全填适合完整网站分析，但对简单输入（一张截图、一句描述）会产生大量空字段。design-skin 按需方式更灵活，但可能导致模型漏填。

---

## 2. Token 层（可量化的）

| 子维度 | design-dna (`design_system`) | design-skin (`tokens`) | 差异 |
|--------|------------------------------|------------------------|------|
| **色彩** | primary/secondary/accent 各一个 hex+role，neutral 一个 scale，semantic 4 个，surface 3 个，palette_type，contrast_strategy | `palette` 按色相族展开全 shade(50-950)；`roles` 开放式语义映射带 light/dark；`gradients` 独立渐变定义 | design-skin 更细（完整 shade + 双模式），design-dna 有 `palette_type` meta 字段 |
| **字体** | 7 级固定 scale(display→overline)，每级 4 属性，3 个 font_families，font_style_notes | `type.stacks` 开放式字体角色；`type.scale` 开放式命名层级；`type.character` 排版性格描述 | design-dna 更严格（7 级固定），design-skin 更灵活（不强制层级名） |
| **间距** | base_unit, scale, content_density, section_rhythm | `space.grid`（base+scale）；`space.roles`（语义间距）；`space.density`（密度+节奏） | design-skin 多了语义间距层（card-inset, control-padding 等） |
| **布局** | grid_system, max_content_width, columns, gutter, breakpoints, alignment_tendency | 同样覆盖网格/列/间距/断点/对齐 | 基本等价，字段名不同 |
| **形状** | border_radius(4 级), border_usage, divider_style | radii scale + border widths + border style + divider treatment | 基本等价，design-skin 合并到 `surfaces` |
| **深度** | shadow_style, 3 层级(low/med/high), depth_cues | elevation levels + depth philosophy | design-dna 多了 `depth_cues`（shadows vs overlapping vs blur） |
| **图标** | style, stroke_weight, size_scale, preferred_set | 同样覆盖 | 等价 |
| **动效** | easing, 3 级 duration, entrance/exit_pattern, philosophy | 4 级 timing, 4 条 easing, philosophy | design-dna 多了进场/退场模式，design-skin 多了 bounce easing |
| **组件** | 6 个固定组件 + notes | 7 个组件（多 badge/table），按需填 | 基本等价 |

---

## 3. 感受层（定性的）

| 子维度 | design-dna (`design_style`) | design-skin (`character`) | 差异 |
|--------|---------------------------|---------------------------|------|
| **个性** | mood, visual_metaphor, era_influence, genre, personality_traits, adjectives（6 字段） | mood, archetype, era, metaphor, traits（5 字段） | design-dna 多了 `adjectives`，design-skin 把 genre 改叫 archetype |
| **视觉语言** | complexity, ornamentation, whitespace_usage, visual_weight_distribution, focal_strategy, contrast_level, texture_usage（7 字段） | complexity, ornamentation, whitespace, weight_distribution, focal_strategy, contrast_level, texture（7 字段） | 1:1 对应，字段名简化 |
| **构图** | hierarchy_method, balance_type, flow_direction, grouping_strategy, negative_space_role（5 字段） | hierarchy_method, balance, flow, grouping, negative_space（5 字段） | 1:1 对应 |
| **图像处理** | photo_treatment, illustration_style, graphic_elements, pattern_usage, image_shape（5 字段） | photos, illustrations, decorative_elements, patterns, image_shapes（5 字段） | 1:1 对应 |
| **交互感** | feedback_style, hover_behavior, transition_personality, loading_style, microinteraction_density（5 字段） | feedback, hover_behavior, transitions, loading, micro_density（5 字段） | 1:1 对应 |
| **品牌语气** | tone, formality, cta_style, empty_state_approach, error_tone（5 字段） | tone, cta_style, empty_states, errors（4 字段） | design-dna 多了 `formality` |

坦白说，感受层两边高度同构。覆盖范围几乎相同，字段名换了但语义一致。

---

## 4. 特效层

| 子维度 | design-dna (`visual_effects`) | design-skin (`spectacle`) | 差异 |
|--------|-------------------------------|---------------------------|------|
| **概览** | effect_intensity, performance_tier, fallback_strategy, primary_technology（4 字段） | intensity, tech_ceiling, degradation（3 字段） | design-dna 多了 `primary_technology` |
| **背景** | background_effects — 6 个 params 字段 | backgrounds — active + kind + tech + params | 结构不同但覆盖相同 |
| **粒子** | particle_systems — enabled + 7 个 params | particles — active + kind + tech + params | design-dna params 更细（逐个列出 count/shape/size_range） |
| **3D** | 3d_elements — enabled + 7 个 params | three_d — active + kind + tech + params | design-dna params 更细（逐个列出 renderer/lighting/camera） |
| **着色器** | shader_effects — 5 个 params | shaders — active + kind + tech + params | 同上 |
| **滚动** | scroll_effects 分 3 子节点 | scroll 分 3 子节点 | 等价 |
| **文字** | text_effects — 4 个 params | text — active + kind + params | design-dna 多了 `technology` 字段 |
| **光标** | cursor_effects — 5 个 params | cursor — active + kind + params | 基本等价 |
| **图片** | image_effects — 4 个 params | images — active + kind + tech + params | 基本等价 |
| **玻璃/拟态** | glassmorphism_neumorphism — 5 个 params | glass — active + style + params | 基本等价 |
| **Canvas** | canvas_drawings — 5 个 params | canvas — active + kind + tech + params | 基本等价 |
| **SVG** | svg_animations — 4 个 params | svg — active + kind + params | 基本等价 |
| **兜底文本** | composite_notes | notes | 等价 |

特效层覆盖范围相同。核心差异：design-dna 逐个列出每个 param 字段（`params.count`, `params.shape`），design-skin 用 `params` 作为自由对象，只在文档里描述包含什么。

---

## 5. design-skin 有而 design-dna 没有的

| 特性 | 说明 |
|------|------|
| 色彩完整 shade scale | 50-950 全 shade，不是单个 hex |
| 双模式 light/dark | 每个语义 role 自带 light+dark 值 |
| 暗色模式自动推导规则 | 详细的 derivation rules |
| 语义间距 roles | page-margin, card-inset, control-padding 等 |
| 排版性格 (`type.character`) | "dramatic scale" vs "flat scale" 等描述 |
| 开放扩展声明 | 明确可以加新维度 |
| Figma 提取优先级 | variables > styles > components > screenshots |
| 输出格式生成 | Tailwind / CSS / React 三种模板 |
| Preview 页面 | 自包含 HTML 预览验证 |

---

## 6. design-dna 有而 design-skin 没有的

| 特性 | 说明 |
|------|------|
| `palette_type` | 配色策略分类（互补/类比/三角等） |
| `depth_cues` | 深度线索类型（阴影 vs 层叠 vs 模糊） |
| `formality` | 品牌语气的正式度 |
| `primary_technology` | 特效主力技术声明 |
| 入场/退场动画模式 | `entrance_pattern` / `exit_pattern` |
| `adjectives` | 独立于 mood 的形容词列表 |
| `description` per effect | 每个特效独立的文字描述字段 |
| Generation Guide | 完整的实现指南（技术选型表、代码模式、质量检查清单） |
| Fallback 代码示例 | `prefers-reduced-motion` + `hardwareConcurrency` 检测 |

---

## 7. 本质区别

| | design-dna | design-skin |
|---|---|---|
| **定位** | 分析 + 生成 HTML — 用 DNA 生成完整设计页面 | 分析 + 生成主题包 — 生成可复用的 Tailwind/CSS/React 主题文件 |
| **输出** | 一个 self-contained HTML | 一套主题文件（给其他项目引入） |
| **用途** | "照着这个风格做一个新页面" | "把这个风格变成可复用的皮肤，以后 vibe-coding 都用它" |
| **Schema 哲学** | 枚举清单，强制全填 | 描述指引，按需填充 |
| **特效实现** | Generation Guide 有详细的代码模式 | 只在 schema 里描述，没有独立的实现指南 |

**最大差距**：design-skin 缺一个等价于 design-dna `generation-guide.md` 的东西——那份文档详细说明了每种特效怎么选技术、怎么写代码、怎么做降级。design-skin 目前只有输出模板（Tailwind/CSS/React），没有 spectacle 层的实现指导。

---

## 8. 待补项

1. 补 `palette_type`、`depth_cues`、`entrance/exit_pattern`、`formality` 等有价值的字段到 skin-schema
2. 写 `references/spectacle-guide.md` — spectacle 层的实现指南（技术选型、代码模式、降级策略）
