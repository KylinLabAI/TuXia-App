# 途匣 TuXia

[English](README.md)

![途匣 TuXia Logo](resources/logo_256.png)

途匣 TuXia 是一款面向县域、乡镇与村庄出行者的离线优先公交线路库与班次换乘规划工具。

## 为什么需要途匣

乡村客运长期缺席主流地图产品。线路公告分散在县政府网站和线下张贴中，班次随时调整，而只按最短距离给出的答案往往无法在当天真正走通。途匣把经过审核的公开公告整理成可持续维护的线路库和可下载的线路包，让乘客在没有网络时也能查询村庄与站点、规划多段行程。

## 功能特性

- **离线线路包** — 带校验和的地区线路包安装在本机，断网后仍可搜索与规划
- **地点搜索** — 支持村庄、乡镇与站点名称及别名，按行政区划限定范围，并显示即将发出的班次
- **线路详情** — 按序列出停靠站、时刻表、票价、方向、班线类型以及停运或季节性状态
- **确定性换乘规划** — 支持步行换乘、最短等待时间、末班校验，并按条件排序多个可行方案
- **地图与步行接力** — 查看线路或行程覆盖图，并把步行段交给外部地图应用导航
- **可信度披露** — 每条线路都显示来源说明、可信度等级与最后更新日期，并附通俗免责声明
- **乘客纠错** — 支持带校验和的证据照片、离线草稿、幂等提交，并进入留痕的人工审核队列
- **运营控制台** — 数据源登记、采集运行、并排审核、版本化线路库、线路包构建与审计记录

## 下载

| 组件 | 下载 | 要求 |
|------|------|------|
| Android 乘客端（APK） | [最新版本](https://gitee.com/KylinLab/TuXia-App/releases/latest) | Android 8.0+（arm64） |
| 后端发布包（Python，Linux x64） | [最新版本](https://gitee.com/KylinLab/TuXia-App/releases/latest) | Python 3.14；发布包内含试点县种子数据 |
| 运营控制台静态包 | [最新版本](https://gitee.com/KylinLab/TuXia-App/releases/latest) | 主流现代浏览器 |
| 后端容器镜像 | `docker pull crpi-5w5kegfxurclu2lz.cn-hangzhou.personal.cr.aliyuncs.com/kylinlab2026/tuxia:latest` | Linux 容器主机 |

Android App Bundle（AAB）与 APK 一同发布，便于应用商店提交。每个发布资产都附有同名 `.sha256` 校验文件。同一批发布资产同步发布于 [GitHub](https://github.com/KylinLabAI/TuXia-App/releases/latest) 与 [Gitee](https://gitee.com/KylinLab/TuXia-App/releases/latest)。

## 快速开始

### 乘客

1. 从最新版本发布页下载 APK，并允许从该来源安装应用。
2. 打开途匣，顶部状态栏显示 **未安装线路包**。
3. 进入 **线路包** 标签页，安装试点县线路包。
4. 搜索村庄、乡镇或站点，或规划行程——安装线路包后均可离线使用。

### 运营人员

1. 解压后端发布包（或拉取容器镜像），在 8421 端口启动 API。
2. 首次启动前设置 `TUXIA_SEED_PASSWORD`，以便创建试点控制台账号；部署在 HTTPS 之后时保持 `TUXIA_SECURE_COOKIES=true`。
3. 将运营控制台作为静态资源部署在同一反向代理之后，并把 `/api/v1` 转发到后端。
4. 使用 `reviewer`、`operator`、`owner` 或 `admin` 登录，处理审核队列，并在有已审核变更后构建线路包。

本地纯 HTTP 开发必须显式设置 `TUXIA_SECURE_COOKIES=false`；默认值保持安全 Cookie。

## 文档

- [用户手册（中文）](../user_manual/user_manual.zh.md)
- [用户手册（英文）](../user_manual/user_manual.md)
- [发布说明](https://gitee.com/KylinLab/TuXia-App/releases/latest)

## 系统架构

| 组件 | 运行环境 | 职责 |
|------|----------|------|
| 乘客端 | Flutter（Dart），Android | 离线搜索、线路详情、换乘规划、线路包、纠错、地图 |
| 后端 | Python 3.14、FastAPI | 数据源登记、采集、变更检测、审核、线路库、线路包构建，并在 `/api/v1` 提供公共 API |
| 控制台 | React、Vite、TypeScript | 审核队列、候选对比、数据源登记、采集健康、线路包状态、审计记录 |
| 数据 | SQLite、规范化 JSON + gzip 线路包 | 线路库状态，以及确定性、带校验和的发布包 |

线路包 schema 是后端与乘客端之间唯一的契约。搜索与行程规划完全在本机完成。

## 已知限制

- 当前版本只覆盖一个试点县（`pilot-county`）。
- 电话预约类班线仅作信息展示，不纳入固定班次换乘规划。
- 地图瓦片需要联网；离线时仍可使用线路与方案的文字信息。

## 联系方式

kylinlab.tech
