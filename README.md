# Maplebirch Author Tools

Maplebirch Framework 配套的模组作者工具包。

用于：

- 生成授权公私钥
- 生成按日期签发的授权凭证
- 为模组 zip 注入校验脚本
- 将模组 zip 转换为 `.modpack`

## 工具

发布包包含：

- `maplebirch-keygen.exe`
- `maplebirch-credential.exe`
- `maplebirch-modpack.exe`

## 使用

1. 双击 `maplebirch-keygen.exe`，生成密钥和 `auth.json`。
2. 双击 `maplebirch-credential.exe`，用私钥生成授权凭证。
3. 将模组 `.zip` 拖到 `maplebirch-modpack.exe` 上，生成 `.modpack`。

模组名始终读取 zip 内的 `boot.json.name`。

发布包不包含源代码。

凭证只能由私钥签发，不能从凭证反推出私钥。
