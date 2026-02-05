# MyMD2Pic - PDR (Product Design Review) Document

## 1. 项目概述

### 1.1 项目名称
MyMD2Pic - Markdown to Image Converter

### 1.2 项目描述
一个纯前端单网页应用，用于将用户提供的Markdown代码渲染为可视化图像，支持中文内容，并允许用户直接复制图像粘贴到其他软件（如Word文档）中。

### 1.3 部署目标
GitHub Pages

### 1.4 技术栈
- **前端框架**：React
- **构建工具**：Vite
- **样式方案**：Tailwind CSS
- **Markdown解析**：markdown-it
- **图像生成**：html2canvas
- **代码高亮**：Prism.js
- **无后端服务**：纯前端应用

### 1.5 安装要求
- **Node.js版本**：>= 16.0.0
- **包管理器**：npm 或 yarn
- **安装方式**：所有依赖仅项目本地安装，不使用全局安装
- **禁止全局安装**：所有npm install命令必须使用--save或--save-dev，不使用-g参数
- **依赖管理**：使用package.json管理所有项目依赖

---

## 2. 功能需求

### 2.1 核心功能

#### 2.1.1 Markdown输入
- **功能描述**：提供一个文本输入区域，允许用户输入或粘贴Markdown代码
- **输入格式**：标准Markdown语法
- **特殊支持**：完整支持中文内容
- **输入方式**：
  - 直接在文本框中输入
  - 粘贴已有Markdown内容
  - （可选）上传.md文件

#### 2.1.2 Markdown渲染
- **功能描述**：将输入的Markdown代码实时渲染为HTML并显示在页面上
- **渲染引擎**：markdown-it
- **支持的Markdown元素**：
  - 标题（H1-H6）
  - 段落和文本格式（粗体、斜体、删除线）
  - 列表（有序、无序）
  - 代码块（支持语法高亮）
  - 引用块
  - 链接
  - 表格
  - 分隔线
  - 任务列表
- **图片处理**：忽略Markdown中的图片链接，在图像中显示图片链接文字

#### 2.1.3 图像生成
- **功能描述**：将渲染后的HTML内容转换为图像格式
- **输出格式**：PNG（推荐）或SVG
- **生成方式**：
  - 使用html2canvas库将DOM元素转换为Canvas
  - 从Canvas导出为图像
- **图像质量**：高分辨率，保证文字清晰度

#### 2.1.4 图像复制
- **功能描述**：允许用户选中生成的图像并复制到剪贴板
- **复制方式**：
  - 点击按钮复制整个图像
  - 选中部分区域复制（可选）
- **兼容性**：支持粘贴到Word、PowerPoint、图片查看器等应用

### 2.2 布局控制功能

#### 2.2.1 尺寸限制
- **最大高度限制**：
  - 默认值：1024像素
  - 可调整范围：400-2000像素
  - 用户可自定义设置
- **最大宽度限制**：
  - 默认值：680像素
  - 可调整范围：400-1200像素
  - 用户可自定义设置

#### 2.2.2 内容适配策略
当内容超出尺寸限制时，采用以下优先级策略：

**优先级1：调整行间距**
- 减小段落之间的间距
- 减小列表项之间的间距
- 调整标题与正文之间的间距
- 调整表格行高
- 最小行间距限制：正常间距的50%

**优先级2：缩小字体**
- 按比例缩小所有字体大小
- 保持字体层级关系（H1 > H2 > H3 > 正文）
- 最小字体限制：10px（正文）
- 缩放范围：50%-100%

**优先级3：分页/分块（可选）**
- 如果内容仍然超出，提示用户考虑分块处理
- 或提供滚动查看选项

#### 2.2.3 实时预览
- 在调整参数时实时显示效果
- 显示当前内容的实际尺寸
- 显示是否超出限制的警告

### 2.3 用户界面

