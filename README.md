# Immich 相册 fnOS FPK（v1.0）

这是由 **clairvoyant** 维护和分发的 Immich fnOS 应用包。它通过飞牛应用中心安装，使用 Docker Compose 启动 Immich Server、机器学习、Redis 和带 pgvector 的 PostgreSQL。

## 功能

- 桌面图标和应用中心入口，网页端口 `2283`。
- `/vol1` 及 `/vol00`、`/vol01`、`/vol2`、`/vol3` 挂载到容器 `/storage*`，用于整盘媒体库。
- 上传、数据库、Redis 和机器学习缓存统一保存到 `${STORAGE_ROOT}/Docker/immich/`；默认存储根为 `/vol1`。
- 首次安装流程会执行应用初始化脚本，自动处理管理员和外部库设置；若当前 Immich 版本拒绝空密码，脚本会在目标机生成随机密码并以 `0600` 权限保存到 `${STORAGE_ROOT}/Docker/immich/administrator-credentials.txt`，日志只记录状态。

自动账号的登录邮箱为 `administrator@immich.local`，显示名为 `Administrator`。Immich 仍然保留自身账户会话，fnOS 的入口免去的是额外的应用中心鉴权。首次安装也会在目标机本地生成数据库密码，升级不会轮换已有密码。
- 默认外部库路径为容器内 `/storage`，可在安装前把 `IMMICH_EXTERNAL_IMPORT_PATHS` 改为逗号分隔的媒体目录，避免扫描 Docker 数据目录。
- 当前包面向 x86 fnOS；镜像使用上游 `release` 标签，正式商店审核前应在目标版本锁定镜像版本或 digest。
- 管理员显示名固定为 `Administrator`，登录邮箱为 `administrator@immich.local`。Immich 拒绝空密码时，安装脚本在目标机生成随机密码并写入 `${STORAGE_ROOT}/Docker/immich/administrator-credentials.txt`（权限 600）；密码不会出现在构建产物、终端或日志中。

## 安装

在飞牛应用中心选择“手动安装”，上传 `dist/immich-1.0.fpk`。安装前确认 Docker 服务已启动，并确保目标存储盘可写。安装后从桌面打开“Immich 相册”。

升级时使用应用中心的更新功能。升级前建议备份 `/vol1/Docker/immich/`（或你在 `.env` 中设置的存储根）以及数据库。

## 从源码构建

`fnpack` 由 fnOS 提供，建议在 fnOS Shell 中执行：

```sh
cd immich-fpk-v3
bash scripts/validate-fpk.sh
bash scripts/build-on-fnos.sh
```

产物写入 `dist/immich-1.0.fpk`，同目录的 `SHA256SUMS` 可用于发布前校验。构建脚本不会把密码、令牌或运行时数据写入发布目录。

## 发布到 GitHub / 应用商店

公开仓库至少应包含本目录源码、`LICENSE`、构建说明和 `SHA256SUMS`。应用商店提交使用构建产物及对应版本说明；提交前在干净的 fnOS 实例完成安装、升级和首次启动验证，并提供校验值。GitHub 发布和飞牛商店审核属于外部操作，需要由仓库/商店账号执行。

## 许可证

本 FPK 工程文件按仓库附带的许可证发布。Immich 及所用容器镜像遵循各自上游许可证和分发条款。
