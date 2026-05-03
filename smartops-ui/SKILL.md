---
name: SmartOps Web UI 风格（暗色运维控制台）
description: 对齐 SmartOps 当前 Vue3 + Element Plus 暗色主题、布局分区、表格/表单/上传/分页等组件展示规范，保证新增页面与现有风格一致。
---

## 何时使用
- 在 SmartOps 前端继续新增/改造页面（Vue3 + Element Plus + Vite）
- 需要保持与当前暗色运维控制台一致：卡片分区、表格可读性、上传区不突兀、分页/下拉/滚动条暗色化
- 需要统一“高风险操作二次确认”“审计友好展示”等交互习惯

## 设计基调（一句话）
深色背景 + 低饱和蓝紫点缀 + 卡片分区 + 细边框层次 + 克制动效；偏企业运维平台，避免高对比白块与花哨渐变。

## 全局设计令牌（来源：`frontend/src/styles.css`）
- 背景层级
  - `--bg-deep`：页面深底
  - `--bg-sidebar`：侧栏
  - `--bg-card` / `--bg-card-hover`：卡片与其 hover
- 边框
  - `--border-color`：默认边框
  - `--border-accent`：强调/悬浮边框
- 文本
  - `--text-primary`：主文案
  - `--text-secondary`：次级说明
  - `--text-muted`：弱化辅助
- 品牌/语义色
  - `--accent` / `--accent-glow`
  - `--success` / `--warning` / `--danger` / `--info`

## 布局与信息架构
- 页面骨架：左侧固定侧栏 + 右侧内容滚动（`.layout` / `.content`）
- 内容分组：统一使用 `.card` 承载区块，区块间距一致
- 页面标题：`.page-header` + `.page-title` + `.page-desc`（标题强、描述弱）

## Element Plus 组件风格约定（全局覆盖优先）
- 输入类：`el-input__wrapper`、`el-textarea__inner`
- 选择器：同时覆盖新版 `el-select__wrapper`（避免白底选择框）
- 下拉弹层（Popper）：深色半透明背景 + 深色边框 + 深阴影（避免白底突兀）
- 表格：表头弱化、行 hover 轻微高亮；长字段列优先 `show-overflow-tooltip`
- 分页：分页按钮深色底 + 细边框；active 用 accent 强调
- 滚动条：WebKit 深色轨道 + 蓝灰滑块（全局一致）

## 上传/拖拽区（审批附件、脚本上传等）
- 避免纯白大面积拖拽区
- 推荐模式：深色渐变底 + 虚线边框 + hover 强化（参考现有 `workflow-upload` 样式思路）
- 需要时用 `<style scoped>` + `:deep(.el-upload-dragger)` 精准覆盖

## 交互与文案（运维产品习惯）
- 高风险动作：`ElMessageBox.confirm` 二次确认，说明后果与责任边界
- 页面级说明：`el-alert`；短反馈：`ElMessage`
- 审计相关列表：优先“可检索 + 可分页”，避免无限拉长页面

## 时间展示（项目口径）
- 用户界面展示时间：统一使用北京时间格式化工具（例如 `frontend/src/utils/datetime.ts` 的 `formatBeijingTime`）
- 注意后端无时区字符串的解析策略，避免显示偏差（以项目当前实现为准）

## 代码组织建议
- 全局暗色与 Element 覆盖：集中在 `frontend/src/styles.css`
- 页面局部特殊交互：`<style scoped>` + `:deep(...)`
- 可复用的小函数：放 `frontend/src/utils/`

## 反模式（避免）
- 大面积纯白容器直接叠在暗色背景上（对比突兀）
- 多个页面复制粘贴同一套 Element 覆盖（应沉淀到全局样式或小组件）
- 表格无 tooltip/无分页/无筛选导致“运维数据一屏爆炸”

## 交付自检清单（新增页面）
- 是否使用 `.card` 分区？标题区是否一致？
- 输入/选择/分页/弹层是否与暗色主题一致？
- 表格是否需要 tooltip / 固定操作列 / 分页？
- 是否需要二次确认与审计提示？
- 时间展示是否符合北京时间口径？
