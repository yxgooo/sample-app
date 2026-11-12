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

## 跳广告提速（v2）

针对哔哩哔哩、酷安、七猫免费小说、抖音、网易云音乐 5 个 App，
只调时序与匹配参数，选择器和规则 key 一律不动：

- **开屏**：4 个 App 的开屏组补 `forcedTime: 10000`（开屏页常不产生无障碍事件，
  不主动轮询就只能等下一次事件，往往要等到广告播完或用户动手）；
  抖音/酷安/网易云的 开屏组再补 `matchRoot: true`，与 B站 现有配置对齐
- **优先匹配**：39 个广告组加 `order: -5`，排在 功能类/权限类 规则之前，
  不再排在几十条无关规则后面
- **快速查找**：22 个广告组补 `fastQuery: true`，以 id/vid/text 开头的选择器
  直接走 `findAccessibilityNodeInfosBy*`，不满足格式的选择器自动退回普通查找
- **减少等待**：抖音 3 条"刷到推广/游戏/直播视频时上滑"去掉 `actionDelay: 200`；
  七猫 字节视频广告组只保留通用"跳过"文本规则 300ms 延迟，SDK 专用关闭按钮设为 0

想再激进一点或更保守：调 `order`（越小越先）、`forcedTime`（轮询时长）、
`actionDelay`（每次动作前等待）三处即可，都不用改选择器。

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