#### 2.3.1 主界面布局
```
┌─────────────────────────────────────────────┐
│              MyMD2Pic 标题栏                 │
├─────────────────────┬───────────────────────┤
│                     │                       │
│   Markdown输入区    │    渲染预览区         │
│                     │                       │
│   [文本框]          │    [渲染结果]         │
│                     │                       │
├─────────────────────┴───────────────────────┤
│              控制面板                        │
│  [高度设置] [宽度设置] [字体设置] [复制]    │
└─────────────────────────────────────────────┘
```
- **布局方式**：左右分栏
- **左侧**：Markdown输入区
- **右侧**：渲染预览区
- **底部**：控制面板

#### 2.3.2 控制面板
- **高度滑块/输入框**：设置最大高度（默认1024px）
- **宽度滑块/输入框**：设置最大宽度（默认680px）
- **字体大小滑块**：调整基础字体大小（默认14px）
- **行间距滑块**：调整行间距系数（默认1.5）
- **主题选择**：浅色/深色主题
- **复制按钮**：一键复制图像到剪贴板
- **下载按钮**：下载图像文件（可选）
- **重置按钮**：恢复默认设置

#### 2.3.3 响应式设计
- 支持桌面端（主要目标）
- 支持平板设备
- 移动端适配（次要优先级）

#### 2.3.4 字体配置
- **中文字体**：使用无版权字体确保在所有环境中正常显示
  - 优先级：`"Noto Sans SC", -apple-system, BlinkMacSystemFont, "Segoe UI", "Microsoft YaHei", "PingFang SC", sans-serif`
- **代码字体**：使用无版权等宽字体
  - 优先级：`"Fira Code", "JetBrains Mono", "Consolas", "Monaco", monospace`

#### 2.3.5 主题配置
- **默认主题**：浅色主题（light）
- **可选主题**：浅色/深色
- **代码高亮主题**：
  - 浅色主题：One Light
  - 深色主题：Tomorrow Night

---

## 3. 技术设计

### 3.1 技术选型

#### 3.1.1 前端框架
**选择**：React
- 优点：组件化、生态丰富、开发效率高
- 缺点：需要构建工具、包体积较大
- 适用场景：复杂交互、长期维护

#### 3.1.2 构建工具
**选择**：Vite
- 优点：快速启动、热更新快、配置简单
- 与React配合良好

#### 3.1.3 样式方案
**选择**：Tailwind CSS
- 优点：快速开发、原子化CSS、响应式友好
- 适合快速原型开发

#### 3.1.4 Markdown解析库
**选择**：markdown-it
- 优点：功能丰富、可扩展、支持插件、中文支持好
- 广泛使用，社区活跃

#### 3.1.5 图像生成库
**选择**：html2canvas
- 优点：成熟稳定、社区活跃、API完善
- 将DOM转换为Canvas，从Canvas导出图像

#### 3.1.6 代码高亮库
**选择**：Prism.js
- 优点：轻量、主题丰富、性能好
- 支持的语言：JavaScript, TypeScript, Python, Java, C++, CSS, HTML, JSON, Bash, SQL
- 主题：Tomorrow Night（深色）/ One Light（浅色）

### 3.2 核心算法

#### 3.2.1 内容适配算法
```
算法：自适应内容布局
输入：渲染后的DOM元素，最大宽度W_max，最大高度H_max
输出：调整后的样式参数

步骤：
1. 获取当前内容的实际尺寸（W_current, H_current）
2. 检查是否超出限制
   - 如果 W_current <= W_max 且 H_current <= H_max
     → 结束，无需调整
3. 尝试调整行间距
   - 设置 line_spacing = line_spacing * 0.9
   - 重新计算尺寸
   - 如果 line_spacing < 0.5，进入下一步
4. 尝试缩小字体
   - 设置 font_size = font_size * 0.95
   - 重新计算尺寸
   - 如果 font_size < 10px，进入下一步
5. 显示警告，建议用户分块处理
```

