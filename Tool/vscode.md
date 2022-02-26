@import "./image/效率笔记TOC.jpg"


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Markdown](#markdown)
  - [编辑样式](#编辑样式)
  - [标题](#标题)
  - [表格](#表格)
  - [资源插入](#资源插入)

<!-- /code_chunk_output -->


# Markdown

## 编辑样式

```bash
// cmd-shift-p
Markdown Preview Enhanced: Customize Css
```

## 标题

```markdown
// 创建
Markdown Preview Enhanced: Create Toc

// 忽略
# 需要被忽略的标题 {ignore=true}
```

## 表格
```markdown
// 创建
Markdown Preview Enhanced: Insert Table
```

## 资源插入

```markdown
// 插入本地图片
@import "./要插入的本地图片名称"

// 插入在线图片
@import "要插入在线图片的地址"

// 设置图片尺寸
@import "./要插入的图片" {width="300px" height="200px" title="图片的标题" alt="我的 alt"}
```