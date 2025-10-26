# FOSSBilling SLD长度限制器

## 功能概述

此功能为FOSSBilling系统添加了二级域名（SLD）长度限制验证，防止用户注册长度不符合要求的域名。

## 实现位置

主要实现在 `library/FOSSBilling/Validate.php` 文件中的 `isSldValid()` 方法。

## 配置参数

### 硬编码配置

在 `Validate.php` 文件中直接配置以下参数：

```php
// SLD长度限制开关
$sldLengthRestrictionEnabled = true; // true=启用限制，false=禁用限制

// SLD长度限制
$minLength = 3;  // 最小字符数
$maxLength = 63; // 最大字符数
```

### 参数说明

- **$sldLengthRestrictionEnabled**: 布尔值，控制是否启用SLD长度限制
- **$minLength**: 整数，SLD的最小字符数（默认：3）
- **$maxLength**: 整数，SLD的最大字符数（默认：63）

## 验证规则

当启用SLD长度限制时，系统会检查：

1. **最小长度**：SLD长度必须 ≥ $minLength
2. **最大长度**：SLD长度必须 ≤ $maxLength

## 错误提示

- **长度过短**："域名长度不能少于 :min 个字符"
- **长度过长**："域名长度不能超过 :max 个字符"

## 使用示例

```php
// 启用长度限制（默认）
$sldLengthRestrictionEnabled = true;
$minLength = 3;
$maxLength = 63;

// 或者禁用长度限制
$sldLengthRestrictionEnabled = false;
```

## 影响范围

此限制会影响以下操作：

- 域名注册
- 域名转移
- 自有域名添加
- 域名验证检查

## 注意事项

1. 此限制仅适用于SLD（二级域名），不影响TLD（顶级域名）
2. 限制对Punycode编码的域名不生效
3. 修改配置后需要清除系统缓存
4. 限制仅对新建域名生效，不影响现有域名

## 测试建议

建议在启用此功能后测试以下场景：

- 注册2个字符的域名（应该失败）
- 注册3个字符的域名（应该成功）
- 注册63个字符的域名（应该成功）
- 注册64个字符的域名（应该失败）

## 故障排除

如果遇到问题，请检查：

1. Validate.php文件中的配置是否正确
2. PHP错误日志中是否有相关错误信息
3. 系统缓存是否已清除
4. 域名服务模块是否正常工作