#### 3.2.2 图像生成流程
```
流程：Markdown → 图像
1. 用户输入Markdown
2. 解析Markdown为HTML
3. 应用样式和主题
4. 检查尺寸，执行适配算法
5. 渲染到DOM
6. 使用html2canvas转换为Canvas
7. 从Canvas导出为Blob
8. 写入剪贴板（Clipboard API）
```

### 3.3 数据流设计

```
用户输入 → Markdown文本
    ↓
Markdown解析器 → HTML
    ↓
样式处理器 → 样式化HTML
    ↓
尺寸检查器 → 是否超出？
    ↓ 是
适配算法 → 调整样式
    ↓
DOM渲染器 → 可视化内容
    ↓
图像生成器 → Canvas
    ↓
剪贴板API → 复制到剪贴板
```

### 3.4 状态管理

```javascript
// 核心状态结构
const state = {
  markdown: '',              // 输入的Markdown文本
  html: '',                  // 渲染后的HTML
  settings: {
    maxHeight: 1024,         // 最大高度
    maxWidth: 680,          // 最大宽度
    fontSize: 14,           // 基础字体大小
    lineHeight: 1.5,         // 行间距系数
    theme: 'light'          // 主题
  },
  currentSize: {
    width: 0,
    height: 0
  },
  isOverflowing: false,      // 是否超出限制
  generatedImage: null       // 生成的图像数据
};
```

---

## 4. 用户交互流程

### 4.1 基本使用流程
1. 用户打开网页
2. 在输入框中输入或粘贴Markdown内容
3. 系统自动实时渲染预览
4. 用户调整尺寸参数（可选）
5. 系统自动适配内容
6. 用户点击"复制图像"按钮
7. 系统生成图像并复制到剪贴板
8. 用户在其他应用（如Word）中粘贴

### 4.2 高级使用流程
1. 用户打开网页
2. 输入Markdown内容
3. 调整主题（浅色/深色）
4. 精细调整尺寸参数
5. 调整字体大小和行间距
6. 查看实时预览和尺寸信息
7. 点击"下载"保存图像文件（可选）
8. 复制图像到剪贴板

### 4.3 错误处理流程
1. Markdown解析失败 → 显示错误提示
2. 图像生成失败 → 显示错误信息，提供重试按钮
3. 剪贴板访问失败 → 显示权限提示，引导用户手动操作
4. 内容超出限制 → 显示警告，建议调整或分块

---

## 5. 非功能性需求

### 5.1 性能要求
- **渲染响应时间**：< 500ms（中等长度内容）
- **图像生成时间**：< 2s（普通内容）
- **页面加载时间**：< 3s（首次访问）
- **内存占用**：< 200MB

### 5.2 兼容性要求
- **浏览器支持**：
  - Chrome/Edge 90+
  - Firefox 88+
  - Safari 14+
- **剪贴板API**：需要现代浏览器支持
- **移动端**：基本支持（iOS Safari 14+, Chrome Android）

### 5.3 可用性要求
- **界面直观**：新用户无需文档即可使用
- **实时反馈**：所有操作都有视觉反馈
- **错误提示**：清晰的错误信息和解决建议
- **键盘快捷键**：
  - `Ctrl/Cmd + C`：复制图像
  - `Ctrl/Cmd + R`：重置设置
- **错误提示方式**：当内容超出限制无法适配时，在预览区顶部显示警告横幅

### 5.4 可访问性要求
- **语义化HTML**：使用正确的HTML标签
- **键盘导航**：支持Tab键导航
- **屏幕阅读器**：基本支持
- **对比度**：符合WCAG AA标准

### 5.5 安全性要求
- **XSS防护**：Markdown解析需要过滤危险标签
- **CSP策略**：设置适当的内容安全策略
- **无敏感数据**：所有数据在客户端处理

---

## 6. 部署方案

