# 【Windows 10 专属】完整流程：将核酸检测系统打包为 `tianhunat` CLI 工具并发布

本流程专为 Windows 10 系统设计，涵盖 **GitHub 仓库准备 → CLI 包开发 → npm 发布 → 用户使用** 全步骤，所有操作均基于 Windows 10 终端（CMD/PowerShell），每一步含具体指令、代码和截图级说明，零 Mac 相关内容。

## 前置准备（Windows 10 环境）

### 1.1 安装必备工具

1. **安装 Node.js（含 npm）**

    - 访问 [Node.js 官网](https://nodejs.org/)，下载 **LTS 版本**（推荐 v18.x，自带 npm）

    - 双击安装包，勾选「Add to PATH」（自动添加环境变量），其余默认下一步即可

    - 验证安装：打开 CMD 终端，输入 `node -v` 和 `npm -v`，能显示版本号即成功

2. **注册必要账号**

    - 注册 [GitHub 账号](https://github.com/)：存放 4 套系统源码（需公开仓库）

    - 注册 [npm 账号](https://www.npmjs.com/)：发布 CLI 工具（需验证邮箱）

3. **安装 Git（可选，辅助上传源码）**

    - 访问 [Git 官网](https://git-scm.com/)，下载 Windows 版本

    - 安装时默认下一步，最后勾选「Git Bash Here」（方便右键打开终端）

### 1.2 整理系统源码并上传 GitHub

假设你的 4 套系统已在本地（如 `D:\核酸检测系统\` 目录），需分别上传到 GitHub 4 个公开仓库：

|系统类型|本地目录示例|GitHub 仓库名（建议）|仓库地址示例|
|---|---|---|---|
|管理系统|D:\核酸检测系统\admin|tianhunat-admin|[https://github.com/](https://github.com/)你的用户名/tianhunat-admin|
|查询系统|D:\核酸检测系统\query|tianhunat-query|[https://github.com/](https://github.com/)你的用户名/tianhunat-query|
|预约系统|D:\核酸检测系统\reserve|tianhunat-reserve|[https://github.com/](https://github.com/)你的用户名/tianhunat-reserve|
|大数据看板|D:\核酸检测系统\dashboard|tianhunat-dashboard|[https://github.com/](https://github.com/)你的用户名/tianhunat-dashboard|
#### 上传源码到 GitHub 步骤（Windows 10 操作）

以「管理系统」为例：

1. 打开 GitHub 官网，登录后点击右上角「+」→「New repository」

2. 填写仓库信息：

    - Repository name：`tianhunat-admin`

    - Description：核酸检测管理系统

    - Visibility：选「Public」（必须公开，否则用户无法下载）

    - 勾选「Add a README file」

    - 点击「Create repository」

3. 本地上传（用 Git Bash）：

    - 右键点击本地 `admin` 目录 →「Git Bash Here」

    - 执行以下命令（替换为你的仓库地址和信息）：

        ```Bash
        
        git init
        git add .
        git commit -m "初始化管理系统源码"
        git branch -M main
        git remote add origin https://github.com/你的用户名/tianhunat-admin.git
        git push -u origin main
        ```

    - 弹出登录框时，输入 GitHub 账号密码（或用 Token 登录，GitHub 已不支持密码直接推送，需提前在 GitHub 生成 Token：Settings → Developer settings → Personal access tokens → Generate new token，勾选 repo 权限）

4. 其余 3 个系统按同样步骤上传，确保所有仓库均为公开。

---

## 步骤 1：在 Windows 10 本地创建 CLI 工具项目

### 1.1 打开 Windows 终端（CMD 或 PowerShell）

- 按下 `Win + R`，输入 `cmd`，回车打开 CMD（推荐，操作更简单）

- 或在开始菜单搜索「Windows PowerShell」打开

### 1.2 创建并进入 CLI 项目目录

在 CMD 中执行以下命令（按自己习惯选择目录，示例用 D 盘）：

```Plain Text

:: 进入 D 盘
D:

:: 创建 CLI 项目文件夹并进入
mkdir tianhunat-cli
cd tianhunat-cli
```

### 1.3 初始化 package.json 文件

执行以下命令，按提示输入信息（一路回车即可，后续可修改）：

```Plain Text

npm init -y
```

执行后，`tianhunat-cli` 目录下会生成 `package.json` 文件。

### 1.4 安装 CLI 核心依赖包

在当前目录（`tianhunat-cli`）执行以下命令，安装 4 个必需依赖：

```Plain Text

npm install commander download-git-repo shelljs chalk --save
```

- 依赖说明：

    - `commander`：解析命令行参数（如 `init --admin`）

    - `download-git-repo`：从 GitHub 下载源码

    - `shelljs`：执行终端命令（如 `npm install`）

    - `chalk`：美化命令行输出颜色（红/绿/蓝提示）

- 安装成功后，`tianhunat-cli` 目录会新增 `node_modules` 文件夹和 `package-lock.json` 文件。

### 1.5 创建 CLI 入口文件（关键步骤）

#### 1.5.1 创建 bin 文件夹和 cli.js 文件

在 CMD 中执行以下命令（Windows 10 新建文件指令）：

```Plain Text

:: 在 tianhunat-cli 目录下创建 bin 文件夹
mkdir bin

:: 进入 bin 文件夹
cd bin

:: 创建 cli.js 文件（Windows 新建空文件指令）
type nul > cli.js
```

#### 1.5.2 编写 cli.js 代码

用记事本或 VS Code 打开 `bin/cli.js` 文件（推荐 VS Code，编辑代码更方便），粘贴以下代码（**必须替换 ** **`你的GitHub用户名`** ** 为真实值**）：

```JavaScript

#!/usr/bin/env node
// 第一行必须写这个，告诉 Windows 系统这是 Node.js 可执行文件

// 导入依赖包
const { Command } = require('commander');
const download = require('download-git-repo');
const shell = require('shelljs');
const chalk = require('chalk');

// 核心配置：GitHub 仓库地址映射（替换成你的 GitHub 用户名！）
const REPO_MAP = {
  admin: '你的GitHub用户名/tianhunat-admin',    // 管理系统仓库
  query: '你的GitHub用户名/tianhunat-query',    // 查询系统仓库
  reserve: '你的GitHub用户名/tianhunat-reserve',// 预约系统仓库
  dashboard: '你的GitHub用户名/tianhunat-dashboard'// 看板系统仓库
};

// 初始化命令行工具
const program = new Command();
program
  .name('tianhunat')
  .description('天互核酸检测系统 Windows 一键安装工具')
  .version('1.0.0', '-v, --version') // 版本号，后续可修改
  .usage('init [--admin/--query/--reserve/--dashboard]'); // 使用提示

// 核心命令：init（下载源码+安装依赖）
program
  .command('init')
  .description('下载指定核酸检测系统并自动安装依赖')
  .option('--admin', '安装【管理系统】')
  .option('--query', '安装【查询系统】')
  .option('--reserve', '安装【预约系统】')
  .option('--dashboard', '安装【大数据看板】')
  .action((options) => {
    // 1. 判断用户选择的系统
    let selectedSystem = null;
    for (const key in options) {
      if (options[key]) {
        selectedSystem = key;
        break;
      }
    }

    // 2. 未选择系统则提示错误
    if (!selectedSystem) {
      console.log(chalk.red('❌ 错误：请指定要安装的系统！'));
      console.log(chalk.yellow('✅ 正确示例：'));
      console.log(chalk.yellow('   tianhunat init --admin   （安装管理系统）'));
      console.log(chalk.yellow('   tianhunat init --query   （安装查询系统）'));
      return;
    }

    // 3. 获取仓库地址和本地项目文件夹名
    const repoUrl = REPO_MAP[selectedSystem];
    const projectDir = `tianhunat-${selectedSystem}`; // 本地生成的文件夹名

    // 4. 开始下载源码
    console.log(chalk.blue(`🚀 正在下载【${selectedSystem}】系统源码...`));
    console.log(chalk.gray(`📦 仓库地址：https://github.com/${repoUrl}`));
    
    download(
      repoUrl,          // GitHub 仓库地址
      projectDir,       // 本地文件夹名
      { clone: true },  // 使用 git clone 方式下载（更稳定）
      (err) => {
        if (err) {
          console.log(chalk.red(`❌ 下载失败！错误原因：${err.message}`));
          console.log(chalk.gray('💡 排查建议：'));
          console.log(chalk.gray('   1. 检查 GitHub 仓库是否公开'));
          console.log(chalk.gray('   2. 检查网络是否能访问 GitHub'));
          console.log(chalk.gray('   3. 确认仓库地址是否正确'));
          return;
        }

        // 5. 下载成功，进入项目目录安装依赖
        console.log(chalk.green(`✅ 源码下载完成！本地目录：./${projectDir}`));
        console.log(chalk.blue('🔧 正在自动安装依赖（可能需要 3-10 分钟，请耐心等待）...'));

        // 检查 npm 是否可用（Windows 环境兼容处理）
        if (!shell.which('npm')) {
          console.log(chalk.red('❌ 错误：未检测到 npm！请先安装 Node.js 并配置环境变量'));
          return;
        }

        // 进入项目目录（Windows 路径兼容）
        shell.cd(projectDir);

        // 执行 npm install 安装依赖（显示详细日志）
        const installProcess = shell.exec('npm install', { async: true, stdio: 'inherit' });

        // 监听依赖安装结果
        installProcess.on('exit', (code) => {
          if (code === 0) {
            // 安装成功提示
            console.log('\n' + chalk.green('🎉 依赖安装完成！'));
            console.log(chalk.yellow('\n👉 下一步操作（复制执行）：'));
            console.log(chalk.yellow(`   1. 进入项目目录：cd ${projectDir}`));
            console.log(chalk.yellow(`   2. 启动系统：npm run start`));
            console.log(chalk.gray(`\n💡 启动后，浏览器访问 http://localhost:3000（具体端口以系统配置为准）`));
          } else {
            // 安装失败提示
            console.log(chalk.red('❌ 依赖安装失败！'));
            console.log(chalk.gray('💡 手动安装建议：'));
            console.log(chalk.gray(`   1. 打开 CMD，进入目录：cd ${projectDir}`));
            console.log(chalk.gray('   2. 手动执行：npm install'));
          }
        });
      }
    );
  });

// 解析用户输入的命令行参数
program.parse(process.argv);
```

### 1.6 配置 package.json（关键：映射命令）

用 VS Code 或记事本打开 `tianhunat-cli` 根目录的 `package.json` 文件，替换为以下内容（**替换 ** **`你的npm用户名`** ** 和 ** **`你的邮箱`**）：

```JSON

{
  "name": "tianhunat",
  "version": "1.0.0",
  "description": "天互核酸检测系统 Windows 一键安装 CLI 工具",
  "main": "bin/cli.js",
  "bin": {
    "tianhunat": "./bin/cli.js"
  },
  "keywords": ["核酸检测系统", "windows", "cli", "nodejs", "一键安装"],
  "author": "你的npm用户名 <你的邮箱>",
  "license": "MIT",
  "dependencies": {
    "chalk": "^5.3.0",
    "commander": "^12.0.0",
    "download-git-repo": "^3.0.2",
    "shelljs": "^0.8.5"
  },
  "engines": {
    "node": ">=16.0.0"
  }
}
```

- 关键说明：`bin` 字段将 `tianhunat` 命令映射到 `./bin/cli.js`，用户全局安装后可直接在 CMD 中输入 `tianhunat`。

---

## 步骤 2：Windows 10 本地测试 CLI 工具（避免发布后出错）

### 2.1 全局链接本地 CLI 包

在 `tianhunat-cli` 根目录（CMD 终端）执行以下命令，将本地包链接为全局命令：

```Plain Text

npm link
```

- 执行成功后，会提示 `C:\Users\你的用户名\AppData\Roaming\npm\node_modules\tianhunat -> D:\tianhunat-cli`

- 此时 Windows 系统已识别 `tianhunat` 命令，可在任意目录执行。

### 2.2 测试核心命令（以管理系统为例）

1. 打开 **新的 CMD 终端**（确保环境变量生效）

2. 选择一个测试目录（如桌面）：

    ```Plain Text
    
    :: 进入桌面
    cd Desktop
    ```

3. 执行安装命令：

    ```Plain Text
    
    tianhunat init --admin
    ```

4. 正常流程会显示：

    - 蓝色提示「正在下载系统源码」

    - 下载完成后显示绿色提示「源码下载完成」

    - 自动执行 `npm install` 安装依赖

    - 最终显示黄色启动步骤提示

### 2.3 测试问题排查（Windows 常见问题）

|问题现象|排查方法|
|---|---|
|输入 `tianhunat` 提示「不是内部或外部命令」|1. 重新执行 `npm link`；2. 检查 Node.js 环境变量是否配置（重启 CMD 重试）|
|下载失败提示「git is not installed」|安装 Git 并勾选「Add to PATH」，重启 CMD 终端|
|依赖安装卡住|切换 npm 源为淘宝镜像：`npm config set registry https://registry.npmmirror.com/`|
### 2.4 测试成功后取消本地链接（可选）

测试无误后，执行以下命令取消全局链接（避免影响后续发布）：

```Plain Text

npm unlink -g tianhunat
```

---

## 步骤 3：发布 CLI 包到 npm（供用户下载）

### 3.1 切换 npm 源为官方源（必须）

Windows 系统默认可能是淘宝镜像，需切换回 npm 官方源才能发布，执行：

```Plain Text

npm config set registry https://registry.npmjs.org/
```

### 3.2 登录 npm 账号

在 `tianhunat-cli` 根目录执行：

```Plain Text

npm login
```

按提示依次输入：

1. Username：你的 npm 用户名

2. Password：你的 npm 密码（输入时不显示，直接回车即可）

3. Email：你的 npm 注册邮箱

4. OTP：邮箱收到的验证码（输入后回车）

- 登录成功会提示「Logged in as 你的用户名 on [https://registry.npmjs.org/](https://registry.npmjs.org/)」

### 3.3 验证包名是否可用

确保 `package.json` 中的 `name` 为 `tianhunat`，且未被他人占用：

- 访问 [npm 官网搜索](https://www.npmjs.com/search?q=tianhunat)

- 若未搜索到结果，说明包名可用；若已被占用，修改 `package.json` 的 `name` 字段（如 `tianhunat-nucleic-acid`）

### 3.4 发布包到 npm

执行发布命令：

```Plain Text

npm publish
```

- 发布成功会提示 `+ tianhunat@1.0.0`

- 发布后可访问 `https://www.npmjs.com/package/tianhunat` 查看你的包

### 3.5 切换回淘宝源（可选，加快后续下载）

```Plain Text

npm config set registry https://registry.npmmirror.com/
```

---

## 步骤 4：Windows 10 用户使用流程（给用户的说明书）

发布成功后，Windows 10 用户只需 3 步即可使用你的系统：

### 4.1 用户安装 CLI 工具

打开 CMD 终端，执行：

```Plain Text

npm install -g tianhunat
```

- 若安装慢，可先切换淘宝源：`npm config set registry https://registry.npmmirror.com/`

### 4.2 用户安装指定系统

|系统类型|用户执行命令|
|---|---|
|管理系统|`tianhunat init --admin`|
|查询系统|`tianhunat init --query`|
|预约系统|`tianhunat init --reserve`|
|大数据看板|`tianhunat init --dashboard`|
### 4.3 用户启动系统

以管理系统为例，用户执行：

```Plain Text

:: 进入项目目录
cd tianhunat-admin

:: 启动系统
npm run start
```

- 启动后，用户打开浏览器访问 `http://localhost:3000`（具体端口以你的系统配置为准）

---

## 步骤 5：后续维护（Windows 10 环境）

### 5.1 更新 CLI 工具版本

1. 修改 `package.json` 的 `version` 字段（如从 1.0.0 改为 1.0.1）

2. 执行发布命令：

    ```Plain Text
    
    npm publish
    ```

3. 用户更新命令：`npm update -g tianhunat`

### 5.2 修复 Bug 或新增功能

1. 本地修改 `cli.js` 代码

2. 测试无误后，更新 `package.json` 版本号

3. 重新执行 `npm publish`

---

## Windows 10 专属注意事项

1. **文件路径兼容**：代码中已处理 Windows 路径分隔符（`\` 和 `/`），无需额外修改

2. **Git 依赖**：用户下载源码时需安装 Git（若用户未安装，CLI 会提示错误并给出安装建议）

3. **Node.js 版本**：要求用户安装 Node.js v16+，避免兼容性问题

4. **npm 权限**：若执行 `npm install -g` 提示权限不足，右键 CMD 选择「以管理员身份运行」

5. **GitHub 访问**：若用户下载源码失败，建议用户开启科学上网或检查网络设置

我可以帮你生成一份 **Windows 用户专属使用说明书（TXT 格式）**，包含安装、启动、常见问题排查，用户可直接查看，需要吗？
> （注：文档部分内容可能由 AI 生成）