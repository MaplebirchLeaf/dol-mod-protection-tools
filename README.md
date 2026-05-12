# DOL Mod Protection Tools

秋枫白桦框架配套的 DOL 模组保护工具。

本工具需要搭配 [秋枫白桦框架](https://github.com/MaplebirchLeaf/SCML-DOL-maplebirchframework) 使用。

## 工具

发布包包含：

- `[DMPT]-keygen.exe`：生成公私密钥和 `auth.json`。
- `[DMPT]-credential.exe`：使用私钥签发授权凭证。
- `[DMPT]-modpack.exe`：为模组 `.zip` 注入校验脚本，并转换为 `.modpack`。

## 使用

1. 双击 `[DMPT]-keygen.exe`，生成 `keys.json` 和 `auth.json`。
2. 双击 `[DMPT]-credential.exe`，用私钥生成授权凭证。
3. 将模组 `.zip` 拖到 `[DMPT]-modpack.exe` 上，生成 `.modpack`。

模组名始终读取 zip 内的 `boot.json.name`，中文名称会原样使用。

发布包不包含源代码。

凭证只能由私钥签发，不能从凭证反推出私钥。

## auth.json

`auth.json` 放进模组 zip 后会被框架读取，用于验证玩家输入的授权凭证。完整配置结构如下：

```json
{
  "key": "main",
  "subject": "MyMod",
  "name": "My Mod",
  "publicKey": "BASE64_SPKI_PUBLIC_KEY",
  "prompt": {
    "title": "授权验证",
    "label": "请输入 My Mod 的授权凭证",
    "placeholder": "maplebirch-auth.<payload>.<signature>",
    "hint": "请粘贴作者提供的完整授权凭证。"
  },
  "date": {
    "timezone": "Asia/Hong_Kong",
    "graceDays": 0
  }
}
```

字段说明：

- `key`：必填。授权 key，必须和签发凭证时的 `key` 完全一致。
- `subject`：可选。凭证绑定对象；未填写时使用模组名。签发凭证时的 `subject` 必须与这里一致。
- `name`：可选。显示给用户看的模组名称；未设置 `subject` 时也可作为签发工具读取的默认模组名。
- `publicKey`：必填。公钥，可以是 `[DMPT]-keygen.exe` 生成的 SPKI Base64 字符串，也可以是 WebCrypto JWK 对象。
- `prompt.title`：可选。授权弹窗标题。
- `prompt.label`：可选。授权弹窗输入框上方文案。
- `prompt.placeholder`：可选。输入框占位文案。
- `prompt.hint`：可选。授权弹窗提示文案。
- `date.timezone`：可选。日期凭证使用的时区，默认 `UTC`。如果凭证包含 `date`，工具会按这个时区比较当天日期。
- `date.graceDays`：可选。日期宽限天数，默认 `0`。例如设为 `1` 时，昨天、今天、明天的日期凭证都可通过。

最小配置如下：

```json
{
  "key": "main",
  "name": "My Mod",
  "publicKey": "BASE64_SPKI_PUBLIC_KEY"
}
```

如果启用了 `date` 配置，凭证必须包含 `date` 字段；如果凭证包含 `expiresAt`，过期后会拒绝解锁。
