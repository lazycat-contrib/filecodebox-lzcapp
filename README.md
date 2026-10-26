# filecodebox-lzcapp

[FileCodeBox](https://github.com/vastsa/filecodebox) 的 LazyCat LPK 打包 —— 轻量自托管文件分享工具（匿名口令取件）。

## 形态

- 单容器镜像：`lanol/filecodebox`，端口 12345
- 镜像交付：mirror 模式（`docker.1ms.run/lanol/filecodebox:{tag}`），digest 校验开启
- 版本发现：Docker Hub 严格三段式 SemVer tag，排除两段式别名（2.8/2.7）、edge-\* 构建、master 与 latest
- 仅喵喵商店；仅 PC 平台

## 使用方式

- **普通用户**：打开首页上传文件即得口令，对方凭口令取件，无需注册
- **管理员**：首次访问走初始化向导（`/setup`）设置管理员密码，之后从 `/admin` 登录管理

部署参数 `admin_password` 自动生成 20 位密码，初始化向导与管理登录页都会自动填充。

## 持久化

`/lzcapp/var/data` → `/app/data`（SQLite、上传文件、配置）

## 可选：本地分享

上游支持把 NAS 上已有目录挂到 `/app/data/local` 后台生成本地提取码（无需上传）。
本包未预置该挂载，如需可自行在 manifest 里加一行。

## 结构

- `package.yml` — LPK v2 包元数据 + 平台限制
- `lzc-manifest.yml` — 单服务、免密登录注入、文件选择器注入
- `lzc-deploy-params.yml` — 管理员密码部署参数
- `content/lazycat-injects/` — 文件选择器拦截脚本
- `.github/lazycat-action.yml` — 镜像版本源，mirror 交付，仅私有商店
- `.github/workflows/lazycat.yml` — 每日自动跟版 + 发布喵喵商店
