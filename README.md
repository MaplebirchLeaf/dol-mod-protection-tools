# DOL Mod Protection Tools

秋枫白桦框架配套的 DOL 模组加密工具。

本工具需要搭配 [秋枫白桦框架](https://github.com/MaplebirchLeaf/SCML-DOL-maplebirchFramework) 使用。

## 工具

发布包包含：

- `[DMPT]-keygen.exe`：生成公私钥和 `auth.json`。
- `[DMPT]-credential.exe`：使用私钥签发授权凭证。
- `[DMPT]-modpack.exe`：将模组 `.zip` 转换为加密壳 `.modpack`。

## 工作流程

1. 双击 `[DMPT]-keygen.exe`，生成 `keys.json` 和 `auth.json`。
2. 双击 `[DMPT]-credential.exe`，用私钥生成授权凭证。
3. 将 `auth.json` 放入原始模组 zip 根目录，或在转换时通过参数提供。
4. 将模组 `.zip` 拖到 `[DMPT]-modpack.exe` 上，生成加密后的 `.modpack`。

转换后的 `.modpack` 是一个壳模组：

- 壳包根目录只包含假的 `boot.json`、earlyload 解密器和若干 `.crypt` 分片。
- 原始 `boot.json`、`auth.json`、脚本、资源等都会进入加密 payload。
- 玩家首次加载时输入授权凭证，框架验证通过后解密真实模组，并通过 ModLoader 懒加载接口注入。
- 凭证验证失败或关闭弹窗时，框架会禁用该加密模组。

模组名始终读取 zip 内的 `boot.json.name`。
发布包不包含源代码。凭证只能由私钥签发，不能从凭证反推出私钥。

## auth.json

`auth.json` 用于声明加密模组的公钥和凭证校验信息。转换后它会被放入加密 payload，不会直接暴露在壳包根目录。

完整示例：

```json
{
  "key": "main",
  "subject": "MyMod",
  "name": "My Mod",
  "publicKey": "BASE64_SPKI_PUBLIC_KEY",
  "prompt": {
    "title": "模组加密",
    "label": "My Mod 凭证",
    "placeholder": "输入凭证",
    "hint": "请输入作者提供的授权凭证。"
  },
  "date": {
    "timezone": "Asia/Hong_Kong",
    "graceDays": 0
  }
}
```

字段说明：

| 字段 | 必填 | 说明 |
| :--- | :--- | :--- |
| `key` | 是 | 凭证标识，必须和签发凭证时的 `key` 一致。 |
| `publicKey` | 是 | 用于校验凭证签名的公钥，支持 SPKI Base64 字符串或 WebCrypto JWK 对象。 |
| `subject` | 否 | 凭证绑定对象，默认使用 `boot.json.name`。签发凭证时的 `subject` 必须与这里一致。 |
| `name` | 否 | 弹窗中显示的模组名称。 |
| `prompt.title` | 否 | 弹窗标题。 |
| `prompt.label` | 否 | 输入框上方文案。 |
| `prompt.placeholder` | 否 | 输入框占位文案。 |
| `prompt.hint` | 否 | 弹窗提示文案。 |
| `date.timezone` | 否 | 日期凭证使用的时区，默认 `UTC`。 |
| `date.graceDays` | 否 | 日期宽限天数，默认 `0`。 |

最小配置：

```json
{
  "key": "main",
  "publicKey": "BASE64_SPKI_PUBLIC_KEY"
}
```

如果启用了 `date` 配置，凭证必须包含 `date` 字段。若凭证包含 `expiresAt`，过期后会被拒绝。

## 说明

加密壳用于减少模组内容被直接查看的可能性，但它不能提供绝对保护。框架和前端运行环境都是玩家可访问的，真正敏感的私钥必须只保存在作者本地，不要放进模组或发布包。