### 6.1 GitHub Pages部署
- **仓库名称**：mymd2pic
- **部署URL**：https://username.github.io/mymd2pic/
- **仓库结构**：
  ```
  mymd2pic/
  ├── dist/               # 构建输出目录（Vite默认）
  ├── public/             # 静态资源目录
  ├── src/                # 源代码目录
  ├── package.json        # 项目配置
  ├── vite.config.js      # Vite配置
  └── README.md           # 项目说明
  ```

### 6.2 详细部署步骤

#### 6.2.1 前置准备
1. **安装Node.js**
   - 下载并安装Node.js（版本 >= 16.0.0）
   - 验证安装：`node --version` 和 `npm --version`

2. **创建GitHub仓库**
   - 登录GitHub，创建新仓库
   - 仓库名称：mymd2pic
   - 设置为Public（公开仓库）
   - 初始化README（可选）

#### 6.2.2 本地项目初始化
```bash
# 1. 克隆仓库到本地
git clone https://github.com/username/mymd2pic.git
cd mymd2pic

# 2. 初始化项目（如果还没有package.json）
npm init -y

# 3. 安装Vite和React（项目本地安装）
npm install --save-dev vite @vitejs/plugin-react

# 4. 安装React（项目本地安装）
npm install react react-dom

# 5. 安装其他依赖（项目本地安装）
npm install --save-dev tailwindcss postcss autoprefixer
npm install markdown-it html2canvas prismjs
```

#### 6.2.3 配置Vite
创建`vite.config.js`文件：
```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/mymd2pic/',  // 重要：设置GitHub Pages的基础路径
  build: {
    outDir: 'dist',
    assetsDir: 'assets',
    sourcemap: false,
    minify: 'terser',
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          markdown: ['markdown-it'],
          canvas: ['html2canvas']
        }
      }
    }
  }
})
```

#### 6.2.4 配置package.json
在`package.json`中添加以下脚本：
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "deploy": "npm run build && gh-pages -d dist"
  },
  "devDependencies": {
    "gh-pages": "^6.0.0"
  }
}
```

#### 6.2.5 构建和部署
```bash
# 1. 安装gh-pages（项目本地安装）
npm install --save-dev gh-pages

# 2. 构建项目
npm run build

# 3. 部署到GitHub Pages
npm run deploy
```

#### 6.2.6 GitHub Pages配置
1. 进入GitHub仓库的Settings页面
2. 点击左侧菜单的"Pages"
3. 在"Build and deployment"部分：
   - Source选择：Deploy from a branch
   - Branch选择：gh-pages分支
   - Folder选择：/(root)
4. 点击"Save"
5. 等待几分钟，GitHub会自动部署
6. 访问生成的URL：https://username.github.io/mymd2pic/

### 6.3 部署脚本

#### 6.3.1 Windows PowerShell部署脚本
创建`deploy.ps1`文件：
```powershell
# deploy.ps1 - Windows PowerShell部署脚本

Write-Host "开始部署到GitHub Pages..." -ForegroundColor Green

# 检查是否在项目根目录
if (-not (Test-Path "package.json")) {
    Write-Host "错误：未找到package.json，请在项目根目录运行此脚本" -ForegroundColor Red
    exit 1
}

# 清理旧的构建
Write-Host "清理旧的构建文件..." -ForegroundColor Yellow
if (Test-Path "dist") {
    Remove-Item -Recurse -Force "dist"
}

# 安装依赖
Write-Host "安装项目依赖..." -ForegroundColor Yellow
npm install

# 构建项目
Write-Host "构建项目..." -ForegroundColor Yellow
npm run build

# 检查构建是否成功
if (-not (Test-Path "dist")) {
    Write-Host "错误：构建失败，未找到dist目录" -ForegroundColor Red
    exit 1
}

# 部署到GitHub Pages
Write-Host "部署到GitHub Pages..." -ForegroundColor Yellow
npm run deploy

