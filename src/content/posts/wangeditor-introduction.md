---
title: WangEditor 富文本编辑器组件介绍
published: 2025-12-8T18:42:26.381Z
tags: [前端开发笔记,富文本编辑器]
category: 技术
draft: false
pinned: false
---

# WangEditor 富文本编辑器组件介绍

## 1. 基本概念

WangEditor 是一款基于 JavaScript 和 DOM 操作的轻量级富文本编辑器，由国内开发者王福朋（wangeditor.com）开发和维护。它以简单易用、功能丰富、扩展性强而著称，广泛应用于各类 Web 应用中。

### 1.1 设计理念

- **轻量级**：核心代码体积小，加载速度快
- **易用性**：API 设计简洁，易于集成和使用
- **功能丰富**：提供常用的富文本编辑功能
- **可扩展**：支持自定义插件和功能扩展
- **兼容性**：兼容主流浏览器，包括 Chrome、Firefox、Safari 和 Edge

## 2. 主要特点

### 2.1 轻量级

- 核心代码仅约 300KB，加载速度快
- 不依赖 jQuery 等重型框架，独立运行
- 支持模块化加载，按需引入功能

### 2.2 易用性

- 简洁的 API 设计，易于集成到各种 Web 项目中
- 提供完整的中文文档和示例
- 丰富的事件接口，方便开发者进行二次开发
- 支持 Vue、React、Angular 等主流前端框架的集成

### 2.3 功能丰富

- **文本格式化**：支持加粗、斜体、下划线、删除线、字体大小、颜色等
- **段落样式**：支持对齐方式、缩进、行高、段落间距等
- **列表功能**：支持有序列表、无序列表、嵌套列表
- **图片处理**：支持图片上传、插入、调整大小、对齐方式
- **表格功能**：支持表格插入、编辑、合并单元格等
- **代码块**：支持代码高亮显示
- **链接功能**：支持插入外部链接、电子邮件链接等
- **引用功能**：支持引用块样式

### 2.4 可扩展性

- 支持自定义工具栏按钮
- 支持自定义插件开发
- 支持自定义主题样式
- 支持自定义图片上传逻辑

## 3. 技术架构

### 3.1 核心组成

- **编辑器主体**：负责内容的编辑和渲染
- **工具栏**：提供各种编辑功能的入口
- **命令系统**：处理各种编辑操作的执行
- **事件系统**：提供编辑器状态变化的通知
- **插件系统**：支持功能的扩展和定制

### 3.2 渲染机制

WangEditor 采用 "所见即所得"（WYSIWYG）的渲染方式，直接操作 DOM 元素进行内容编辑和渲染。它使用原生 JavaScript 实现，不依赖其他重型框架，具有较高的性能和兼容性。

## 4. 在 Vue 项目中的应用

### 4.1 组件引入

```javascript
// 在 Vue 单文件组件中引入
import WangEditor from "@/components/WangEditor/index.vue";
```

### 4.2 基本使用

```html
<!-- 在模板中使用 -->
<template>
  <div>
    <WangEditor v-model="content" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import WangEditor from "@/components/WangEditor/index.vue";

const content = ref("");
</script>
```

### 4.3 高级配置

```html
<template>
  <div>
    <WangEditor 
      v-model="content" 
      :config="editorConfig" 
      @change="handleContentChange"
    />
  </div>
</template>

<script setup>
import { ref, reactive } from "vue";
import WangEditor from "@/components/WangEditor/index.vue";

const content = ref("");

// 编辑器配置
const editorConfig = reactive({
  // 工具栏配置
  toolbarConfig: {
    excludeKeys: ['fullscreen', 'insertVideo'] // 排除不需要的按钮
  },
  // 编辑区配置
  editorConfig: {
    placeholder: '请输入内容...',
    readOnly: false
  },
  // 上传图片配置
  uploadImgConfig: {
    server: '/api/upload',
    fieldName: 'file',
    maxSize: 2 * 1024 * 1024 // 2MB
  }
});

// 内容变化事件
function handleContentChange(newContent) {
  console.log('内容已更新:', newContent);
}
</script>
```

## 5. 核心功能详解

### 5.1 文本格式化

WangEditor 提供了丰富的文本格式化功能，包括：

- 加粗、斜体、下划线、删除线
- 字体大小、字体颜色、背景颜色
- 上标、下标
- 清除格式

