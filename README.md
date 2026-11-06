# id667 GKD 订阅（结构修复版）

基于 id667的GKD订阅 626 版整理，修复了原文件里 95 处结构性错误，
让 27 个 App 里被损坏的规则重新生效。

## 订阅链接

`
https://raw.githubusercontent.com/yxgooo/sample-app/main/gkd.json5
`

大陆访问 raw.githubusercontent.com 不稳定时，用 jsDelivr 镜像：

`
https://cdn.jsdelivr.net/gh/yxgooo/sample-app@main/gkd.json5
`

在 GKD 里：订阅 → 右上角 + → 从链接添加 → 粘贴上面的地址。

## 这份订阅修了什么

原订阅 626 版有 100 个结构错误（GKD 不报错，但对应规则静默失效）：

| 问题 | 数量 | 修复方式 |
|---|---|---|
| 组内 rules 被压成字符串/对象 | 28 组 | 重建为规则数组 |
| snapshotUrls / exampleUrls 放在组级 | 31 处 | 下沉到规则级 |
| preKeys 写成标量 | 17 处 | 包装成数组 |
| preKeys 指向不存在或后方的 key | 16 处 | 删除无效引用 |
| 空规则 {} | 3 条 | 删除 |

修复后：1000 个 App / 2503 个规则组 / 3948 条规则，校验 ERROR=0。
**group.key 与 rule.key 全部未变动**，已安装用户本地的组开关配置不会丢。

修复影响到的 27 个 App 包括：Gspace、Keep、菜鸟、大众点评、高德、京东、
夸克漫画、荣耀智慧空间、扫描全能王、天翼生活、招商银行、微信读书等。

## 与官方订阅的关系

- 原订阅：<https://gkd667.vv.ax/gkd.json5>（作者 👻）
- 本仓库：仅做结构修复，不新增、不删除任何业务规则
- 订阅 id 改为 6671，避免与原订阅 id 667 冲突；两条同时安装会双份规则，请只保留一条

## 更新记录

见 [CHANGELOG.md](./CHANGELOG.md)

## 来源与致谢

规则内容来自原订阅作者 👻 与 [Lin-arm/GKD_subscription](https://github.com/Lin-arm/GKD_subscription)，
选择器能力来自 [gkd-kit/gkd](https://github.com/gkd-kit/gkd)。本仓库仅做格式修复与分发，署名与来源见 [NOTICE.md](./NOTICE.md)。

## 免责声明

规则通过无障碍服务自动点击，可能因 App 改版而失效或误点。请自行评估风险；
本仓库不提供任何担保，也不收集任何用户数据。
