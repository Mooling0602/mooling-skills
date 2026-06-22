# 关于 MCDReforged

MCDReforged (MCDR, 下同) 是一个管理 Minecraft 服务器的工具，拥有自定义插件系统。完全无需修改 Minecraft 服务器本身。

MCDR 拥有丰富的 社区插件生态。小至计算器、高亮玩家、b 站弹幕姬，大至操控计分板、管理结构文件、自助备份回档，都可以通过 MCDR 及相配套的插件实现，且支持热重载，无需关闭服务器即可更新插件。

在兼容性方面，MCDR 支持大多数流行服务端 (Vanilla, Fabric, Spigot, Paper 等)，支持多平台 (Windows, Linux, Mac 等)。

本文档将指导你设计 MCDReforged 的插件代码，以及代码调试、测试工作。

关于更多信息，你需要查看此[官方文档](https://docs.mcdreforged.com/zh-cn/latest/index.html)。

> 推荐使用 `webfetch` 工具获取文档内容，或通过 `curl` CLI 工具来访问并自行解析文档。

## 安装此技能

参见[README](README.md)。

## 接口使用

MCDR 推荐开发者使用 `from mcdreforged.api.all import PluginServerInterface` 这样的方式导入常用的公共接口。如果插件专为 2.15 及以上版本的 MCDReforged 设计的话，还可以直接从 `mcdreforged` 主模块导入。

为了尽量提高插件的跨 MCDR 版本兼容性，推荐从 `mcdreforged.api.all` 模块中导入接口，但大多数开发环境下，在安装较新版本的 MCDReforged 时会自动从 `mcdreforged` 中导入接口，如果发现这种情况，推荐进行修正。

## 翻译

MCDR 在 `PluginServerInterface` 中提供名为 `rtr` 的富文本 (即 RText) 翻译接口。部分插件项目内，可能也会提供名为 `tr` 的自定义翻译封装。

插件的翻译文件一般存放在源码目录下的 `lang` 目录中，并与插件元数据 (`mcdreforged.plugin.json`) 同级。

如果一个插件项目中带有翻译，在检查翻译时，你需要比对中英文的 yml 文件是否相互对应，源码中引用的翻译条目和接口使用方式是否正确。

部分插件通过一些间接手段可将翻译文件解压到配置目录下或其他地方，需要根据实际的项目代码具体分析。
