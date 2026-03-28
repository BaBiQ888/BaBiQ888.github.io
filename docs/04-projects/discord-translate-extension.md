---
title: "项目开源：Discord 实时翻译 Chrome 浏览器插件"
date: 2026-03-28
category: "Projects"
---

# 项目开源：Discord 实时翻译 Chrome 浏览器插件

如果你经常混迹于各类 Web3 或开源国际社区的 Discord 频道中，不可避免地会遇到来自世界各地的语言。对于很多人而言，如果不能在应用内直接查看大段讨论的翻译信息，体验会变得相当割裂。为了解决日常刷 Discord 社区的语言阅读障碍，我开发了一款可以在原生 Web 端运行翻译的插件 —— **Discord-Translate**。

开源仓库地址：[BaBiQ888/discord-translate](https://github.com/BaBiQ888/discord-translate)

## 📌 插件的主要功能

这款基于 Chrome Extension 开发的辅助插件具有如下核心特性：

1. **多语言即时转化**: 无需离开当前浏览或呼出其它词典工具窗口。只要有他人发出新信息或者往回翻阅聊天记录，它就能原位、实时地展示翻译过来的文字。
2. **无缝整合 UI**: 与 Discord 官方深色/浅色主题的完全视觉贴合，翻译内容可以像原生跟帖一样悬浮存在。不会打乱本身就非常挤压的 Discord 空间结构排版。
3. **高度自定义**: 可以在扩展图标配置自己的译入语和译出语。比如中英转换、中日转换，针对一些长篇讨论可自动关闭以节约网络资源。

## 🛠️ 技术实现原理浅析

作为一个原生运行的浏览器扩展，主要依赖了以下操作：
- **MutationObserver 页面嗅探**: 由于 Discord 的聊天窗体是一个高度动态渲染的长列表 (Virtual DOM list)，直接获取所有的 DOM 元素来翻译是无效的（只渲染用户可视区）。利用 `MutationObserver`，我们可以实时监听新 DOM Node 的插入事件（如新发出消息的气泡），精准地抓取内部文本。
- **避免并发阻塞**: 将嗅探到的文本压入本地构建的延时异步队列当中，减少因为弹幕一样的聊天导致瞬间过百次翻译接口的高并发滥用，提升翻译准确性与稳定性。
- **DOM 侵入式绘制**: 拿到翻译的内容后，我们将生成的新跨节点 Element 直接插入到 `div.messageContent` 等相关的树层级上。

## 🚀 下载与安装体验

作为一个轻量化玩具开源产品，你可以这样快速安装：

**1. Clone 项目**
```bash
git clone https://github.com/BaBiQ888/discord-translate.git
```

**2. 挂载浏览器**
- 打开最新的 Chrome / Edge 的插件管理面板并勾选 **开发者模式 (Developer Mode)**。
- 随后点击 **加载已解压的扩展程序 (Load unpacked)**。
- 指定刚刚你克隆代码所在的本地文件夹目录即可装载成功。

**3. 使用配置**
随后进入 `discord.com/app` 网页端，点击上方扩展的小图标就可以设置您想要的偏好语言并直接启用。

## ✍️ 写在最后

开发这类型插件能大幅提升操作底层 DOM 结构的熟悉度与 Web API 实践。如果你在使用中看到了任何界面的跑版（DOM Mutation 可能会受到 Discord 官网结构变化而挂掉），或者你有任何的新想法，[随时来提个 Pull Request](https://github.com/BaBiQ888/discord-translate)！
