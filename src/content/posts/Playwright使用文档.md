---
title: Playwright 使用文档
published: 2026-04-08
description: Playwright 是微软推出的一款强大的浏览器自动化工具，本文档详细介绍了其使用方法和最佳实践。
tags: [Playwright, 自动化测试, 浏览器]
category: 技术
---

# Playwright 使用文档

## 一、什么是 Playwright？

Playwright 是微软推出的一款强大的浏览器自动化工具，它就像一个 "浏览器操控大师"，可以模拟人类在浏览器中的各种操作，比如点击、输入、导航等。想象一下，你有一个任劳任怨的机器人助手，它可以帮你打开浏览器、访问网站、填写表单、点击按钮，甚至还能处理复杂的交互场景——这就是 Playwright 的魅力所在！

### Playwright 的特点

- **跨浏览器支持**：支持 Chrome、Firefox、Safari 和 Edge 等主流浏览器
- **跨平台**：可在 Windows、macOS 和 Linux 上运行
- **自动等待**：智能等待元素出现，无需手动添加 sleep
- **强大的选择器**：支持 CSS、XPath、文本内容等多种元素定位方式
- **网络拦截**：可以拦截和修改网络请求
- **无头模式**：支持无界面运行，适合 CI/CD 环境
- **多语言支持**：支持 JavaScript、TypeScript、Python、Java、C# 等多种语言

## 二、安装与环境配置

### 1. 安装 Playwright

在项目中安装 Playwright 非常简单，只需运行以下命令：

```bash
# 使用 npm
npm install playwright

# 使用 yarn
yarn add playwright

# 使用 pnpm
pnpm add playwright
```

### 2. 安装浏览器驱动

安装 Playwright 后，需要下载对应的浏览器驱动。Playwright 提供了一个命令来自动下载所有支持的浏览器：

```bash
npx playwright install
```

如果你只需要特定的浏览器，可以指定浏览器名称：

```bash
# 只安装 Chrome
npx playwright install chromium

# 只安装 Firefox
npx playwright install firefox

# 只安装 Safari
npx playwright install webkit
```

### 3. 项目配置

在 TypeScript 项目中，建议在 `tsconfig.json` 中添加以下配置：

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true
  }
}
```

## 三、核心功能使用

### 1. 启动浏览器

Playwright 可以启动不同的浏览器实例，支持无头模式和有头模式。

#### 示例：启动 Chrome 浏览器

```typescript
import { chromium } from 'playwright';

async function run() {
  // 启动浏览器（有头模式）
  const browser = await chromium.launch({
    headless: false, // 有头模式
    slowMo: 500, // 慢动作，方便观察
    executablePath: 'C:/Program Files (x86)/Chromebrowser/Chrome.exe' // 可选：指定本地浏览器路径
  });

  // 创建新页面
  const page = await browser.newPage();

  // 访问网站
  await page.goto('https://www.xiaohongshu.com/');

  // 关闭浏览器
  await browser.close();
}

run();
```

#### 示例：启动 Firefox 浏览器

```typescript
import { firefox } from 'playwright';

async function run() {
  const browser = await firefox.launch({
    headless: false
  });
  // ...
}
```

#### 示例：启动 Safari 浏览器

```typescript
import { webkit } from 'playwright';

async function run() {
  const browser = await webkit.launch({
    headless: false
  });
  // ...
}
```

### 2. 导航与页面操作

#### 导航到网页

```typescript
// 基本导航
await page.goto('https://www.xiaohongshu.com/');

// 等待页面完全加载
await page.goto('https://www.xiaohongshu.com/', {
  waitUntil: 'networkidle' // 等待网络空闲
});

// 前进和后退
await page.goBack();
await page.goForward();

// 刷新页面
await page.reload();
```

#### 获取页面信息

```typescript
// 获取页面标题
const title = await page.title();
console.log('页面标题:', title);

// 获取页面 URL
const url = page.url();
console.log('页面 URL:', url);

// 获取页面内容
const content = await page.content();
console.log('页面内容:', content.substring(0, 100) + '...'); // 只显示前 100 个字符
```

### 3. 元素定位与交互

Playwright 提供了多种元素定位方式，让你可以精确找到页面上的任何元素。

#### 元素定位方法

| 方法 | 描述 | 示例 |
|------|------|------|
| `page.$(selector)` | 查找单个元素 | `const button = await page.$('button');` |
| `page.$$(selector)` | 查找多个元素 | `const links = await page.$$('a');` |
| `page.waitForSelector(selector)` | 等待元素出现 | `const input = await page.waitForSelector('input');` |
| `page.locator(selector)` | 创建定位器（推荐） | `const button = page.locator('button');` |

#### 选择器类型

- **CSS 选择器**：`'button'`, `'.login-btn'`, `'#username'`
- **XPath 选择器**：`'//button'`, `'//div[@class="login-btn"]'`
- **文本内容**：`'text=登录'`, `'text=Sign in'`
- **数据属性**：`'[data-testid="login-button"]'`
- **组合选择器**：`'button:has-text("登录")'`

#### 元素交互操作

```typescript
// 点击元素
await page.click('button');