### 5.2 段落样式

- 对齐方式：左对齐、居中对齐、右对齐、两端对齐
- 缩进：增加缩进、减少缩进
- 行高：设置文本行高
- 段落间距：设置段落之间的距离

### 5.3 列表功能

- 有序列表：1, 2, 3...
- 无序列表：•, •, •...
- 嵌套列表：支持多级列表嵌套
- 任务列表：支持勾选框样式的列表

### 5.4 图片处理

- 图片上传：支持本地图片上传到服务器
- 图片插入：支持从网络插入图片
- 图片编辑：支持调整图片大小、对齐方式
- 图片属性：支持设置图片 alt、title 等属性

### 5.5 表格功能

- 表格插入：支持自定义行数和列数
- 表格编辑：支持添加/删除行、添加/删除列
- 单元格操作：支持合并单元格、拆分单元格
- 表格样式：支持设置边框、背景色等

### 5.6 代码块

- 支持插入代码块
- 支持代码高亮显示
- 支持多种编程语言

### 5.7 其他功能

- 链接：支持插入外部链接、电子邮件链接
- 引用：支持插入引用块
- 分割线：支持插入水平分割线
- 全屏：支持全屏编辑模式

## 6. 自定义与扩展

### 6.1 自定义工具栏

```javascript
const editorConfig = {
  toolbarConfig: {
    // 自定义工具栏按钮顺序
    toolbarKeys: [
      'bold', 'italic', 'underline', 'strikeThrough',
      'color', 'bgColor',
      '|', // 分隔线
      'fontSize', 'fontFamily',
      '|',
      'bulletedList', 'numberedList', 'todo',
      '|',
      'link', 'image', 'table', 'codeBlock'
    ],
    // 排除不需要的按钮
    excludeKeys: ['fullscreen', 'insertVideo']
  }
};
```

### 6.2 自定义上传图片

```javascript
const editorConfig = {
  uploadImgConfig: {
    // 上传服务器地址
    server: '/api/upload',
    // 上传字段名
    fieldName: 'file',
    // 最大文件大小
    maxSize: 2 * 1024 * 1024, // 2MB
    // 允许的图片类型
    allowedFileTypes: ['image/jpeg', 'image/png', 'image/gif'],
    // 自定义上传参数
    meta: {
      token: 'your-token'
    },
    // 自定义文件名
    customFilename: (file) => {
      const timestamp = Date.now();
      const ext = file.name.split('.').pop();
      return `${timestamp}.${ext}`;
    },
    // 上传成功回调
    onSuccess: (res) => {
      console.log('上传成功:', res);
      return res.data.url; // 返回图片 URL
    },
    // 上传失败回调
    onFail: (err) => {
      console.error('上传失败:', err);
    }
  }
};
```

### 6.3 自定义插件

WangEditor 支持开发自定义插件，可以扩展编辑器的功能：

```javascript
// 自定义插件示例
function myPlugin(editor) {
  // 注册命令
  editor.registerCommand('myCommand', {
    exec: () => {
      // 执行命令逻辑
      editor.insertText('Hello WangEditor!');
    },
    undo: () => {
      // 撤销命令逻辑
      editor.deleteBackward('Hello WangEditor!'.length);
    }
  });
  
  // 添加工具栏按钮
  editor.getToolbar().addItem({
    key: 'myButton',
    title: '我的按钮',
    iconSvg: '<svg>...</svg>',
    menu: false,
    command: 'myCommand'
  });
}

// 使用插件
editor.use(myPlugin);
```
## 7. 总结

WangEditor 是一款优秀的轻量级富文本编辑器，具有轻量、易用、功能丰富等特点，非常适合各类 Web 应用使用。在会议预订系统中，它为用户提供了便捷的富文本编辑功能，提升了内容编辑的效率和体验。

如果您需要一个简单易用、功能适中的富文本编辑器，WangEditor 是一个不错的选择。它的中文文档完善，社区活跃，能够满足大多数 Web 应用的富文本编辑需求。

## 8. 学习资源

- **官方网站**：[wangeditor.com](https://www.wangeditor.com/)
- **GitHub 仓库**：[github.com/wangeditor-team/wangEditor](https://github.com/wangeditor-team/wangEditor)
- **Vue 集成示例**：[www.wangeditor.com/v5/for-frame.html#vue-3](https://www.wangeditor.com/v5/for-frame.html#vue-3)