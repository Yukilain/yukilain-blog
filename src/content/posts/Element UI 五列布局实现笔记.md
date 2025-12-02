---
title: Element UI 五列布局以及适配移动端实现笔记
published: 2025-12-02
description: Element UI栅格系统五列布局实现方案，同时考虑移动端响应式布局问题
tags: [Vue, 前端开发笔记, Element Plus,vue-router]
category: 技术
draft: false
pinned: false
---

# Element UI 五列布局以及适配移动端实现笔记

## 问题描述
在使用Element UI栅格系统时，需要实现一行显示**五个均等分布的数据卡片**，但Element UI的栅格系统默认是24列，直接使用`el-col :md="4"`会导致一行只能显示6个卡片，而使用`:md="5"`则只能显示4个卡片，都无法满足五列布局的需求。同时，需要确保**移动端保持正常的响应式布局**，不受五列布局的影响。

## 解决方案
结合CSS Flexbox和Element UI栅格系统，**只在非移动端应用CSS Flexbox直接控制宽度**，实现真正的五列均等布局，移动端保持Element UI默认的响应式布局。

## 实现步骤

### 1. 了解现有样式
首先查看项目中已有的`row-flex`类样式定义：

```scss
// row-flex
.row-flex {
    margin-top: -10px;
    margin-bottom: -10px;
    display: flex;
    flex-wrap: wrap;
}

.row-flex .el-col {
    padding-top: 10px;
    padding-bottom: 10px;
}
// row-flex END
```

### 2. 添加媒体查询样式
在SCSS文件中添加媒体查询，**只在非移动端（屏幕宽度≥1080px）应用五列布局**：

```scss
// 只在非移动端应用五列布局（屏幕宽度≥1080px）
@media (min-width: 1080px) {
    // 针对数据卡片的五列布局
    .row-flex.five-cols .el-col {
        flex: 0 0 20% !important;
        max-width: 20% !important;
    }
}
```

### 3. 修改HTML结构
为数据卡片的`el-row`添加`five-cols`类，启用五列布局样式：

```vue
<el-row class="row-flex five-cols">
  <el-col :md="4" :sm="12" :xs="12">
    <!-- 卡片内容 -->
  </el-col>
  <!-- 重复4次，共5个el-col -->
</el-row>
```

### 4. 保持响应式设计
保留原有的响应式配置（`:md="4" :sm="12" :xs="12"`），确保在小屏幕上自动调整为2列或1列布局。

## 实现原理

1. **Element UI栅格系统**：默认24列，通过`col`的`span`或`md`等属性设置列宽，实现响应式布局
2. **CSS Flexbox**：直接控制元素的宽度和弹性布局，用于实现五列均等分布
3. **媒体查询**：通过`@media (min-width: 1080px)`精准控制样式作用范围，只在非移动端应用五列布局
4. **选择器优先级**：使用`.row-flex.five-cols .el-col`选择器，结合`!important`确保样式生效
5. **移动端兼容**：移动端不触发媒体查询，保持Element UI默认的响应式布局，一行显示2个卡片

## 媒体查询断点说明

| 断点 | 屏幕宽度 | 效果 |
|------|----------|------|
| 移动端 | < 1080px | Element UI默认响应式布局，一行显示2个卡片 |
| 非移动端 | ≥ 1080px | CSS Flexbox五列布局，一行显示5个卡片 |

## 注意事项

1. **避免使用小数列宽**：Element UI不支持小数列宽（如`:md="4.8"`），会导致布局异常
2. **媒体查询精准性**：根据项目实际需求调整媒体查询断点，确保在合适的屏幕尺寸下应用五列布局
3. **保持响应式设计**：不要移除原有的响应式配置，确保在不同屏幕尺寸下都有良好表现
4. **样式优先级**：使用`!important`确保Flexbox样式能够覆盖Element UI默认样式
5. **结合现有样式**：结合项目中已有的样式类（如`row-flex`），避免样式冲突

## 完整代码示例

### SCSS样式
```scss
// 五列布局样式 - 只在非移动端生效
@media (min-width: 1080px) {
    .row-flex.five-cols .el-col {
        flex: 0 0 20% !important;
        max-width: 20% !important;
    }
}
```

### Vue组件
```vue
<template>
  <div class="content">
    <!-- 数据卡片容器 -->
    <el-row class="row-flex five-cols">
      <!-- 卡片1 -->
      <el-col :md="4" :sm="12" :xs="12">
        <div class="item">
          <div class="icon-box">
            <el-icon size="24"><MessageFilled /></el-icon>
          </div>
          <div class="text-box">
            <div class="text">今日会议总数</div>
            <div class="num">12</div>
          </div>
        </div>
      </el-col>
      
      <!-- 卡片2 -->
      <el-col :md="4" :sm="12" :xs="12">
        <div class="item">
          <div class="icon-box">
            <el-icon size="24"><UserFilled /></el-icon>
          </div>
          <div class="text-box">
            <div class="text">今日参会人数</div>
            <div class="num">34</div>
          </div>
        </div>
      </el-col>
      
      <!-- 卡片3-5省略 -->
      
    </el-row>
  </div>
</template>

<style scoped>
.item {
  background: #fff;
  padding: 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  transition: all 0.3s ease;
  height: 100%;
}

.icon-box {
  margin-right: 16px;
  color: #409eff;
}

.text {
  font-size: 14px;
  color: #606266;
  margin-bottom: 8px;
}

.num {
  font-size: 24px;
  font-weight: bold;
  color: #303133;
}
</style>
```

## 效果对比

| 方案 | 非移动端效果 | 移动端效果 | 问题 |
|------|--------------|------------|------|
| `:md="4"` | 一行显示6个卡片 | 一行显示2个卡片 | 无法实现5列布局 |
| `:md="5"` | 一行显示4个卡片 | 一行显示2个卡片 | 剩余空间过大 |
| `:md="4.8"` | 布局异常 | 布局异常 | Element UI不支持小数列宽 |
| **媒体查询+flex: 0 0 20%** | **一行显示5个均等卡片** | **一行显示2个卡片** | **完美实现需求** |

## 总结
通过结合Element UI栅格系统、CSS Flexbox和媒体查询，可以灵活实现各种特殊的列数布局，解决Element UI默认24列栅格系统的限制，同时确保移动端响应式布局正常。

**关键思路**：
- 利用Element UI的响应式配置，确保移动端正常显示
- 通过媒体查询精准控制样式作用范围，只在非移动端应用五列布局
- 结合CSS Flexbox直接控制列宽，实现真正的五列均等分布

**技术优势**：
1. **精准控制**：通过媒体查询可以根据不同屏幕尺寸应用不同的布局策略
2. **响应式兼容**：移动端保持Element UI默认的响应式布局，无需额外调整
3. **灵活扩展**：可以根据需要调整媒体查询断点和列宽，适用于各种特殊布局需求
4. **代码简洁**：样式集中管理，易于维护和扩展

---

**适用场景**：
- 需要实现非24列约数的均等布局（如5列、7列等）
- 希望保持Element UI的响应式设计特性
- 需要灵活调整列宽，不受24列系统限制
- 要求移动端和非移动端有不同的布局表现

**技术栈**：
- Vue 3
- Element Plus
- CSS Flexbox
- CSS Media Query
- Element UI栅格系统

**优化建议**：
- 根据项目实际需求调整媒体查询断点，建议使用设计稿中的断点值
- 可以为不同的布局场景定义不同的列数类（如`five-cols`、`seven-cols`等）
- 结合CSS变量动态调整列宽，实现更灵活的布局控制
- 添加过渡效果，提升用户体验
