---
title: Promise 实战笔记
published: 2025-11-26T11:42:26.381Z
tags: [Promise, 异步编程, 前端开发笔记]
category: 技术
draft: false
pinned: false
---


# Promise 实战笔记：从对话框理解异步编程

## 🎯 什么是 Promise？
**Promise** 是 JavaScript 中用于处理异步操作的对象，就像一张**"外卖订单"**：
- 你下单后拿到一张订单（Promise 对象）
- 订单状态会从「处理中」→「已完成」或「已取消」
- 无论结果如何，你都会得到通知

## 📊 Promise 的三种状态
| 状态 | 中文 | 类比 | 说明 |
|------|------|------|------|
| `pending` | 进行中 | 商家正在做饭 | 初始状态，异步操作还在执行 |
| `fulfilled` | 已成功 | 外卖送到了 | 异步操作完成，结果可用 |
| `rejected` | 已失败 | 外卖取消了 | 异步操作失败，有错误信息 |

> ⚠️ **状态不可逆**：一旦从 `pending` 转变为其他状态，就无法再改变

## 🔧 Promise 基本用法
### 语法结构
```javascript
const promise = new Promise((resolve, reject) => {
  // 异步操作逻辑
  if (操作成功) {
    resolve(结果); // 状态变为 fulfilled
  } else {
    reject(错误); // 状态变为 rejected
  }
});

// 处理结果
promise
  .then(成功回调)  // 处理 fulfilled 状态
  .catch(失败回调) // 处理 rejected 状态
  .finally(总是执行); // 无论成功失败都会执行
```

## 🚀 实战案例：对话框交互
### 代码实例
```javascript
// Element Plus 对话框返回 Promise 对象
ElMessageBox.confirm("确定批量启用所选项？")
  .then(() => {
    // 用户点击「确定」→ Promise 成功
    ElMessage({ type: "success", message: "已全部启用！" });
  })
  .catch(() => {
    // 用户点击「取消」→ Promise 失败
  });
```

### 执行流程图
```
开始 → 调用 ElMessageBox.confirm() → 返回 pending 状态的 Promise
       ↓
用户交互 → 点击「确定」→ Promise 状态变为 fulfilled → 执行 .then() 回调 → 显示成功消息
       ↓
用户交互 → 点击「取消」→ Promise 状态变为 rejected → 执行 .catch() 回调 → 什么都不做
```

## 💡 Promise 的优势
### 1. 避免回调地狱
**传统回调**（嵌套地狱）：
```javascript
elConfirm("确定？", function(ok) {
  if (ok) {
    elMessage("成功", function() {
      // 更多嵌套... 😵
    });
  }
});
```

**Promise**（链式调用）：
```javascript
ElMessageBox.confirm("确定？")
  .then(() => elMessage("成功"))
  .catch(() => console.log("取消")); // 清晰优雅 🎉
```

### 2. 统一错误处理
所有异步操作的错误都可以在最后的 `.catch()` 中捕获，避免了到处写错误处理代码。

### 3. 方便的链式操作
可以通过 `.then()` 链式调用，将多个异步操作按顺序连接起来，就像流水线一样。

## 🎨 形象记忆法
把 Promise 比作**"快递物流"**：
- `new Promise()` → 下单
- `pending` → 运输中
- `fulfilled` → 已签收
- `rejected` → 已退回
- `.then()` → 签收后的操作
- `.catch()` → 退回后的处理

## 📝 核心要点总结
1. **Promise 是异步操作的容器**：封装异步操作，返回一个代表结果的对象
2. **三种状态**：pending → fulfilled / rejected（不可逆）
3. **链式调用**：`.then()` 处理成功，`.catch()` 处理失败
4. **避免回调地狱**：让异步代码更清晰、易维护
5. **统一错误处理**：所有错误集中处理

## 🚩 实战技巧
1. **始终添加 `.catch()`**：避免未捕获的 Promise 错误
2. **合理使用 `finally()`**：清理资源、隐藏加载状态等
3. **结合 `async/await`**：让异步代码看起来像同步代码（高级用法）