Write-Host "部署完成！" -ForegroundColor Green
Write-Host "访问地址：https://username.github.io/mymd2pic/" -ForegroundColor Cyan
```

使用方法：
```powershell
# 在PowerShell中运行
.\deploy.ps1
```

#### 6.3.2 Bash部署脚本（Linux/Mac/Git Bash）
创建`deploy.sh`文件：
```bash
#!/bin/bash
# deploy.sh - Bash部署脚本

echo "开始部署到GitHub Pages..."

# 检查是否在项目根目录
if [ ! -f "package.json" ]; then
    echo "错误：未找到package.json，请在项目根目录运行此脚本"
    exit 1
fi

# 清理旧的构建
echo "清理旧的构建文件..."
rm -rf dist

# 安装依赖
echo "安装项目依赖..."
npm install

# 构建项目
echo "构建项目..."
npm run build

# 检查构建是否成功
if [ ! -d "dist" ]; then
    echo "错误：构建失败，未找到dist目录"
    exit 1
fi

# 部署到GitHub Pages
echo "部署到GitHub Pages..."
npm run deploy

echo "部署完成！"
echo "访问地址：https://username.github.io/mymd2pic/"
```

使用方法：
```bash
# 添加执行权限
chmod +x deploy.sh

# 运行脚本
./deploy.sh
```

#### 6.3.3 一键部署npm脚本
在`package.json`中添加：
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "deploy": "npm run build && gh-pages -d dist",
    "deploy:clean": "rm -rf dist && npm run build && gh-pages -d dist"
  }
}
```

使用方法：
```bash
# 标准部署
npm run deploy

# 清理后重新部署
npm run deploy:clean
```

### 6.4 页面配置
- **页面标题**：MyMD2Pic - Markdown转图片工具
- **页面描述**：将Markdown代码转换为精美图片，支持中文，可直接粘贴到Word等文档中
- **Meta标签**：
  ```html
  <title>MyMD2Pic - Markdown转图片工具</title>
  <meta name="description" content="将Markdown代码转换为精美图片，支持中文，可直接粘贴到Word等文档中">
  ```

### 6.5 构建流程
```bash
# 开发环境
npm run dev

# 生产构建
npm run build

# 本地预览构建结果
npm run preview

# 部署到GitHub Pages
npm run deploy
```

### 6.6 CDN优化
- 使用CDN加载第三方库
- 启用Gzip压缩
- 设置合理的缓存策略

### 6.7 部署注意事项
1. **首次部署**：
   - 确保GitHub仓库是公开的
   - 首次部署可能需要等待5-10分钟
   - 检查GitHub Actions的部署状态

2. **更新部署**：
   - 每次代码更新后运行`npm run deploy`
   - 部署会自动更新gh-pages分支
   - GitHub Pages会自动检测并重新部署

3. **故障排查**：
   - 如果页面无法访问，检查GitHub Pages设置
   - 查看GitHub Actions的部署日志
   - 确认vite.config.js中的base路径正确

4. **自定义域名**（可选）：
   - 在仓库Settings → Pages中添加自定义域名
   - 在项目根目录添加CNAME文件
   - 配置DNS解析

---

## 7. 已确认的技术决策

### 7.1 技术选型
- **前端框架**：React
- **构建工具**：Vite
- **样式方案**：Tailwind CSS

### 7.2 功能范围
- **文件上传**：不支持
- **图像下载**：不支持
- **历史记录**：不支持
- **预设模板**：不支持
- **批量处理**：不支持
- **自定义样式**：不支持
- **多格式导出**：仅PNG
- **代码高亮**：支持
- **国际化**：不支持
- **离线使用**：不支持
- **数据分析**：不支持

### 7.3 UI/UX设计
- **主题**：浅色/深色（默认浅色）
- **布局**：左右分栏
- **移动端优先**：否

