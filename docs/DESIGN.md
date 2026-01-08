# 傻妞AI智能手机助手 - 开发设计方案

## 1. 项目概述

### 1.1 项目简介
傻妞AI智能手机助手是一款基于Android平台的智能语音助手，通过唤醒词激活，支持语音识别、语音输出，并根据用户指令执行通用对话问答或自动化手机操作（如自动点外卖）。

### 1.2 核心功能
1. 语音唤醒：通过唤醒词激活助手
2. 语音输出：按设定的傻妞音色进行语音反馈
3. 语音识别：识别用户的语音输入
4. 智能操作：
   - 通用场景：直接回答用户问题
   - 自动化场景：操作手机完成复杂任务（如点外卖）

### 1.3 技术栈
- 开发语言：Kotlin
- Android版本：API 31+
- 构建工具：Gradle 7.3.0, Kotlin 1.8.0
- 架构模式：MVVM + Clean Architecture

---

## 2. 系统架构设计

### 2.1 整体架构

用户界面层：主界面、设置界面、权限引导
业务逻辑层：语音管理、对话管理、自动化
数据层：本地存储、网络服务、设备接口
系统服务层：唤醒服务、无障碍服务、TTS引擎

### 2.2 模块划分

核心模块：
1. 语音唤醒模块 (WakeUpModule)
2. 语音识别模块 (ASRModule)
3. 语音合成模块 (TTSModule)
4. 对话管理模块 (ChatModule)
5. 自动化操作模块 (AutoGlmModule)
6. 设备控制模块 (DeviceControlModule)

支持模块：
1. 权限管理模块 (PermissionModule)
2. 配置管理模块 (ConfigModule)
3. 网络服务模块 (NetworkModule)
4. 本地存储模块 (StorageModule)

---

## 3. 核心功能模块设计

### 3.1 语音唤醒模块

#### 功能描述
- 持续监听唤醒词
- 低功耗运行（后台服务）
- 快速响应（<500ms）
- 误唤醒过滤

#### 技术方案

方案一：使用KWS（关键词检测）引擎
- 引擎：Porcupine (Picovoice) 或 TensorFlow Lite
- 本地模型，零延迟
- 支持自定义训练唤醒词

方案二：使用云服务
- 科大讯飞唤醒词API
- 阿里云语音唤醒
- 需要网络，但准确率更高

推荐方案：方案一（本地KWS）+ 方案二（云端验证）

---

### 3.2 语音识别模块 (ASR)

#### 功能描述
- 实时语音转文字
- 支持流式识别
- 降噪处理
- 多语言支持

#### 技术选型

推荐方案：阿里云实时语音识别

---

### 3.3 语音合成模块 (TTS)

#### 功能描述
- 自然语音输出
- 多种音色选择
- 情感语音合成
- 实时合成播放

推荐方案：阿里云语音合成（定制傻妞音色）

#### TTS配置选项
- voice：傻妞音色（xiaoxiao-neural 或自定义）
- style：风格（cheerful友好、gentle温柔等）
- rate：语速（-10 到 10）
- pitch：音调（-10 到 10）

---

### 3.4 对话管理模块

#### 功能描述
- 意图识别与分类
- 多轮对话管理
- 上下文记忆
- 通用问答能力

#### LLM选择
- 通义千问（阿里云）
- GLM-4（智谱AI）
- 或者本地部署：Ollama + Llama3

#### 意图类型

1. 通用问答：天气、新闻、百科、翻译、计算等
2. 自动化操作：点外卖、订票、打车、购物、导航等
3. 设备控制：打开应用、设置闹钟、调节亮度、播放音乐等
4. 系统指令：关机、重启、清理缓存等

---

### 3.5 自动化操作模块 (Auto-GLM)

这是核心创新模块。

#### 功能描述
- 理解用户的自动化操作意图
- 将复杂任务分解为可执行步骤
- 通过无障碍服务操作手机界面
- 执行结果验证与反馈

#### 技术架构

用户指令 -> 任务理解（LLM）-> 任务分解 -> 动作序列生成 -> 执行引擎 -> 设备操作 -> 结果反馈

#### 操作类型