// 输入文本
await page.fill('input[type="text"]', '用户名');

// 清空输入框
await page.fill('input[type="text"]', '');

// 键盘操作
await page.press('input[type="text"]', 'Enter');

// 悬停元素
await page.hover('.menu-item');

// 拖拽元素
await page.dragAndDrop('.source', '.target');

// 检查元素是否可见
const isVisible = await page.isVisible('button');
console.log('按钮是否可见:', isVisible);

// 检查元素是否启用
const isEnabled = await page.isEnabled('button');
console.log('按钮是否启用:', isEnabled);
```

### 4. 等待机制

Playwright 提供了智能的等待机制，无需手动添加 `sleep`，让你的代码更加稳定。

#### 自动等待

Playwright 在执行操作时会自动等待元素达到可操作状态，比如：
- 点击元素时，会等待元素可见且可点击
- 输入文本时，会等待元素可见且可编辑

#### 显式等待

如果需要等待特定条件，可以使用显式等待：

```typescript
// 等待元素出现
await page.waitForSelector('button', { timeout: 5000 });

// 等待元素消失
await page.waitForSelector('button', { state: 'hidden' });

// 等待函数返回 true
await page.waitForFunction(() => {
  return document.querySelector('.loading') === null;
});

// 等待网络空闲
await page.waitForLoadState('networkidle');
```

### 5. 处理对话框

Playwright 可以处理浏览器的对话框，如 alert、confirm、prompt 等。

```typescript
// 监听对话框
page.on('dialog', async (dialog) => {
  console.log('对话框消息:', dialog.message());
  
  // 接受对话框
  await dialog.accept();
  
  // 或者拒绝对话框
  // await dialog.dismiss();
  
  // 对于 prompt 对话框，可以输入内容
  // await dialog.accept('输入内容');
});

// 触发对话框
await page.evaluate(() => {
  alert('测试对话框');
});
```

### 6. 文件上传

处理文件上传操作非常简单：

```typescript
// 选择文件输入框
const fileInput = await page.$('input[type="file"]');

// 上传文件
await fileInput.setInputFiles(['path/to/file1.txt', 'path/to/file2.jpg']);

// 清除文件选择
await fileInput.setInputFiles([]);
```

### 7. 网络请求拦截

Playwright 可以拦截和修改网络请求，这对于测试和模拟场景非常有用。

```typescript
// 拦截所有网络请求
await page.route('**/*', async (route, request) => {
  console.log('请求 URL:', request.url());
  console.log('请求方法:', request.method());
  
  // 继续请求
  await route.continue();
  
  // 或者中止请求
  // await route.abort();
  
  // 或者修改请求
  // await route.continue({
  //   url: 'https://example.com',
  //   headers: {
  //     ...request.headers(),
  //     'X-Custom-Header': 'value'
  //   }
  // });
});

// 访问网站，触发网络请求
await page.goto('https://www.xiaohongshu.com/');
```

### 8. 截图与录屏

#### 截图

```typescript
// 截取整个页面
await page.screenshot({ path: 'screenshot.png', fullPage: true });

// 截取特定元素
const element = await page.$('.header');
await element.screenshot({ path: 'element.png' });
```

#### 录屏

```typescript
// 启动录屏
await page.video().start({ path: 'recording.webm' });

// 执行操作
await page.goto('https://www.xiaohongshu.com/');
await page.click('button');

// 停止录屏
await page.video().stop();
```

## 四、高级功能

### 1. 浏览器上下文

浏览器上下文是一个隔离的环境，可以在同一个浏览器实例中创建多个上下文，每个上下文都有自己的 cookies、localStorage 等。

```typescript
// 创建浏览器实例
const browser = await chromium.launch();

// 创建第一个上下文
const context1 = await browser.newContext();
const page1 = await context1.newPage();
await page1.goto('https://www.xiaohongshu.com/');

// 创建第二个上下文
const context2 = await browser.newContext();
const page2 = await context2.newPage();
await page2.goto('https://www.xiaohongshu.com/');

// 关闭上下文
await context1.close();
await context2.close();

// 关闭浏览器
await browser.close();
```

### 2. 持久化上下文

持久化上下文可以保存登录状态，避免每次都需要重新登录。

```typescript
// 创建持久化上下文
const context = await chromium.launchPersistentContext('./user-data', {
  headless: false
});

const page = await context.newPage();
await page.goto('https://www.xiaohongshu.com/');

// 手动登录后，状态会保存在 ./user-data 目录中
// 下次启动时会自动保持登录状态

await context.close();
```

### 3. 多页面操作

在同一个浏览器上下文可以创建多个页面，实现多标签页操作。

```typescript
// 创建浏览器上下文
const context = await chromium.launch();

// 创建第一个页面
const page1 = await context.newPage();
await page1.goto('https://www.xiaohongshu.com/');