### 7.4 详细配置
- **中文字体**：使用无版权字体（Noto Sans SC优先）
- **代码字体**：使用无版权等宽字体
- **代码高亮主题**：One Light（浅色）/ Tomorrow Night（深色）
- **支持语言**：JavaScript, TypeScript, Python, Java, C++, CSS, HTML, JSON, Bash, SQL
- **图片处理**：忽略图片，显示图片链接文字
- **快捷键**：Ctrl/Cmd + C（复制）、Ctrl/Cmd + R（重置）
- **错误提示**：预览区顶部警告横幅
- **页面标题**：MyMD2Pic - Markdown转图片工具
- **页面描述**：将Markdown代码转换为精美图片，支持中文，可直接粘贴到Word等文档中
- **仓库名称**：mymd2pic

---

## 8. 开发计划

### 8.1 第一阶段：MVP（最小可行产品）
- [ ] 基础UI框架搭建
- [ ] Markdown输入和渲染
- [ ] 基础图像生成
- [ ] 简单的复制功能
- [ ] 基础尺寸限制

### 8.2 第二阶段：核心功能完善
- [ ] 内容适配算法实现
- [ ] 参数调整界面
- [ ] 实时预览优化
- [ ] 错误处理
- [ ] 响应式设计

### 8.3 第三阶段：用户体验优化
- [ ] 主题系统
- [ ] 快捷键支持
- [ ] 加载动画
- [ ] 使用提示
- [ ] 性能优化

### 8.4 第四阶段：部署和维护
- [ ] GitHub Pages部署
- [ ] 文档完善
- [ ] Bug修复
- [ ] 用户反馈收集

---

## 9. 风险评估

### 9.1 技术风险
- **风险1**：html2canvas在某些浏览器中兼容性问题
  - 缓解措施：提供polyfill或降级方案
- **风险2**：剪贴板API权限问题
  - 缓解措施：提供手动复制方案
- **风险3**：中文渲染问题
  - 缓解措施：测试多种中文字体和渲染方案

### 9.2 性能风险
- **风险1**：大文件渲染性能问题
  - 缓解措施：限制输入长度，提供分页提示
- **风险2**：图像生成内存占用
  - 缓解措施：优化Canvas处理，及时清理内存

### 9.3 用户体验风险
- **风险1**：适配算法效果不理想
  - 缓解措施：提供手动微调选项
- **风险2**：复制到Word后格式变化
  - 缓解措施：测试多种粘贴场景，提供格式选项

---

## 10. 成功标准

### 10.1 功能完整性
- ✅ 能够正确渲染常见Markdown语法
- ✅ 支持中文内容显示
- ✅ 能够生成高质量图像
- ✅ 能够成功复制到剪贴板并粘贴到Word

### 10.2 性能指标
- ✅ 页面加载时间 < 3s
- ✅ 渲染响应时间 < 500ms
- ✅ 图像生成时间 < 2s

### 10.3 用户体验
- ✅ 界面直观易用
- ✅ 错误提示清晰
- ✅ 适配算法有效

### 10.4 部署成功
- ✅ 成功部署到GitHub Pages
- ✅ 可通过公开URL访问
- ✅ 在主流浏览器中正常工作

---

## 附录

### A. 参考资源
- [CommonMark规范](https://spec.commonmark.org/)
- [html2canvas文档](https://html2canvas.hertzen.com/)
- [markdown-it文档](https://github.com/markdown-it/markdown-it)
- [GitHub Pages文档](https://docs.github.com/en/pages)

### B. 类似项目参考
- [Carbon](https://carbon.now.sh/) - 代码片段美化
- [Dillinger](https://dillinger.io/) - Markdown编辑器
- [Marked Demo](https://marked.js.org/demo/)

### C. 术语表
- **Markdown**：轻量级标记语言
- **DOM**：文档对象模型
- **Canvas**：HTML5画布元素
- **PWA**：渐进式Web应用
- **CSP**：内容安全策略

---

**文档版本**：2.0  
**创建日期**：2026-02-04  
**最后更新**：2026-02-04  
**状态**：已确认，可以开始开发
