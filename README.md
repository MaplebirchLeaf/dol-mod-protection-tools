# Maplebirch Author Tools

秋枫白桦框架配套的模组作者工具。

本工具需要搭配 [SCML-DOL-maplebirchFramework 秋枫白桦框架](https://github.com/MaplebirchLeaf/SCML-DOL-maplebirchframework) 使用。

## 工具

发布包包含：

- `maplebirch-keygen.exe`：生成公私密钥和 `auth.json`。
- `maplebirch-credential.exe`：使用私钥签发按日期生成的授权凭证。
- `maplebirch-modpack.exe`：为模组 `.zip` 注入校验脚本，并转换为 `.modpack`。

## 使用

1. 双击 `maplebirch-keygen.exe`，生成密钥和 `auth.json`。
2. 双击 `maplebirch-credential.exe`，用私钥生成授权凭证。
3. 将模组 `.zip` 拖到 `maplebirch-modpack.exe` 上，生成 `.modpack`。

模组名始终读取 zip 内的 `boot.json.name`，中文名称会原样使用。

发布包不包含源代码。

凭证只能由私钥签发，不能从凭证反推出私钥。
