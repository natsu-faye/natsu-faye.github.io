---
title: Markdown 语法速查
date: 2026-09-30 07:45:04
tags: [Markdown, 笔记]
categories:
  - 语法
---

## 标题层级

# 一级标题
## 二级标题
### 三级标题

## 列表

- 无序项一
- 无序项二
  - 嵌套子项

1. 有序项一
2. 有序项二

- [ ] 未完成任务
- [x] 已完成任务

## 表格

| 项目 | 说明 | 状态 |
| --- | --- | --- |
| APlayer | 音乐播放器 | 👌已配置 |

p.s.
- 表头下方的分隔行 | --- | --- | --- | 必须存在!列数要和表头一致
- 想让某列居中或右对齐，可以在分隔行里加冒号：:--- 左对齐、:---: 居中、---: 右对齐
- 单元格里如果要写真正的竖线|符号，需要转义成 \|，否则表可能会乱掉！
- 表格的一个单元格里默认不能直接回车换行，否则会被当成新的一行。如果确实需要换行，用 HTML 的 <br> 标签
  - 示例：
| 步骤 | 说明 |
| :---: | :---: |
| 排查 | 第一步：检查接口<br>第二步：检查插件<br>第三步：检查配置 |

## 代码

行内代码：`hexo clean`


## 引用

> 这是一段普通引用(=^-ω-^=)

> 引用可以有多行，
> 换行不需要额外符号，
> 只要每行前面都带 `>`。

### 嵌套引用
> 外层引用!
>
> > 内层引用~
> > 可以继续嵌套( *ˊᵕˋ)✩︎‧₊

### 引用里也可以放其他元素~比如列表和代码
> **排查要点**
> - 先确认接口是否连通
> - 再确认插件是否支持自定义
>
> `grep -r "meting_api" node_modules/hexo-tag-aplayer/`

>p.s.引用块内部如果要分段，中间需要留一个空的 > 行，否则会被合并成一段。


## 图片
标准语法示意：![替代文字(这个图会裂开^^)](图片路径 "可选标题")
![Meting 请求返回的报错内容](/images/meting-error.png "网络面板响应截图")

*如果需要控制显示尺寸，可以直接用 HTML 标签：*

<img src="/images/meting-error.png" alt="Meting 报错响应" width="600" />

| 属性 | 写什么 | 示例 |
| --- | --- | --- |
| `src` | 图片在站点里的路径 | `/images/meting-error.png` |
| `alt` | 图片描述，加载失败时显示 | `Meting 报错响应` |
| `width` | 想要的宽度（像素） | `600` |


p.s.
1. 图片文件放在 source/images/ 下，这样路径写 /images/xxx.png 比较稳
2. 替代文字（方括号里）不要留空，方便日后看图找图
3. 引号里的是鼠标悬停时显示的提示~


## 链接

### 普通链接：[显示文字](URL)
[Hexo 官方文档](https://hexo.io/zh-cn/)
[MetingJS 仓库](https://github.com/metowolf/MetingJS)

### 带标题的链接（有悬停提示）
[Hexo 官方文档](https://hexo.io/zh-cn/ "Hexo 中文文档首页")

### 站内文章互链（这里推荐用相对路径或者 Hexo 的标签~）
[本篇：Markdown 语法速查](/2026/09/30/Markdown-语法速查/)

### 引用式链接（适合同一个链接在文中出现多次）
接口文档见 [Meting][meting-ref]，插件说明见 [APlayer][aplayer-ref]。

[meting-ref]: https://github.com/metowolf/MetingJS
[aplayer-ref]: https://aplayer.js.org/


## （表格书写尝试~）aplyer配置时 问题排查过程の表格
| 环节 | 操作 / 现象 | 结论 |
| :---: | :---: | :---: |
| 接口连通性 | 浏览器直接访问 `api.injahow.cn/meting/?server=netease&type=playlist&id=18430435018` | 返回正常 JSON，每首歌带 `url`、`pic`、`lrc` 字段 |
| 插件支持确认 | `grep -r "meting_api" node_modules/hexo-tag-aplayer/` | 有输出，说明支持自定义接口 |
| 配置注入验证 | 页面源代码搜索 `meting_api` | 第 195 行已注入，配置生效 |
| 异常发现 | 网络面板中该 xhr 仅 377 B，`Content-Type` 为 `text/html` | 体积异常，疑似返回错误信息而非歌单数据 |
| 响应内容 | `{"error":"unknown type"}` | 接口侧未正确识别请求 |
| 根因定位 | 请求 URL 中参数被拼接两遍，且 `auth=undefined` | 占位符未被替换，反而被追加到已有参数后 |
| 解决方案 | `meting_api` 只填基础地址，不写 `:server` 等占位符 | 歌单正常加载，播放恢复 |




```bash
hexo clean && hexo g && hexo server
