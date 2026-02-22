


# 校园社团活动管理系统 - 项目文档

## 目录
- [项目概述](#项目概述)
- [技术栈](#技术栈)
- [系统架构](#系统架构)
- [功能模块](#功能模块)
- [数据库设计](#数据库设计)
- [安装部署](#安装部署)
- [API接口](#api接口)
- [WebSocket通信](#websocket通信)
- [开发规范](#开发规范)
- [常见问题](#常见问题)

---
## 项目预览：
### 部分前端页面：
<img width="1914" height="1381" alt="image" src="https://github.com/user-attachments/assets/37b02fbf-f9ba-49a4-8f7d-cdf4acdf1caf" />
<img width="1914" height="1780" alt="image" src="https://github.com/user-attachments/assets/0d215562-86ff-4792-8edd-47386d8ccb3b" />
<img width="1914" height="1780" alt="image" src="https://github.com/user-attachments/assets/f19e9b03-23cd-4101-abb7-96086eaeabfa" />
<img width="1911" height="646" alt="image" src="https://github.com/user-attachments/assets/7553c53b-502b-463d-9703-f2c64fccedbc" />
<img width="1914" height="1780" alt="image" src="https://github.com/user-attachments/assets/fbde81dd-36ae-42e6-9bfd-a43eb725cb73" />

### 部分后台管理页面：
![社团管理后台_申请管理](https://github.com/user-attachments/assets/ccd3dabe-d853-4460-a459-aae91fe62e98)
<img width="1914" height="1780" alt="image" src="https://github.com/user-attachments/assets/4614ce99-4003-41dc-91e0-4055f7bf72c1" />
<img width="1914" height="1780" alt="image" src="https://github.com/user-attachments/assets/bc91a458-53a1-4bf4-ae3f-3da958af59cb" />
<img width="1914" height="1780" alt="image" src="https://github.com/user-attachments/assets/99f3fca9-cbff-43e9-bf99-0a5e1f4c461f" />
<img width="1914" height="1780" alt="image" src="https://github.com/user-attachments/assets/2d992aac-4c47-4619-a064-8cf979327fbe" />
<img width="1914" height="1780" alt="image" src="https://github.com/user-attachments/assets/4aa7d4e5-2552-4e44-8070-1ec7a690f9a4" />

## 源码购买联系方式

![15ffea3fd0043ae3e7a188c1b611d6b1](https://github.com/user-attachments/assets/feb164db-fc0f-4e9c-95cf-c2f9e45609f4)

## 购买包含内容：

<img width="306" height="239" alt="image" src="https://github.com/user-attachments/assets/3cc357b3-e51f-4aa0-93a3-36a33e54f01e" />


## 项目概述

### 项目简介
校园社团活动管理系统是一个基于Django开发的综合性社团管理平台，旨在为高校社团提供完整的线上管理解决方案。系统支持社团创建、活动发布、成员管理、实时聊天、活动签到等核心功能。

### 主要特性
- 🏢 **社团管理**：社团创建申请、信息维护、成员管理
- 📅 **活动管理**：活动发布、报名审核、签到管理
- 💬 **实时聊天**：基于WebSocket的群聊功能，自动创建社团/活动群组
- 🔔 **通知系统**：实时通知推送，申请审批状态更新
- 👥 **权限管理**：多角色权限控制（学生、社团管理员、系统管理员）
- 📊 **数据统计**：社团数据、活动数据、签到统计可视化
- 🎨 **现代化UI**：基于Tailwind CSS的响应式设计

### 项目目标
- 简化社团管理流程，提高管理效率
- 促进社团成员之间的交流与互动
- 为学校提供统一的社团管理平台
- 提供数据支持，辅助决策分析

---

## 技术栈

### 后端技术
- **框架**: Django 5.2
- **Python版本**: 3.12
- **WebSocket**: Django Channels 4.3.2
- **异步支持**: Channels Redis 4.3.0
- **数据库**: SQLite
- **图像处理**: Pillow
- **HTTP请求**: Requests

### 前端技术
- **CSS框架**: Tailwind CSS (CDN)
- **图标库**: Font Awesome 6
- **JavaScript框架**: Alpine.js 3.x
- **实时通信**: WebSocket API

### 开发工具
- **ASGI服务器**: Daphne
- **版本控制**: Git
- **虚拟环境**: venv

---

## 系统架构

### 整体架构
```
┌─────────────────────────────────────────────────────────┐
│                      前端层 (Templates)                   │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐│
│  │ 社团广场  │  │ 活动中心  │  │ 聊天系统  │  │ 管理后台  ││
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘│
└─────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────┐
│                    业务逻辑层 (Views)                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐│
│  │ 社团管理  │  │ 活动管理  │  │ 用户管理  │  │ 通知管理  ││
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘│
└─────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────┐
│                  WebSocket层 (Consumers)                  │
│  ┌──────────────────┐       ┌──────────────────┐       │
│  │  ChatConsumer    │       │ NotificationConsumer│     │
│  └──────────────────┘       └──────────────────┘       │
└─────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────┐
│                    数据层 (Models)                        │
│  ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐     │
│  │User│ │Club│ │Activity│ │Chat│ │Notification│ ...   │
│  └────┘ └────┘ └────┘ └────┘ └────┘ └────┘ └────┘     │
└─────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────┐
│                    数据库 (SQLite)                        │
└─────────────────────────────────────────────────────────┘
```

### 应用模块划分
```
club_management/          # 项目主目录
├── accounts/            # 用户账户模块
├── clubs/              # 社团管理模块
├── activities/         # 活动管理模块
├── chat/               # 聊天系统模块
├── system_admin/       # 系统管理模块
└── club_management/    # 项目配置
```

---

## 功能模块

### 1. 用户管理模块 (accounts)

#### 功能列表
- ✅ 用户注册/登录/登出
- ✅ 个人资料管理（头像、学号、手机号等）
- ✅ 密码修改
- ✅ 通知中心
- ✅ 角色权限管理

#### 核心模型
```python
class User(AbstractUser):
    role = models.CharField(max_length=20, choices=ROLE_CHOICES)
    student_id = models.CharField(max_length=20, unique=True)
    phone = models.CharField(max_length=11)
    avatar = models.ImageField(upload_to='avatars/')
```

#### 主要视图
- `register()` - 用户注册
- `profile()` - 个人中心
- `update_profile()` - 更新资料
- `change_password()` - 修改密码
- `upload_avatar()` - 上传头像
- `notifications()` - 通知列表

---

### 2. 社团管理模块 (clubs)

#### 功能列表
- ✅ 社团列表展示（分类筛选、搜索）
- ✅ 社团详情页
- ✅ 社团创建申请（需系统管理员审批）
- ✅ 社团入会申请（需社团管理员审批）
- ✅ 社团信息编辑（名称、描述、分类、Logo、封面）
- ✅ 成员管理（添加、移除、角色变更）
- ✅ 社团管理后台

#### 核心模型
```python
class Club(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()
    category = models.CharField(max_length=50, choices=CATEGORY_CHOICES)
    president = models.ForeignKey(User, on_delete=models.CASCADE)
    logo = models.ImageField(upload_to='clubs/logos/')
    cover_image = models.ImageField(upload_to='clubs/covers/')
    status = models.CharField(max_length=20, choices=STATUS_CHOICES)

class Membership(models.Model):
    club = models.ForeignKey(Club, on_delete=models.CASCADE)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    role = models.CharField(max_length=20, choices=ROLE_CHOICES)
    status = models.CharField(max_length=20, choices=STATUS_CHOICES)
```

#### 社团管理后台功能
- **概览页**：社团统计数据、活动统计、待处理申请
- **社团管理**：查看管理的所有社团、申请创建新社团
- **活动管理**：查看管理的所有活动、创建新活动
- **申请管理**：审批社团入会申请、活动报名申请
- **签到管理**：查看活动签到情况、生成签到码

---

### 3. 活动管理模块 (activities)

#### 功能列表
- ✅ 活动列表展示（状态筛选、搜索）
- ✅ 活动详情页
- ✅ 活动创建（需社团管理员权限）
- ✅ 活动报名（需审批）
- ✅ 活动签到（签到码验证）
- ✅ 活动评论
- ✅ 活动封面图（Unsplash API）

#### 核心模型
```python
class Activity(models.Model):
    club = models.ForeignKey(Club, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    description = models.TextField()
    location = models.CharField(max_length=200)
    start_time = models.DateTimeField()
    end_time = models.DateTimeField()
    max_participants = models.IntegerField()
    cover_image = models.URLField()
    check_in_code = models.CharField(max_length=10)
    status = models.CharField(max_length=20, choices=STATUS_CHOICES)

class ActivityRegistration(models.Model):
    activity = models.ForeignKey(Activity, on_delete=models.CASCADE)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    reason = models.TextField()
    status = models.CharField(max_length=20, choices=STATUS_CHOICES)
    reviewer = models.ForeignKey(User, on_delete=models.SET_NULL)

class ActivityCheckIn(models.Model):
    registration = models.OneToOneField(ActivityRegistration, on_delete=models.CASCADE)
    checked_in_at = models.DateTimeField(auto_now_add=True)
```

#### 签到流程
1. 活动管理员生成6位数字签到码
2. 活动开始后，用户输入签到码
3. 系统验证签到码并记录签到时间
4. 管理员可查看签到统计和详情

---

### 4. 聊天系统模块 (chat)

#### 功能列表
- ✅ 实时群聊（WebSocket）
- ✅ 自动创建社团群组
- ✅ 自动创建活动群组
- ✅ 消息发送/接收
- ✅ 未读消息提醒
- ✅ 聊天室列表
- ✅ 消息历史记录

#### 核心模型
```python
class ChatRoom(models.Model):
    name = models.CharField(max_length=200)
    room_type = models.CharField(max_length=20, choices=TYPE_CHOICES)
    club = models.ForeignKey(Club, on_delete=models.CASCADE)
    activity = models.ForeignKey(Activity, on_delete=models.CASCADE)
    avatar = models.ImageField(upload_to='chat/avatars/')

class ChatRoomMember(models.Model):
    room = models.ForeignKey(ChatRoom, on_delete=models.CASCADE)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    last_read_at = models.DateTimeField()

class ChatMessage(models.Model):
    room = models.ForeignKey(ChatRoom, on_delete=models.CASCADE)
    sender = models.ForeignKey(User, on_delete=models.CASCADE)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
```

#### WebSocket通信
- **连接URL**: `/ws/chat/<room_id>/`
- **消息格式**:
```json
{
    "type": "chat_message",
    "message": {
        "id": 123,
        "sender": "username",
        "content": "消息内容",
        "timestamp": "2026-02-22 20:00:00"
    }
}
```

#### 自动群组创建
- **社团群组**: 用户加入社团时自动加入社团群
- **活动群组**: 用户报名活动通过后自动加入活动群

---

### 5. 通知系统模块

#### 功能列表
- ✅ 实时通知推送（WebSocket）
- ✅ 通知列表查看
- ✅ 标记已读/未读
- ✅ 通知分类（社团申请、活动报名、评论等）
- ✅ 通知跳转链接

#### 核心模型
```python
class Notification(models.Model):
    recipient = models.ForeignKey(User, on_delete=models.CASCADE)
    sender = models.ForeignKey(User, on_delete=models.CASCADE)
    notification_type = models.CharField(max_length=30, choices=TYPE_CHOICES)
    title = models.CharField(max_length=200)
    content = models.TextField()
    link = models.CharField(max_length=500)
    is_read = models.BooleanField(default=False)
```

#### 通知类型
- `club_application` - 社团入会申请
- `club_creation` - 创建社团申请
- `activity_registration` - 活动报名申请
- `activity_comment` - 活动评论
- `system` - 系统通知

#### 通知推送流程
1. 触发事件（如申请审批）
2. 创建通知记录
3. 通过WebSocket推送给用户
4. 前端显示通知弹窗和红点提醒

---

### 6. 系统管理模块 (system_admin)

#### 功能列表
- ✅ 系统概览（统计数据）
- ✅ 社团管理（全部社团的增删改查）
- ✅ 活动管理（全部活动的增删改查）
- ✅ 申请管理（审批创建社团申请）
- ✅ 用户管理（全部用户的增删改查）

#### 权限控制
- 仅超级管理员（`is_superuser=True`）可访问
- 使用 `@user_passes_test(is_superuser)` 装饰器

#### 主要视图
- `admin_home()` - 系统概览
- `club_management()` - 社团管理
- `activity_management()` - 活动管理
- `application_management()` - 申请管理
- `user_management()` - 用户管理

---

## 数据库设计

### ER图概览
```
User ──┬─── Membership ─── Club ──┬─── Activity
       │                          │
       ├─── ClubApplication        ├─── ActivityRegistration
       │                          │
       ├─── ClubCreationApplication├─── ActivityCheckIn
       │                          │
       ├─── ChatMessage            └─── ActivityComment
       │
       ├─── ChatRoomMember ─── ChatRoom
       │
       └─── Notification
```

### 主要数据表

#### 用户相关
- `accounts_user` - 用户表
- `accounts_notification` - 通知表

#### 社团相关
- `clubs_club` - 社团表
- `clubs_membership` - 成员关系表
- `clubs_clubapplication` - 入会申请表
- `clubs_clubcreationapplication` - 创建社团申请表

#### 活动相关
- `activities_activity` - 活动表
- `activities_activityregistration` - 报名表
- `activities_activitycheckin` - 签到表
- `activities_activitycomment` - 评论表

#### 聊天相关
- `chat_chatroom` - 聊天室表
- `chat_chatroommember` - 聊天室成员表
- `chat_chatmessage` - 消息表

---

## 安装部署

### 环境要求
- Python 3.7.5+
- pip
- virtualenv

### 安装步骤

#### 1. 克隆项目
```bash
git clone <repository-url>
cd 校园社团活动管理系统1
```

#### 2. 创建虚拟环境
```bash
python -m venv venv
```

#### 3. 激活虚拟环境
```bash
# Windows
venv\Scripts\activate

# Linux/Mac
source venv/bin/activate
```

#### 4. 安装依赖
```bash
pip install -r requirements.txt
```

#### 5. 配置数据库
```bash
python manage.py makemigrations
python manage.py migrate
```

#### 6. 创建超级管理员
```bash
python manage.py createsuperuser
```

#### 7. 收集静态文件（生产环境）
```bash
python manage.py collectstatic
```

#### 8. 启动开发服务器
```bash
# 使用Daphne（支持WebSocket）
daphne -b 127.0.0.1 -p 8000 club_management.asgi:application

# 或使用Django开发服务器（不支持WebSocket）
python manage.py runserver
```

#### 9. 访问系统
- 前台: http://127.0.0.1:8000/
- 管理后台: http://127.0.0.1:8000/admin/

---

## API接口

### 用户相关
| 接口 | 方法 | 说明 |
|------|------|------|
| `/accounts/register/` | POST | 用户注册 |
| `/accounts/login/` | POST | 用户登录 |
| `/accounts/logout/` | POST | 用户登出 |
| `/accounts/profile/update/` | POST | 更新资料 |
| `/accounts/profile/change-password/` | POST | 修改密码 |
| `/accounts/profile/upload-avatar/` | POST | 上传头像 |
| `/accounts/notifications/` | GET | 通知列表 |
| `/accounts/api/notifications/unread-count/` | GET | 未读通知数 |

### 社团相关
| 接口 | 方法 | 说明 |
|------|------|------|
| `/clubs/` | GET | 社团列表 |
| `/clubs/<id>/` | GET | 社团详情 |
| `/clubs/<id>/apply/` | POST | 申请加入社团 |
| `/dashboard/<id>/edit/` | POST | 编辑社团信息 |
| `/dashboard/<id>/upload-logo/` | POST | 上传Logo |
| `/dashboard/<id>/upload-cover/` | POST | 上传封面 |
| `/dashboard/<id>/members/add/` | POST | 添加成员 |
| `/dashboard/<id>/members/<mid>/update-role/` | POST | 更新成员角色 |
| `/dashboard/<id>/members/<mid>/remove/` | POST | 移除成员 |

### 活动相关
| 接口 | 方法 | 说明 |
|------|------|------|
| `/activities/` | GET | 活动列表 |
| `/activities/<id>/` | GET | 活动详情 |
| `/activities/<id>/register/` | POST | 报名活动 |
| `/activities/<id>/check-in/` | POST | 活动签到 |
| `/activities/<id>/comment/` | POST | 发表评论 |
| `/dashboard/<cid>/activities/<aid>/registrations/<rid>/approve/` | POST | 通过报名 |
| `/dashboard/<cid>/activities/<aid>/registrations/<rid>/reject/` | POST | 拒绝报名 |
| `/dashboard/<cid>/activities/<aid>/generate-check-in-code/` | POST | 生成签到码 |

### 聊天相关
| 接口 | 方法 | 说明 |
|------|------|------|
| `/chat/` | GET | 聊天室列表 |
| `/chat/<room_id>/` | GET | 聊天室详情 |
| `/chat/api/unread-count/` | GET | 未读消息数 |
| `/ws/chat/<room_id>/` | WebSocket | 聊天WebSocket |

---

## WebSocket通信

### 通知WebSocket

#### 连接
```javascript
const wsUrl = 'ws://127.0.0.1:8000/ws/notifications/';
const socket = new WebSocket(wsUrl);
```

#### 接收消息
```javascript
socket.onmessage = function(e) {
    const data = JSON.parse(e.data);
    
    if (data.type === 'notification') {
        // 新通知
        console.log(data.notification);
    } else if (data.type === 'unread_count') {
        // 未读数量
        console.log(data.count);
    }
};
```

#### 标记已读
```javascript
socket.send(JSON.stringify({
    type: 'mark_read',
    notification_id: 123
}));
```

### 聊天WebSocket

#### 连接
```javascript
const roomId = 1;
const wsUrl = `ws://127.0.0.1:8000/ws/chat/${roomId}/`;
const socket = new WebSocket(wsUrl);
```

#### 发送消息
```javascript
socket.send(JSON.stringify({
    message: '你好，世界！'
}));
```

#### 接收消息
```javascript
socket.onmessage = function(e) {
    const data = JSON.parse(e.data);
    console.log(data.message);
};
```

---


### 数据库迁移
```bash
# 创建迁移文件
python manage.py makemigrations

# 应用迁移
python manage.py migrate

# 查看迁移状态
python manage.py showmigrations
```

### 静态文件管理
- 开发环境：使用 `STATICFILES_DIRS`
- 生产环境：使用 `collectstatic` 收集到 `STATIC_ROOT`

---

#




