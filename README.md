# Extensions Tools Universal

适用于 Cocos Creator `2.4.x` 与 `3.x` 的跨版本扩展。仓库当前对外提供的核心能力有两项：

- `映射资源名称路径`：扫描项目中的 Bundle，生成 `assets/scripts/BundleAssetsConfig.ts`
- `资源目录说明 Inspector`：为资源文件夹读取 `.{文件夹名}.md` 并显示在检查器中

扩展内部还保留了一套跨版本适配层与 `Tools` 工具类，方便继续在此基础上开发新的编辑器能力。

## 快速开始

1. 将扩展目录放到对应位置。
   - Cocos Creator 2.x：项目 `packages/` 下
   - Cocos Creator 3.x：项目 `extensions/` 下
2. 在扩展目录执行安装与编译。

```bash
npm install
npm run build
```

安装阶段会执行 `scripts/install.js`，自动检测当前 Creator 大版本，并用 `package.v2.json` 或 `package.v3.json` 更新根目录 `package.json`，同时写入 `.cocos-version`。

## 当前功能

### 1. 映射资源名称路径

命令文案为 `映射资源名称路径`，启用扩展后可在编辑器菜单中找到。执行后会：

- 从项目 `assets/` 开始递归扫描 Bundle 目录
- 根据 `.meta` 中的 Bundle 标记识别资源包
- 执行 [src/business/BundlePath.ts](/E:/project/CCProject/Journey-to-the-West-Battle-Flag/extensions/cc_extensions_tools_universal/src/business/BundlePath.ts) 中的生成逻辑，输出 `assets/scripts/BundleAssetsConfig.ts`
- 自动调用资源刷新接口，让生成文件进入资源数据库

生成文件包含：

- `BundleName`：Bundle 名称枚举
- `AssetPath`：资源路径映射对象
- `GetDirPath()`：从映射对象反推目录
- `GetFileName()`：从资源路径提取文件名

### 2. 资源目录说明 Inspector

选中资源文件夹时，扩展会查找同目录下的隐藏 Markdown 文件：

```text
.{文件夹名}.md
```

例如选中 `assets/game/scripts`，则会读取：

```text
assets/game/scripts/.scripts.md
```

只要文件存在，其内容就会显示在 Inspector 的“资源目录说明”区块中。

## 目录概览

```text
src/
├── main.ts                 # 扩展主入口，仅注册 bundlePath 消息
├── business/BundlePath.ts  # Bundle 路径映射实现
├── asset_directory/index.ts# 资源目录说明 Inspector
├── core/                   # 版本检测、工厂、基类、接口
├── adapters/               # v2 / v3 适配器
├── panels/default/index.ts # 示例面板实现（当前未接入菜单）
└── tools/Tools.ts          # 编辑器侧通用工具类
```

## 构建命令

```bash
npm run build      # 同时编译 v2 与 v3
npm run build:v2   # 仅编译 v2
npm run build:v3   # 仅编译 v3
npm run watch      # TypeScript watch
```

## 说明

- 仓库当前不再通过 `asset-db.mount` 或自动双向同步挂载运行时 `assets/` 工具脚本
- [使用说明.md](/E:/project/CCProject/Journey-to-the-West-Battle-Flag/extensions/cc_extensions_tools_universal/使用说明.md) 提供更完整的安装、功能与二次开发说明
