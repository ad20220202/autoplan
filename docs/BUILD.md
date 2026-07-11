# 打包流程

## 前提条件

- Node.js 22（项目 `.nvmrc` 指定版本）
- `package.json` 必须有 `homepage` 字段，否则 deb 构建报错

## 命令

```bash
# 加载 nvm（如果使用 nvm 管理 Node.js）
nvm use 22

# 完整打包（TypeScript 检查 + Vite 构建 + electron-builder）
npm run package:linux    # AppImage + deb

# 仅重新打包 deb（跳过 tsc/vite，适合已构建过的情况）
npx electron-builder --linux deb --x64
```

## 分平台打包

| 命令 | 产物 |
|---|---|
| `npm run package:linux` | AppImage + deb |
| `npm run package:mac` | DMG + ZIP |
| `npm run package:win` | NSIS 安装包 |
| `npm run package:all` | 三平台全量 |

## 产物位置

```
release/
├── AutoPlan-0.2.3-beta.9-linux-amd64.deb
├── AutoPlan-0.2.3-beta.9-linux-x86_64.AppImage
└── linux-unpacked/       # 未打包的解压目录
```

## 已知问题

| 问题 | 原因 | 解决 |
|---|---|---|
| `Please specify project homepage` | `package.json` 缺少 `homepage` 字段 | 添加 `"homepage": "https://github.com/lyming99/autoplan"` |
| deb 构建要求 `homepage` | `electron-builder` 的 `FpmTarget` 需要此元数据 | 同上 |
| `desktopName is not set` 警告 | Linux 桌面环境需要此字段关联窗口 | 可选修复，不影响功能 |