- CLICK：点击
- SCROLL：滚动
- INPUT：输入文本
- SWIPE：滑动
- LONG_PRESS：长按
- WAIT：等待
- LAUNCH_APP：启动应用
- PRESS_BACK：返回
- PRESS_HOME：主页

#### UI元素定位策略

1. 文本匹配：通过元素文本查找
2. 资源ID匹配：通过android:id查找
3. 描述匹配：通过contentDescription查找
4. AI视觉辅助：使用视觉模型识别元素位置

#### 应用适配器

为每个目标应用创建专用适配器：
- MeituanAdapter：美团外卖
- ElemeAdapter：饿了么
- DidiAdapter：滴滴出行
- AmapAdapter：高德地图
- GenericAdapter：通用适配器

#### 典型场景：自动点外卖

执行流程：
1. 理解意图：点外卖
2. 选择平台：美团外卖
3. 打开应用
4. 搜索美食
5. 筛选排序（价格、距离）
6. 选择商家
7. 选择菜品
8. 提交订单
9. 确认支付
10. 反馈结果

---

### 3.6 设备控制模块

#### 功能描述
- 应用启动与关闭
- 系统设置控制（音量、亮度、WiFi等）
- 通知管理
- 定位服务
- 相机操作

#### 支持的操作

- openApp(packageName)：打开应用
- closeApp(packageName)：关闭应用
- setVolume(level)：设置音量
- setBrightness(level)：设置亮度
- toggleWiFi(enable)：切换WiFi
- getCurrentLocation()：获取当前位置

---

## 4. 权限管理与配置

### 4.1 必需权限列表

- 录音权限：RECORD_AUDIO
- 网络权限：INTERNET, ACCESS_NETWORK_STATE
- 无障碍服务：BIND_ACCESSIBILITY_SERVICE
- 系统权限：SYSTEM_ALERT_WINDOW, WRITE_SETTINGS
- 存储权限：READ/WRITE_EXTERNAL_STORAGE
- 前台服务：FOREGROUND_SERVICE, FOREGROUND_SERVICE_MICROPHONE
- 定位权限：ACCESS_FINE_LOCATION, ACCESS_COARSE_LOCATION
- 其他：VIBRATE, WAKE_LOCK

### 4.2 特殊权限处理

1. 无障碍权限：需要用户在设置中手动开启
2. 悬浮窗权限：SYSTEM_ALERT_WINDOW
3. 修改系统设置：WRITE_SETTINGS

---

## 5. 安全与隐私设计

### 5.1 数据安全

- 使用Android KeyStore加密敏感数据
- 语音数据本地处理，不上传云端（除非用户授权）
- HTTPS通信加密
- 定期清理历史数据

### 5.2 隐私保护

1. 语音数据处理：默认本地处理，用户可选择云端模式，定期清理历史录音
2. 敏感操作二次确认：支付密码输入必须人工确认，大额转账需要用户手动确认，删除重要数据需要二次确认
3. 操作透明度：所有自动化操作可回放查看，提供操作日志，用户可随时暂停自动化
4. 数据脱敏：手机号、验证码、银行卡号等敏感信息自动脱敏

---

## 6. 开发阶段规划

### Phase 1：基础功能（2-3周）

目标：实现语音唤醒、识别、合成和基础对话

任务列表：
- [ ] 项目架构搭建
- [ ] 语音唤醒模块实现（KWS）
- [ ] 语音识别模块集成（阿里云ASR）
- [ ] 语音合成模块集成（阿里云TTS）
- [ ] 基础UI界面开发
- [ ] 简单对话能力（LLM集成）
- [ ] 权限管理实现

验收标准：
- 能够通过唤醒词唤醒
- 能够识别语音并转为文字
- 能够播放语音回复
- 能够进行简单对话

### Phase 2：通用场景（2-3周）

目标：完善对话能力，支持多轮对话

任务列表：
- [ ] 对话历史管理
- [ ] 上下文记忆实现
- [ ] 知识库集成（RAG）
- [ ] 意图识别优化
- [ ] 多轮对话管理
- [ ] 个性设置功能

验收标准：
- 支持多轮对话
- 能够记住上下文
- 能够回答常见问题

### Phase 3：自动化核心（4-6周）

目标：实现自动化操作核心功能

