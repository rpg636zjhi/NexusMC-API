<p align="center">
    <img src="logo.png" alt="NexusMC-API Logo" width="400"/>
</p>

# NexusMC API 文档（非官方）

本文档基于实际抓取的 API 响应整理而成，所有端点均返回 JSON 格式（除 RSS 外）。  
> 🚧 **非官方文档**：本 API 文档基于对 [NexusMC](https://nexusmc.cn) 网站实际网络请求的抓包整理而成(除了RSS)，**并非官方发布**，可能随时因网站更新而失效。请合理使用，勿对目标服务器造成过大负担。

> 🚧 **禁止爬虫恶意使用**：NexusMC在其[robots.txt](https://nexusmc.cn/robots.txt)中 禁止爬虫访问 /admin/ /api/ /settings/ messages/ /search?* /user/me/ 

## 简介 

[NexusMC](https://nexusmc.cn/) 是一个 Minecraft 资源分享社区，本站点提供资源下载、论坛交流等功能。此文档整理了 NexusMC 公开 API 的调用方式、参数和返回结构，旨在帮助开发者了解其数据接口，方便二次开发或学习交流。

---

## 目录
1. [RSS 订阅源](#1-rss-订阅源)
2. [资源详情](#2-资源详情)
3. [资源列表](#3-资源列表)
4. [资源分类](#4-资源分类)
5. [站点横幅](#5-站点横幅)
6. [热门推荐](#6-热门推荐)
7. [热门标签](#7-热门标签)
8. [帖子回复](#8-帖子回复)
9. [帖子列表](#9-帖子列表)
10. [论坛板块列表](#10-论坛板块列表)
11. [社区统计](#11-社区统计)
12. [用户公开信息](#12-用户公开信息)
13. [用户徽章](#13-用户徽章)
14. [用户活跃度](#14-用户活跃度)
15. [当前用户信息](#15-当前用户信息)
16. [每日签到状态](#16-每日签到状态)
17. [全局搜索](#17-全局搜索)
18. [站点设置](#18-站点设置)

---

## 1. RSS 订阅源
获取全站最新动态（资源发布、论坛帖子、更新日志等），返回标准的 RSS 2.0 XML。

- **端点**: `/api/rss`
- **方法**: `GET`
- **参数**: 无
- **返回**: `application/rss+xml`

**返回结构**：标准的 RSS 2.0 XML，包含 `<channel>` 和多个 `<item>`。每个 `<item>` 包含标题、链接、发布时间、作者、分类和描述。

**示例**：见 [https://nexusmc.cn/api/rss](https://nexusmc.cn/api/rss)

---

## 2. 资源详情
获取单个资源的详细信息。

- **端点**: `/api/resources/{resourceId}`
- **方法**: `GET`
- **路径参数**:  
  - `resourceId`: 资源的 UUID（例如 `2309820f-4f87-4ac2-8800-57af8b38e78c`）
- **返回**: `application/json`

**返回字段说明**：

| 字段 | 类型 | 说明 |
|------|------|------|
| `id` | string | 资源UUID |
| `title` | string | 标题 |
| `description` | string | 简短描述 |
| `content` | string | 详细介绍（ProseMirror JSON 字符串，需解析） |
| `authorId` | string | 作者用户ID |
| `author` | object | 作者信息（包含 `id`, `username`, `avatar`） |
| `platform` | string | 平台：`java` / `bedrock` / `universal` |
| `category` | string | 分类，如 `plugin_config`、`plugin`、`texture`、`utility`、`launcher` |
| `subCategory` | string or null | 子分类（可能为JSON数组或null） |
| `sideSupport` | string | 支持端：`both` 等 |
| `mcVersions` | string | JSON数组，支持的MC版本，如 `["通用"]` 或 `["1.21.11","1.21.10"]` |
| `tags` | string | JSON数组，标签，如 `["数据同步","跨服务器"]` |
| `coverImage` | string or null | 封面图片路径（相对路径，需拼接基础URL） |
| `downloadCount` | int | 下载次数 |
| `views` | int | 浏览次数 |
| `likeCount` | int | 点赞数 |
| `favoriteCount` | int | 收藏数 |
| `downloadType` | string | 下载类型：`external`（外部链接）或 `file`（本站文件） |
| `fileUrl` | string | 下载链接（若为外部链接则为URL，否则为文件路径） |
| `fileSize` | int | 文件大小（字节），若无则为0 |
| `fileName` | string | 文件名（若有） |
| `version` | string | 资源版本号 |
| `gallery` | string | JSON数组，画廊图片URL列表 |
| `sourceType` | string | 来源类型：`closed` 等 |
| `repositoryUrl` | string or null | 代码仓库URL（若有） |
| `isRecommended` | boolean | 是否推荐 |
| `status` | string | 状态：`approved` 等 |
| `rejectReason` | string or null | 拒绝原因（若被拒绝） |
| `createdAt` | string | ISO 8601 时间戳 |
| `updatedAt` | string | ISO 8601 时间戳 |

**示例**（截取关键部分）：
```json
{
  "id": "2309820f-4f87-4ac2-8800-57af8b38e78c",
  "title": "Zenith插件包",
  "description": "一个MCPE插件包，适合新手开服",
  "content": "{\"type\":\"doc\",\"content\":[...]}",
  "authorId": "43d177bc-4ef6-448b-ae71-6798555823d9",
  "author": {
    "id": "43d177bc-4ef6-448b-ae71-6798555823d9",
    "username": "XIaoao5297",
    "avatar": "/avatars/default.png"
  },
  "platform": "universal",
  "category": "plugin_config",
  "subCategory": null,
  "sideSupport": "both",
  "mcVersions": "[\"通用\"]",
  "tags": "[]",
  "coverImage": "/uploads/images/yHruzfcZtnMa7_VAHzC-1.webp",
  "downloadCount": 4,
  "views": 57,
  "likeCount": 0,
  "favoriteCount": 0,
  "downloadType": "external",
  "fileUrl": "https://github.com/Xiaoao5297/Zenith-Core/releases",
  "fileSize": 0,
  "fileName": "",
  "version": "1.9.10",
  "gallery": "[\"https://nexusmc.cn/uploads/images/MLDkPEYYw-hDXWaWbEfe_.webp\",\"https://img.shields.io/badge/License-MIT-blue.svg\",\"https://img.shields.io/badge/Download-1.9.9-orange.svg\"]",
  "sourceType": "closed",
  "repositoryUrl": null,
  "isRecommended": false,
  "status": "approved",
  "rejectReason": null,
  "createdAt": "2026-03-03T01:23:12.276Z",
  "updatedAt": "2026-03-03T11:14:43.268Z"
}
```

---

## 3. 资源列表
获取资源列表，支持分页和排序。

- **端点**: `/api/resources`
- **方法**: `GET`
- **查询参数**:
  - `page`: 页码（默认1）
  - `pageSize`: 每页数量（默认40）
  - `sort`: 排序方式，例如 `latest`（最新发布）
  - 可能支持平台、分类等更多筛选（未验证）
- **返回**: `application/json`

**返回结构**：

```json
{
  "resources": [
    {
      "id": "string",
      "title": "string",
      "description": "string",
      "authorId": "string",
      "platform": "string",
      "category": "string",
      "subCategory": "string or null",
      "sideSupport": "string",
      "mcVersions": "string (JSON array)",
      "tags": "string (JSON array)",
      "coverImage": "string or null",
      "downloadCount": "int",
      "views": "int",
      "likeCount": "int",
      "favoriteCount": "int",
      "version": "string",
      "status": "string",
      "isRecommended": "boolean",
      "createdAt": "string (ISO 8601)",
      "updatedAt": "string (ISO 8601)",
      "author": {
        "id": "string",
        "username": "string",
        "avatar": "string"
      }
    }
  ],
  "pagination": {
    "page": "int",
    "pageSize": "int",
    "total": "int",
    "totalPages": "int"
  }
}
```

**字段说明**：
- `resources` 数组中的每个元素与资源详情接口的元数据部分一致（不含 `content`、`downloadType`、`fileUrl` 等详细字段）。
- `pagination` 包含分页信息。

**示例**（按最新排序，第1页）：见 [https://nexusmc.cn/api/resources?page=1&sort=latest](https://nexusmc.cn/api/resources?page=1&sort=latest)

---

## 4. 资源分类
获取资源分类树，可按平台筛选。

- **端点**: `/api/resources/categories`
- **方法**: `GET`
- **查询参数**:
  - `platform`: 可选，筛选平台，如 `java`, `bedrock`, `universal`。不提供则返回所有平台分类。
- **返回**: `application/json`（直接返回分类数组）

**返回结构**（嵌套数组）：

```json
[
  {
    "id": "string",
    "name": "string",
    "slug": "string",
    "description": "string",
    "icon": "string",
    "color": "string",
    "platform": "string",
    "parentId": "string or null",
    "sortOrder": "int",
    "groupLabel": "string",
    "versionGroups": "string (JSON array of UUIDs)",
    "children": [ ... ], // 子分类数组，结构与父分类相同
    "filterValue": "string"
  },
  ...
]
```

**字段说明**：
- `id`：分类ID（UUID）。
- `name`：分类名称。
- `slug`：URL友好的标识符。
- `description`：描述（可能为空）。
- `icon`：图标标识（如 `mdi:puzzle` 或图片路径）。
- `color`：主题色（十六进制）。
- `platform`：所属平台（`java`/`bedrock`/`universal`）。
- `parentId`：父分类ID，若为顶级分类则为 `null`。
- `sortOrder`：排序顺序（数字越小越靠前）。
- `groupLabel`：分组标签（用于UI分组）。
- `versionGroups`：关联的版本组ID数组（JSON字符串）。
- `children`：子分类数组，结构相同。
- `filterValue`：用于筛选的值（通常与 `slug` 相同或为特定标识）。

**示例**（通用平台 `platform=universal`）：
```json
[
  {
    "id": "12d45e34-44a5-4af6-8d73-61ac2d541f35",
    "name": "实用软件",
    "slug": "utility",
    "description": "",
    "icon": "mdi:wrench",
    "color": "#3b82f6",
    "platform": "universal",
    "parentId": null,
    "sortOrder": 20,
    "groupLabel": "",
    "versionGroups": "[]",
    "children": [],
    "filterValue": "utility"
  },
  {
    "id": "9b56acdd-acf9-4448-b1c4-b31535a077d1",
    "name": "启动器",
    "slug": "launcher",
    "icon": "mdi:rocket-launch",
    "color": "#8b5cf6",
    "platform": "universal",
    "parentId": null,
    "sortOrder": 21,
    "groupLabel": "",
    "versionGroups": "[]",
    "children": [],
    "filterValue": "launcher"
  }
  // ... 更多分类
]
```
其他平台示例：见 [https://nexusmc.cn/api/resources/categories?platform=java](https://nexusmc.cn/api/resources/categories?platform=java) 和 [https://nexusmc.cn/api/resources/categories?platform=bedrock](https://nexusmc.cn/api/resources/categories?platform=bedrock)

---

## 5. 站点横幅
获取网站首页或页面的横幅（Banner）展示信息。

- **端点**: `/api/banners`
- **方法**: `GET`
- **参数**: 无
- **返回**: `application/json`（直接返回横幅数组）

**返回结构**：

```json
[
  {
    "id": "string",
    "title": "string",
    "subtitle": "string",
    "imageUrl": "string",
    "linkUrl": "string",
    "bgColor": "string",
    "htmlContent": "string",
    "sortOrder": "int",
    "enabled": "boolean",
    "createdAt": "string (ISO 8601)",
    "updatedAt": "string (ISO 8601)"
  },
  ...
]
```

**字段说明**：
- `id`：横幅ID。
- `title`：标题。
- `subtitle`：副标题。
- `imageUrl`：背景图片URL（可能为空）。
- `linkUrl`：点击跳转链接。
- `bgColor`：背景颜色（十六进制，可能与 `htmlContent` 共同作用）。
- `htmlContent`：自定义HTML内容，用于复杂布局（包含样式和内嵌图片）。
- `sortOrder`：排序顺序。
- `enabled`：是否启用。
- `createdAt`：创建时间。
- `updatedAt`：更新时间。

**示例**：见 [https://nexusmc.cn/api/banners](https://nexusmc.cn/api/banners)（数组内包含多个横幅对象，如欢迎横幅和版本更新横幅）

---

## 6. 热门推荐
获取首页或特定页面的热门推荐内容列表（混合资源和帖子）。

- **端点**: `/api/hot-picks`
- **方法**: `GET`
- **参数**: 无
- **返回**: `application/json`（直接返回内容数组）

**返回结构**（数组元素为资源或帖子，通过 `type` 字段区分）：

| 字段 | 类型 | 说明 |
|------|------|------|
| `id` | string | 内容ID |
| `title` | string | 标题 |
| `type` | string | 内容类型：`resource`（资源）或 `post`（帖子） |

**当 `type` 为 `resource` 时，额外包含以下字段：**

| 字段 | 类型 | 说明 |
|------|------|------|
| `description` | string | 资源描述 |
| `coverImage` | string or null | 封面图片路径 |
| `downloadCount` | int | 下载次数 |
| `views` | int | 浏览次数 |
| `likeCount` | int | 点赞数 |
| `createdAt` | string | 发布时间 |
| `platform` | string | 平台 |
| `category` | string | 分类 |
| `author` | object | 作者信息（`id`, `username`, `avatar`） |

**当 `type` 为 `post` 时，额外包含以下字段：**

| 字段 | 类型 | 说明 |
|------|------|------|
| `views` | int | 浏览次数 |
| `likeCount` | int | 点赞数 |
| `createdAt` | string | 发布时间 |
| `author` | object | 作者信息（`id`, `username`, `avatar`） |
| `board` | object | 所属板块信息（至少包含 `name`） |
| `_count` | object | 计数，包含 `replies`（回复数） |

**示例**：见 [https://nexusmc.cn/api/hot-picks](https://nexusmc.cn/api/hot-picks)

---

## 7. 热门标签
获取站内热门搜索标签及其使用次数。

- **端点**: `/api/search/hot-tags`
- **方法**: `GET`
- **参数**: 无
- **返回**: `application/json`（直接返回标签数组）

**返回结构**：

```json
[
  {
    "id": "string",
    "name": "string",
    "slug": "string",
    "count": "int",
    "createdAt": "string (ISO 8601)"
  },
  ...
]
```

**字段说明**：
- `id`：标签ID。
- `name`：标签名称。
- `slug`：标签的URL友好标识（通常与 `name` 相同）。
- `count`：使用次数（即被多少个资源/帖子使用）。
- `createdAt`：首次创建/记录时间。

**示例**：见 [https://nexusmc.cn/api/search/hot-tags](https://nexusmc.cn/api/search/hot-tags)

---

## 8. 帖子回复
获取某个论坛帖子的回复列表（包含嵌套回复）。

- **端点**: `/api/posts/{postId}/replies`
- **方法**: `GET`
- **路径参数**:  
  - `postId`: 帖子的 UUID（例如 `e20e89b1-1e3b-44e2-91e1-64da4452cb2b`）
- **查询参数**:
  - `page`: 页码（默认1）
  - `pageSize`: 每页数量（默认20）
- **返回**: `application/json`

**返回结构**：

```json
{
  "replies": [
    {
      "id": "string",
      "content": "string (ProseMirror JSON)",
      "postId": "string",
      "parentId": "string or null",
      "replyToUserId": "string or null",
      "replyToUsername": "string or null",
      "floorNumber": "int",
      "createdAt": "string (ISO 8601)",
      "updatedAt": "string (ISO 8601)",
      "author": {
        "id": "string",
        "username": "string",
        "avatar": "string",
        "role": "string",
        "status": "string",
        "contribution": "int"
      },
      "replies": [ ... ]  // 子回复数组，结构与父回复相同
    }
  ],
  "pagination": {
    "page": "int",
    "pageSize": "int",
    "total": "int",
    "totalPages": "int"
  }
}
```

**字段说明**：
- `replies` 数组：每个元素为一条回复，可嵌套 `replies` 表示子回复。
- `content`：回复内容，ProseMirror JSON 字符串，需解析。
- `parentId`：若为子回复，则为父回复ID。
- `replyToUserId`/`replyToUsername`：被回复的用户信息。
- `author`：作者信息，包含 `role`（用户角色）、`status`（状态）、`contribution`（贡献值）。

**示例**：见 [https://nexusmc.cn/api/posts/e20e89b1-1e3b-44e2-91e1-64da4452cb2b/replies?page=1](https://nexusmc.cn/api/posts/e20e89b1-1e3b-44e2-91e1-64da4452cb2b/replies?page=1)

---

## 9. 帖子列表
获取论坛帖子列表，支持分页。

- **端点**: `/api/posts`
- **方法**: `GET`
- **查询参数**（推测）:
  - `page`: 页码（默认1）
  - `pageSize`: 每页数量（默认20）
  - 可能支持按板块筛选（未验证）
- **返回**: `application/json`

**返回结构**：

```json
{
  "posts": [
    {
      "id": "string",
      "title": "string",
      "authorId": "string",
      "boardId": "string",
      "views": "int",
      "likeCount": "int",
      "favoriteCount": "int",
      "isPinned": "boolean",
      "isLocked": "boolean",
      "isHighlighted": "boolean",
      "status": "string",
      "copyrightEnabled": "boolean",
      "copyrightLicense": "string or null",
      "lastReplyAt": "string (ISO 8601)",
      "createdAt": "string (ISO 8601)",
      "updatedAt": "string (ISO 8601)",
      "author": {
        "id": "string",
        "username": "string",
        "avatar": "string"
      },
      "board": {
        "id": "string",
        "name": "string",
        "description": "string",
        "icon": "string",
        "color": "string",
        "rules": "string",
        "sortOrder": "int",
        "postCount": "int",
        "createdAt": "string (ISO 8601)",
        "updatedAt": "string (ISO 8601)"
      },
      "_count": {
        "replies": "int"
      }
    }
  ],
  "pagination": {
    "page": "int",
    "pageSize": "int",
    "total": "int",
    "totalPages": "int"
  }
}
```

**字段说明**：
- `posts` 数组：每个元素为一个帖子。
- `isPinned`：是否置顶；`isLocked`：是否锁定；`isHighlighted`：是否高亮。
- `status`：帖子状态，如 `approved`。
- `copyrightEnabled`：是否启用版权声明；`copyrightLicense`：版权许可证。
- `lastReplyAt`：最后回复时间。
- `author`：作者信息。
- `board`：所属板块信息（与 `/api/posts/boards` 返回的板块对象一致）。
- `_count.replies`：回复数量。

**示例**：见 [https://nexusmc.cn/api/posts](https://nexusmc.cn/api/posts)

---

## 10. 论坛板块列表
获取所有论坛板块的详细信息。

- **端点**: `/api/posts/boards`
- **方法**: `GET`
- **参数**: 无
- **返回**: `application/json`（直接返回板块数组）

**返回结构**：

```json
[
  {
    "id": "string",
    "name": "string",
    "description": "string",
    "icon": "string",
    "color": "string",
    "rules": "string",
    "sortOrder": "int",
    "postCount": "int",
    "createdAt": "string (ISO 8601)",
    "updatedAt": "string (ISO 8601)"
  },
  ...
]
```

**字段说明**：
- `id`：板块ID（数字或UUID）。
- `name`：板块名称。
- `description`：板块描述。
- `icon`：图标标识（如 `mdi:forum`）。
- `color`：主题色（十六进制）。
- `rules`：板块规则（可能为空）。
- `sortOrder`：排序顺序（数字越小越靠前）。
- `postCount`：该板块下的帖子总数。
- `createdAt`：创建时间。
- `updatedAt`：更新时间。

**示例**：见 [https://nexusmc.cn/api/posts/boards](https://nexusmc.cn/api/posts/boards)

---

## 11. 社区统计
获取社区的整体统计信息。

- **端点**: `/api/posts/community-stats`
- **方法**: `GET`
- **参数**: 无
- **返回**: `application/json`

**返回结构**：

```json
{
  "resources": 23,
  "posts": 12,
  "today": 9,
  "online": 1
}
```

**字段说明**：
- `resources`：资源总数。
- `posts`：帖子总数。
- `today`：今日新增内容（可能包括资源、帖子等）。
- `online`：当前在线人数。

**示例**：见 [https://nexusmc.cn/api/posts/community-stats](https://nexusmc.cn/api/posts/community-stats)

---

## 12. 用户公开信息
获取指定用户的公开个人资料信息。

- **端点**: `/api/users/{userId}`
- **方法**: `GET`
- **路径参数**:  
  - `userId`: 用户的 UUID（例如 `7e1c4f79-3406-4959-b3a0-7c5a89ce887b`）
- **返回**: `application/json`

**返回字段说明**：

| 字段 | 类型 | 说明 |
|------|------|------|
| `id` | string | 用户UUID |
| `uid` | int | 用户数字ID |
| `username` | string | 用户名 |
| `avatar` | string | 头像路径（相对路径） |
| `bio` | string or null | 个人简介 |
| `location` | string or null | 所在地 |
| `onlineTime` | int | 在线时长（单位可能是分钟） |
| `role` | string | 角色，如 `user`（普通用户）、`admin`（管理员）等 |
| `status` | string | 账户状态，如 `active`（正常） |
| `contribution` | int | 贡献值 |
| `warningPoints` | int | 警告点数 |
| `messagePrivacy` | string | 私信隐私设置，如 `everyone`（所有人） |
| `createdAt` | string | 注册时间（ISO 8601） |

**注意**：
- 此接口返回的是用户的**公开信息**，与需要认证的 `/api/auth/me` 接口相比，字段较少（不包含邮箱、通知设置等隐私信息）。
- 要获取用户获得的**徽章列表**，请使用 `/api/users/{userId}` 接口，但根据之前的抓包，可能需要额外的路径或查询参数（如 `/api/users/{userId}/badges`），具体需进一步验证。目前已知的直接访问 `/api/users/{userId}` 仅返回上述基本信息。

**示例**：
```json
{
  "id": "7e1c4f79-3406-4959-b3a0-7c5a89ce887b",
  "uid": 11,
  "username": "冷例",
  "avatar": "/uploads/images/_wNcZZn_Tp5yI__toblSP.webp",
  "bio": null,
  "location": null,
  "onlineTime": 10,
  "role": "user",
  "status": "active",
  "contribution": 2,
  "warningPoints": 0,
  "messagePrivacy": "everyone",
  "createdAt": "2026-02-28T15:40:45.101Z"
}
```

---

## 13. 用户徽章
获取用户获得的徽章列表。

- **端点**: 推测为 `/api/users/{userId}/badges`（或类似路径），根据之前抓包数据，直接访问 `/api/users/{userId}` **并不返回**徽章信息。
- **方法**: `GET`
- **路径参数**:  
  - `userId`: 用户的 UUID
- **返回**: `application/json`（直接返回徽章数组）

**返回结构**（数组）：

```json
[
  {
    "id": "string",
    "key": "string",
    "name": "string",
    "description": "string",
    "icon": "string",
    "color": "string",
    "category": "string",
    "condition": "string (JSON)",
    "sortOrder": "int",
    "createdAt": "string (ISO 8601)",
    "earnedAt": "string (ISO 8601)"
  },
  ...
]
```

**字段说明**：
- `id`：徽章ID。
- `key`：徽章唯一标识。
- `name`：徽章名称。
- `description`：徽章描述。
- `icon`：图标（可能是 emoji 或图标标识）。
- `color`：颜色代码（十六进制）。
- `category`：分类，如 `special`、`social`、`milestone`。
- `condition`：获取条件（JSON 字符串）。
- `sortOrder`：排序顺序。
- `createdAt`：徽章创建时间。
- `earnedAt`：用户获得该徽章的时间。

**注意**：此接口的准确路径有待进一步确认。之前抓包数据显示，访问 `https://nexusmc.cn/api/users/e83753f6-6610-4cef-aab1-17dab7083589` 返回的是**徽章数组**，这与新数据矛盾。可能该用户ID有特殊行为，或接口路径已变更。建议通过实际测试确定。

---

## 14. 用户活跃度
获取用户在某一年内的活跃度数据（发帖/回复数量按天统计）。

- **端点**: `/api/users/{userId}/activity`
- **方法**: `GET`
- **路径参数**:  
  - `userId`: 用户的 UUID
- **查询参数**:
  - `year`: 年份，例如 `2026`
- **返回**: `application/json`

**返回结构**：

```json
{
  "year": "int",
  "total": "int",
  "counts": {
    "YYYY-MM-DD": "int",
    ...
  }
}
```

**字段说明**：
- `year`：查询年份。
- `total`：该年总活跃天数。
- `counts`：对象，键为日期（`YYYY-MM-DD`），值为当天活跃次数。

**示例**：
```json
{
  "year": 2026,
  "total": 5,
  "counts": {
    "2026-03-01": 4,
    "2026-03-02": 1
  }
}
```

---

## 15. 当前用户信息
获取当前登录用户的详细信息（需要认证）。

- **端点**: `/api/auth/me`
- **方法**: `GET`
- **认证**: 需要携带有效的登录凭证（Cookie 或 Authorization Header）
- **参数**: 无
- **返回**: `application/json`

**返回结构**：

```json
{
  "id": "string",
  "uid": "int",
  "username": "string",
  "email": "string",
  "avatar": "string",
  "bio": "string or null",
  "location": "string or null",
  "onlineTime": "int",
  "role": "string",
  "status": "string",
  "contribution": "int",
  "messagePrivacy": "string",
  "emailNotifyReply": "boolean",
  "emailNotifyLike": "boolean",
  "emailNotifySystem": "boolean",
  "twoFactorEnabled": "boolean",
  "createdAt": "string (ISO 8601)",
  "hasPassword": "boolean",
  "hasPasskey": "boolean"
}
```

**字段说明**：
- `id`：用户UUID。
- `uid`：用户数字ID。
- `username`：用户名。
- `email`：邮箱（明文邮箱）。
- `avatar`：头像路径。
- `bio`：个人简介。
- `location`：所在地。
- `onlineTime`：在线时长（分钟）。
- `role`：角色，如 `user`、`admin` 等。
- `status`：账户状态，如 `active`。
- `contribution`：贡献值。
- `messagePrivacy`：私信隐私设置，如 `everyone`。
- `emailNotifyReply`：是否邮件通知回复。
- `emailNotifyLike`：是否邮件通知点赞。
- `emailNotifySystem`：是否邮件通知系统消息。
- `twoFactorEnabled`：是否启用两步验证。
- `createdAt`：注册时间。
- `hasPassword`：是否设置了密码。
- `hasPasskey`：是否设置了通行密钥。

**示例**：
```json
{
  "id": "114514",
  "uid": 114514,
  "username": "不给看",
  "email": "不给看@114514.mail.com",
  "avatar": "/uploads/images/不给看.webp",
  "bio": null,
  "location": null,
  "onlineTime": 114514,
  "role": "admin",
  "status": "active",
  "contribution": 2,
  "messagePrivacy": "everyone",
  "emailNotifyReply": false,
  "emailNotifyLike": false,
  "emailNotifySystem": false,
  "twoFactorEnabled": false,
  "createdAt": "114514-02-28T15:40:45.101Z",
  "hasPassword": false,
  "hasPasskey": false
}
```

---

## 16. 每日签到状态
获取当前用户的每日签到状态和统计（需要认证）。

- **端点**: `/api/checkin/status`
- **方法**: `GET`
- **认证**: 需要携带有效的登录凭证（Cookie 或 Authorization Header）
- **参数**: 无
- **返回**: `application/json`

**返回结构**：

```json
{
  "todayCount": "int",
  "checked": "boolean",
  "streak": "int",
  "contribution": "int"
}
```

**字段说明**：
- `todayCount`：今日已签到总人数。
- `checked`：当前用户今日是否已签到。
- `streak`：当前用户的连续签到天数。
- `contribution`：本次签到获得的贡献值（若 `checked` 为 `true`，则显示已获得的贡献值；若为 `false`，可能显示签到可获得的贡献值，需验证）。

**示例**：
```json
{
  "todayCount": 14,
  "checked": true,
  "streak": 1,
  "contribution": 2
}
```

---

## 17. 全局搜索
搜索全站的资源、帖子等内容，支持分页和多种筛选条件。

- **端点**: `/api/search`
- **方法**: `GET`
- **查询参数**:

| 参数 | 类型 | 说明 | 示例 |
|------|------|------|------|
| `q` | string | **搜索关键词**（URL 编码） | `%E4%BD%A0%E5%A5%BD` ("你好") |
| `type` | string | 搜索类型：`all`(全部) / `resource`(资源) / `post`(帖子) 等 | `all` |
| `boardId` | string | 板块ID过滤（可选） | 空 |
| `platform` | string | 平台过滤：`java` / `bedrock` / `universal`（仅资源类型有效） | 空 |
| `category` | string | 分类过滤（仅资源类型有效） | 空 |
| `sort` | string | 排序方式：`relevance`(相关度) / `newest`(最新) / `popular`(热门) | `relevance` |
| `page` | int | 页码（默认1） | `1` |

- **返回**: `application/json`

**返回结构**：

```json
{
  "items": [
    {
      // 资源对象：结构与资源列表中的元素完全一致，并额外包含以下字段
      "type": "resource",           // 内容类型标识
      "_tagMatched": boolean        // 是否通过标签匹配（而非标题/内容）
      // 其他字段参见资源列表接口中的 `resources` 数组元素
    },
    {
      // 帖子对象（目前示例中未出现，推测其结构与帖子列表接口中的 `posts` 数组元素类似，并包含 `type` 和 `_tagMatched` 字段）
    }
  ],
  "total": "int",
  "matchedTags": [
    "string"
  ],
  "pagination": {
    "page": "int",
    "pageSize": "int",
    "total": "int",
    "totalPages": "int"
  }
}
```

**字段说明**：
- `items`：搜索结果数组。
  - 当结果为**资源**时，每个元素的结构与[资源列表](#3-资源列表)接口中的 `resources` 数组元素完全一致。
  - 额外增加两个字段：
    - `type`：固定为 `"resource"`，标识内容类型。
    - `_tagMatched`：布尔值，表示该结果是否是通过匹配资源的标签（`tags`）而命中的。
  - 当结果为**帖子**时，推测其结构与[帖子列表](#9-帖子列表)接口中的 `posts` 数组元素类似，并同样包含 `type` 和 `_tagMatched` 字段。
- `total`：总结果数。
- `matchedTags`：本次搜索匹配到的标签列表，可用于二次筛选提示。
- `pagination`：分页信息。

**示例**（搜索关键词 "pock"，返回一个资源结果）：
```json
{
  "items": [
    {
      "id": "05a53f5a-c14b-4776-a260-0ecce3067588",
      "title": "pocketmine for Android",
      "authorId": "a05f4596-76d7-44a9-83f9-5e346ffa6ce0",
      "platform": "universal",
      "category": "utility",
      "subCategory": null,
      "sideSupport": "both",
      "mcVersions": "[\"通用\"]",
      "tags": "[]",
      "coverImage": "/uploads/images/h68I0wfLhHv3-vYpEK-oE.webp",
      "downloadCount": 6,
      "views": 51,
      "likeCount": 0,
      "favoriteCount": 0,
      "version": "2.1.6",
      "status": "approved",
      "createdAt": "2026-03-02T04:20:59.839Z",
      "updatedAt": "2026-03-03T19:16:25.526Z",
      "author": {
        "id": "a05f4596-76d7-44a9-83f9-5e346ffa6ce0",
        "username": "qianlong",
        "avatar": "/avatars/default.png"
      },
      "type": "resource",
      "_tagMatched": false
    }
  ],
  "total": 1,
  "matchedTags": [],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "total": 1,
    "totalPages": 1
  }
}
```

---

## 18. 站点设置
获取站点的全局配置参数。

- **端点**: `/api/site-settings`
- **方法**: `GET`
- **参数**: 无
- **返回**: `application/json`

**返回结构**（所有字段值均为字符串，部分为 JSON 字符串）：

| 字段 | 类型 | 说明 |
|------|------|------|
| `cookie-enabled` | string | `"true"`/`"false"` 是否启用 Cookie 提示 |
| `cookie-text` | string | Cookie 提示文案 |
| `cookie-privacy-url` | string | 隐私政策链接 |
| `cookie-accept-text` | string | 接受按钮文字 |
| `cookie-reject-text` | string | 拒绝按钮文字 |
| `maintenance-enabled` | string | `"true"`/`"false"` 是否开启维护模式 |
| `maintenance-title` | string | 维护模式标题 |
| `maintenance-message` | string | 维护模式提示信息 |
| `maintenance-countdown` | string | `"true"`/`"false"` 是否显示倒计时 |
| `maintenance-end-time` | string | 维护结束时间（可能为空） |
| `register-mode` | string | 注册模式：`open` / `invite` / `closed` |
| `passkey-enabled` | string | `"true"`/`"false"` 是否启用通行密钥 |
| `site-subtitle` | string | 站点副标题/口号 |
| `register-email-verify` | string | `"true"`/`"false"` 注册是否需要邮箱验证 |
| `site-favicon` | string | 网站图标路径 |
| `register-show-email-optin` | string | `"true"`/`"false"` 是否显示邮件订阅选项 |
| `register-show-terms` | string | `"true"`/`"false"` 是否显示服务条款勾选框 |
| `register-terms-url` | string | 服务条款链接 |
| `site-announcement-link` | string | 公告详情链接 |
| `footer-text` | string | 底部版权信息 |
| `footer-icp` | string | ICP 备案号（可能为空） |
| `footer-links` | string | JSON数组，底部链接，格式 `[{"name":"关于我们","url":"/p/about"}]` |
| `register-privacy-url` | string | 隐私政策链接 |
| `footer-powered` | string | Powered by 信息（含 HTML 标签） |
| `register-captcha` | string | `"true"`/`"false"` 注册是否需要验证码 |
| `login-captcha` | string | `"true"`/`"false"` 登录是否需要验证码 |
| `password-captcha` | string | `"true"`/`"false"` 修改密码是否需要验证码 |
| `email-change-captcha` | string | `"true"`/`"false"` 修改邮箱是否需要验证码 |
| `reset-password-captcha` | string | `"true"`/`"false"` 重置密码是否需要验证码 |
| `oauth-captcha` | string | `"true"`/`"false"` OAuth 登录是否需要验证码 |
| `theme-hue` | string | 主题色色调（如 "217"） |
| `custom-css` | string | 自定义 CSS（可能为空） |
| `site-name` | string | 站点名称 |
| `site-logo` | string | Logo 图片路径 |
| `seo-keywords` | string | SEO 关键词 |
| `seo-google-verification` | string | Google 验证代码 |
| `seo-bing-verification` | string | Bing 验证代码 |
| `site-notices` | string | JSON数组，站点通知 |
| `seo-title-template` | string | 标题模板（可能为空） |
| `seo-baidu-verification` | string | 百度验证代码 |
| `seo-description` | string | SEO 描述 |
| `twofactor-require-password-login` | string | `"true"`/`"false"` 密码登录是否需要二步验证 |
| `twofactor-require-oauth-login` | string | `"true"`/`"false"` OAuth 登录是否需要二步验证 |
| `twofactor-require-passkey-login` | string | `"true"`/`"false"` 通行密钥登录是否需要二步验证 |
| `site-announcement` | string | 站点公告内容 |
| `mc-tips` | string | Minecraft 小贴士（多行文本，用换行分隔） |

**示例**：见 [https://nexusmc.cn/api/site-settings](https://nexusmc.cn/api/site-settings)

---

**最后更新** 2026-03-05

**整理者**: [Rpg636zjhi](https://github.com/rpg636zjhi)

## 许可证

本文档采用 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 许可协议，转载请注明出处。