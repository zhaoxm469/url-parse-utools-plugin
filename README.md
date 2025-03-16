# URL参数解析器 - uTools插件

一个简单而强大的 URL 参数解析工具，帮助你快速解析和编辑 URL 参数。

## 功能特点

- 自动识别剪贴板中的 URL
- 解析 URL 参数并以 JSON 格式展示
- 支持参数编码/解码切换
- 支持编辑参数并实时更新 URL
- 一键复制参数到剪贴板

## 使用方法

1. 复制任意含有参数的 URL 到剪贴板
2. 呼出 uTools 搜索框
3. 输入 "url" 或 "URL解析" 或 "参数解析" 来启动插件
4. 插件会自动解析剪贴板中的 URL 并显示参数

## 开发说明

本插件使用 Vue 3 + Vite 开发。

### 开发环境设置

```bash
# 安装依赖
npm install

# 启动开发服务器
npm run dev

# 构建生产版本
npm run build
```

### 项目结构

```
url-parser-plugin/
├── src/
│   ├── components/
│   ├── App.vue          # 主应用组件
│   ├── main.js          # 入口文件
│   └── main.css        # 全局样式
├── index.html           # HTML 入口
├── vite.config.js       # Vite 配置
└── package.json         # 项目配置
```

## 注意事项

- 插件需要 uTools 平台支持
- URL 必须是有效的格式才能被正确解析
- 编辑参数时请确保使用有效的 JSON 格式

## **发布事项**

1. 运行 npm run build 
2. 然后在 utools 开发者市场点击打包
3. 导入打包好的文件