<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "90d0d072cf26ccc1f271a580d3e45d70",
  "translation_date": "2025-05-27T02:40:41+00:00",
  "source_file": "CONTRIBUTING.md",
  "language_code": "zh"
}
-->
# 贡献指南

本项目欢迎贡献和建议。大多数贡献需要您同意一份贡献者许可协议（CLA），声明您有权并实际授予我们使用您贡献内容的权利。详情请访问 [https://cla.opensource.microsoft.com](https://cla.opensource.microsoft.com)

当您提交拉取请求（PR）时，CLA 机器人会自动判断您是否需要提供 CLA，并相应地标记 PR（例如，状态检查、评论）。只需按照机器人提供的指示操作即可。您在所有使用我们 CLA 的仓库中只需完成一次此操作。

## 行为准则

本项目采用了 [Microsoft 开源行为准则](https://opensource.microsoft.com/codeofconduct/)。  
欲了解更多信息，请阅读 [行为准则常见问题](https://opensource.microsoft.com/codeofconduct/faq/) 或通过邮箱 [opencode@microsoft.com](mailto:opencode@microsoft.com) 联系我们，提出任何额外的问题或建议。

## 创建 Issue 的注意事项

请勿为一般支持问题创建 GitHub Issue，因为 GitHub 列表应主要用于功能请求和错误报告。这样我们可以更方便地跟踪代码中的实际问题或错误，并将一般讨论与代码问题区分开。

## 如何贡献

### 拉取请求指南

提交 Phi-3 CookBook 仓库的拉取请求时，请遵循以下指南：

- **Fork 仓库**：在进行修改前，请务必先将仓库 Fork 到您自己的账户。

- **分开提交拉取请求（PR）**：
  - 不同类型的更改请分别提交拉取请求。例如，错误修复和文档更新应分别提交。
  - 拼写错误修正和小幅文档更新可适当合并为一个 PR。

- **处理合并冲突**：如果您的拉取请求显示有合并冲突，请先更新本地 `main` 分支，使其与主仓库保持同步，然后再进行修改。

- **翻译提交**：提交翻译 PR 时，请确保翻译文件夹包含原始文件夹中所有文件的翻译。

### 撰写指南

为确保所有文档的一致性，请遵守以下指南：

- **URL 格式**：所有 URL 应用方括号包裹，后跟圆括号，中间无多余空格。例如：`[example](https://www.microsoft.com)`。

- **相对链接**：当前目录下的文件或文件夹使用 `./`，父目录下的使用 `../`。例如：`[example](../../path/to/file)` 或 `[example](../../../path/to/file)`。

- **非国家/地区特定语言环境**：确保链接中不包含国家/地区特定的语言环境。例如，避免使用 `/en-us/` 或 `/en/`。

- **图片存储**：所有图片应存放在 `./imgs` 文件夹。

- **描述性图片命名**：图片名称应使用英文字符、数字和短横线，具有描述性。例如：`example-image.jpg`。

## GitHub 工作流

提交拉取请求时，以下工作流将自动触发以验证更改。请按照以下说明确保您的拉取请求能通过工作流检查：

- [检查相对路径是否损坏](../..)
- [检查 URL 是否包含语言环境](../..)

### 检查相对路径是否损坏

此工作流确保文件中的所有相对路径均正确。

1. 使用 VS Code 执行以下操作，确保链接正常：
    - 将鼠标悬停在文件中的任意链接上。
    - 按 **Ctrl + 点击** 跳转链接。
    - 如果本地点击链接无效，则该链接会触发工作流错误，且在 GitHub 上也无法正常使用。

1. 使用 VS Code 提供的路径建议修复此问题：
    - 输入 `./` 或 `../`。
    - VS Code 会根据输入提示您选择可用选项。
    - 点击所需文件或文件夹，确保路径正确。

添加正确的相对路径后，保存并推送更改。

### 检查 URL 是否包含语言环境

此工作流确保所有网页 URL 不包含国家/地区特定的语言环境。由于本仓库面向全球用户，确保 URL 不含本地语言环境非常重要。

1. 验证 URL 是否包含国家/地区语言环境：
    - 检查 URL 中是否有 `/en-us/`、`/en/` 或其他语言环境标识。
    - 如果 URL 中没有这些标识，则通过此检查。

1. 修复方法：
    - 打开工作流指出的文件路径。
    - 从 URL 中移除国家/地区语言环境。

移除语言环境后，保存并推送更改。

### 检查损坏的 URL

此工作流确保文件中的所有网页 URL 都能正常访问并返回 200 状态码。

1. 验证 URL 是否正常：
    - 检查文件中 URL 的状态。

2. 修复损坏的 URL：
    - 打开包含损坏 URL 的文件。
    - 将 URL 更新为正确地址。

修复后，保存并推送更改。

> [!NOTE]  
> 可能会出现 URL 检查失败但链接实际可访问的情况，原因包括：  
>  
> - **网络限制**：GitHub Actions 服务器可能有限制，导致无法访问某些 URL。  
> - **超时问题**：响应时间过长的 URL 可能触发超时错误。  
> - **临时服务器问题**：服务器偶尔停机或维护，导致验证时 URL 暂时不可用。

**免责声明**：  
本文件由 AI 翻译服务 [Co-op Translator](https://github.com/Azure/co-op-translator) 翻译而成。尽管我们力求准确，但请注意，自动翻译可能包含错误或不准确之处。原始文件的母语版本应被视为权威来源。对于关键信息，建议采用专业人工翻译。因使用本翻译而产生的任何误解或曲解，我们概不负责。