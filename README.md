# DT 谨慎创造模式（DSH Agent 预设）

以 DSH 自带的**创造模式**（`cordis` 预设）为基础派生的一个 Agent 预设，预设 id 为
`desktop-cautious`，界面显示名为 **DT谨慎创造模式**。

## 和「创造模式」的差别

只有两处，其余插件清单逐项一致（含 plan 模式、压缩、子代理/工作流、todo、web、技能、插件管理、cordis 工具等）：

1. **persona 前缀**：追加三组长期生效的规则（顺序：平台口径 → 代码风格 → 改动边界）
   - 桌面端口径：要生效就重启 DSH 桌面应用（侧边栏底部设置按钮旁的「重启」，或退出后重开）；
     不说“刷新页面 / F5 / Ctrl+R / 重载标签页”，也不把 `127.0.0.1` 地址当作给用户的操作入口；
     看文件、终端、网页、文档用界面内的面板，打开文件说「在应用中打开」。
   - 组件化编码：先找可复用的现有实现，重复逻辑收敛成单一组件/模块，用组合代替复制，结构尽量简单。
   - 改动边界：只改用户明确点名的东西；联动点（共享组件、相邻配置、类型与文档、测试、调用方）
     先列清单问过再动；不顺手重构/清理/升级依赖；发现问题先报告。
2. **skill-filesystem 的技能目录**：创造模式那 4 个技能（`agent-experience`、
   `cordis-plugin-development`、`editing-cordis-compositions`、`cordis-composition-reference`）
   位于 dsh 运行时自带的 `@deepseek-ai/dsh-agent-preset/skills`。预设子插件的 `baseUrl`
   不保证能解析到这个包，所以这里先用 `createRequire(baseUrl)` 尝试，再从启动参数的
   dsh 运行时根目录兜底，只保留实际存在的目录。
   注意 `customSkillDirs` 是**字符串数组**：`!!js` 必须写在数组这一层并返回数组
   （写成 `- !!js ...` 返回单个字符串会被 schema 判成 `invalid config`）。

## 使用

1. 已安装到 `desktop` profile（`dsh.profile.bundles` 里的 `@local/dsh-preset-desktop-cautious`）。
2. 在**新建会话**页面的预设选择器（或设置中的 Agent 预设位置）选择「DT谨慎创造模式」。
3. 预设在**新会话**生效；已有会话沿用启动时绑定的预设版本，不会自动切换。
4. 修改本目录的 `cordis.patch.yml` 或 `package.json` 后，需要重启 DSH 桌面应用才能生效
   （侧边栏底部、设置按钮旁的「重启」按钮）。

## 文件

| 文件 | 作用 |
|---|---|
| `package.json` | bundle 清单，`dsh.bundle.patch` 指向补丁文件 |
| `cordis.patch.yml` | 唯一的补丁层：插入 `preset-desktop-cautious` 这一行预设声明 |