// 创建第二个页面
const page2 = await context.newPage();
await page2.goto('https://www.baidu.com/');

// 在页面之间切换
await page1.bringToFront();
await page1.click('button');

await page2.bringToFront();
await page2.fill('input', 'Playwright');

// 关闭页面
await page1.close();
await page2.close();

// 关闭上下文
await context.close();
```

### 4. 执行 JavaScript

在页面中执行 JavaScript 代码：

```typescript
// 执行简单的 JavaScript
const title = await page.evaluate(() => {
  return document.title;
});
console.log('页面标题:', title);

// 传递参数给 evaluate
const result = await page.evaluate((value) => {
  return value * 2;
}, 10);
console.log('结果:', result); // 输出 20

// 操作 DOM
await page.evaluate(() => {
  document.querySelector('button').click();
});
```

## 五、最佳实践

### 1. 使用页面对象模型 (POM)

页面对象模型是一种设计模式，将页面的元素和操作封装成对象，提高代码的可维护性。

```typescript
// pages/login-page.ts
import { Page, Locator } from 'playwright';

export class LoginPage {
  private page: Page;
  private usernameInput: Locator;
  private passwordInput: Locator;
  private loginButton: Locator;

  constructor(page: Page) {
    this.page = page;
    this.usernameInput = page.locator('input[name="username"]');
    this.passwordInput = page.locator('input[name="password"]');
    this.loginButton = page.locator('button[type="submit"]');
  }

  async login(username: string, password: string) {
    await this.usernameInput.fill(username);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }
}

// 使用页面对象
import { LoginPage } from './pages/login-page';

const loginPage = new LoginPage(page);
await loginPage.login('admin', 'password');
```

### 2. 错误处理

添加适当的错误处理，提高代码的健壮性：

```typescript
try {
  await page.goto('https://www.xiaohongshu.com/');
  await page.click('button');
} catch (error) {
  console.error('操作失败:', error);
  // 可以在这里添加截图、日志等操作
  await page.screenshot({ path: 'error.png' });
}
```

### 3. 性能优化

- **减少不必要的导航**：尽量在同一个页面完成操作，避免频繁导航
- **使用定位器**：`page.locator()` 比 `page.$()` 性能更好
- **合理使用等待**：避免过度使用 `waitForTimeout`，优先使用自动等待
- **并行执行**：对于独立的测试，可以使用 `Promise.all` 并行执行

### 4. 调试技巧

- **使用有头模式**：`headless: false`，可以观察浏览器的实际操作
- **慢动作**：`slowMo: 500`，减慢操作速度，方便观察
- **截图**：在关键步骤添加截图，便于调试
- **日志**：添加适当的日志，记录操作过程
- **浏览器开发者工具**：在有头模式下，可以使用浏览器的开发者工具查看网络请求、DOM 结构等

## 六、常见问题与解决方案

### 1. 元素定位失败

**问题**：无法找到页面元素

**解决方案**：
- 检查选择器是否正确
- 等待元素出现：使用 `page.waitForSelector()`
- 检查页面是否加载完成：使用 `page.waitForLoadState()`
- 检查元素是否在 iframe 中：如果在 iframe 中，需要先切换到 iframe

### 2. 超时问题

**问题**：操作超时

**解决方案**：
- 增加超时时间：`page.waitForSelector(selector, { timeout: 10000 })`
- 检查网络连接：确保网络正常
- 检查页面是否有加载动画：等待加载动画消失
- 检查元素是否真的存在：可能页面结构已变化

### 3. 登录状态丢失

**问题**：每次启动浏览器都需要重新登录

**解决方案**：
- 使用持久化上下文：`chromium.launchPersistentContext('./user-data')`
- 保存和恢复 cookies：`const cookies = await context.cookies();` 和 `await context.addCookies(cookies);`

### 4. 网络请求失败

**问题**：网络请求失败或被拦截

**解决方案**：
- 检查网络连接
- 检查网站是否需要 VPN
- 使用网络请求拦截：`page.route()` 可以模拟网络请求
- 增加请求超时时间

### 5. 浏览器启动失败

**问题**：无法启动浏览器

**解决方案**：
- 检查浏览器是否安装
- 指定浏览器路径：`executablePath: 'path/to/browser.exe'`
- 检查权限：确保有足够的权限启动浏览器
- 检查端口是否被占用：可能需要更换端口

## 七、总结

Playwright 是一款功能强大、易于使用的浏览器自动化工具，它不仅可以用于测试，还可以用于数据抓取、自动化操作等场景。通过本文档的学习，你应该已经掌握了 Playwright 的基本用法和高级功能，能够开始使用它来解决实际问题。

## 八、参考资源

- **Playwright 官方文档**：[https://playwright.dev/docs/intro](https://playwright.dev/docs/intro)
- **Playwright GitHub**：[https://github.com/microsoft/playwright](https://github.com/microsoft/playwright)
- **Playwright 中文文档**：[https://playwright.dev/docs/intro](https://playwright.dev/docs/intro)（可以使用浏览器翻译功能）

---