任务列表：
- [ ] AccessibilityService开发
- [ ] UI元素定位器实现
- [ ] 操作执行器开发
- [ ] 任务规划引擎实现
- [ ] 应用适配器框架
- [ ] 基础应用适配（美团、饿了么）
- [ ] 安全执行机制
- [ ] 错误处理与恢复

验收标准：
- 能够自动点外卖
- 能够自动完成至少2种应用操作
- 执行成功率 > 80%

### Phase 4：场景扩展（持续）

目标：扩展更多应用场景，提升用户体验

任务列表：
- [ ] 更多应用适配（滴滴、高德等）
- [ ] 复杂任务编排
- [ ] 用户反馈学习
- [ ] 性能优化
- [ ] 用户体验优化

---

## 7. 技术难点与解决方案

| 难点 | 解决方案 |
|------------|----------|
| 唤醒词误触发 | 双词唤醒+VAD+自适应阈值+声纹识别 |
| UI元素定位不稳定 | 多策略融合（文本/ID/描述）+AI视觉辅助+动态重试 |
| 应用版本兼容 | 适配器模式+动态检测+云端更新规则库 |
| 操作失败恢复 | 重试机制+状态快照+人工接管 |
| 实时性要求 | 边缘计算+预加载模型+并行处理 |
| 电池消耗 | 优化唤醒算法+后台策略+智能休唤醒 |
| 隐私保护 | 本地处理优先+数据加密+用户授权 |

---

## 8. 项目目录结构

app/
├── src/
│   └── main/
│       ├── java/com/fx/shaniu/
│       │   ├── ShaniuApplication.kt          # Application类
│       │   ├── MainActivity.kt               # 主界面
│       │   ├── service/                      # 服务
│       │   │   ├── WakeUpService.kt         # 唤醒服务
│       │   │   ├── ShaniuAccessibilityService.kt  # 无障碍服务
│       │   │   └── VoiceAssistantService.kt # 语音助手服务
│       │   ├── module/                       # 核心模块
│       │   │   ├── wakeup/                   # 唤醒模块
│       │   │   ├── asr/                      # 语音识别
│       │   │   ├── tts/                      # 语音合成
│       │   │   ├── chat/                     # 对话管理
│       │   │   ├── autoglm/                  # 自动化操作
│       │   │   │   └── adapter/             # 应用适配器
│       │   │   └── device/                   # 设备控制
│       │   ├── manager/                      # 管理器
│       │   ├── ui/                           # UI组件
│       │   └── data/                         # 数据层
│       └── res/                              # 资源文件
│           ├── layout/
│           ├── values/
│           └── xml/
│               └── accessibility_service_config.xml
└── build.gradle

---

## 9. 关键配置

### 9.1 依赖配置 (build.gradle)

dependencies {
    // AndroidX
    implementation "androidx.core:core-ktx:1.12.0"
    implementation "androidx.appcompat:appcompat:1.6.1"
    implementation "com.google.android.material:material:1.11.0"
    implementation "androidx.constraintlayout:constraintlayout:2.1.4"
    
    // Lifecycle
    implementation "androidx.lifecycle:lifecycle-runtime-ktx:2.7.0"
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.7.0"
    
    // Coroutines
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3"
    
    // 网络请求
    implementation "com.squareup.retrofit2:retrofit:2.9.0"
    implementation "com.squareup.retrofit2:converter-gson:2.9.0"
    implementation "com.squareup.okhttp3:okhttp:4.12.0"
    
    // 阿里云SDK
    implementation "com.alibaba:fastjson:2.0.43"
    implementation "com.aliyun:alicloud-android-nls-sdk:2.2.4"
}

---

## 10. 总结

本设计方案完整覆盖了傻妞AI智能手机助手的开发需求，包括：

1. 完整的架构设计：从系统层到应用层的清晰分层
2. 详细的功能模块：6大核心模块的设计与实现
3. 核心技术方案：语音技术、AI服务、自动化操作的完整实现
4. 开发路线图：4个阶段的详细规划
5. 安全与隐私：完善的安全保障机制

建议按照 Phase 1 -> Phase 2 -> Phase 3 -> Phase 4 的顺序逐步开发，每个阶段完成后进行充分测试，确保系统稳定性和可靠性。

---

文档版本：v1.0
最后更新：2026年1月8日
