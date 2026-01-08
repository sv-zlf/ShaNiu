# 傻妞AI智能手机助手

基于Android平台的智能语音助手，通过语音唤醒词激活，支持语音识别、语音输出，并能根据用户指令执行通用问答或自动化手机操作（如自动点外卖）。

## 功能特性

- **语音唤醒**：通过`傻妞`唤醒词快速激活助手（<500ms响应）
- **语音识别**：实时语音转文字，支持流式识别和降噪
- **语音合成**：定制化的`傻妞`音色，支持情感语音合成
- **智能对话**：基于大模型的多轮对话，支持上下文记忆
- **自动化操作**：通过无障碍服务自动操作手机，完成点外卖、订票等任务
- **设备控制**：打开应用、调节音量/亮度、切换WiFi等

## 技术栈

- **开发语言**：Kotlin
- **Android版本**：API 31+
- **构建工具**：Gradle 7.3.0, Kotlin 1.8.0
- **架构模式**：MVVM + Clean Architecture
- **语音技术**：Porcupine（唤醒）+ 阿里云ASR/TTS
- **AI服务**：通义千问/GLM-4
- **无障碍服务**：Android AccessibilityService

## 项目结构

```
app/
  src/main/java/com/fx/shaniu/
    module/           # 核心模块
      wakeup/       # 语音唤醒
      asr/          # 语音识别
      tts/          # 语音合成
      chat/         # 对话管理
      autoglm/      # 自动化操作
      device/       # 设备控制
    service/          # Android服务
      WakeUpService.kt
      ShaniuAccessibilityService.kt
    manager/          # 管理器
    ui/              # UI组件
    data/            # 数据层
  src/test/            # 单元测试
  src/androidTest/      # 集成测试
  res/                 # 资源文件
docs/
  DESIGN.md            # 详细设计文档
```

## 快速开始

### 环境要求

- Android Studio Arctic Fox或更高版本
- JDK 11或更高版本
- Android SDK API 31+
- Gradle 7.3.0+

### 安装步骤

1. 克隆仓库
```bash
git clone https://github.com/your-org/shaniu.git
cd shaniu
```
2. 配置API密钥

在`local.properties`中添加：
```properties
# 阿里云ASR
alibaba.asr.app.id=your_app_id
alibaba.asr.app.key=your_app_key
# 阿里云TTS
alibaba.tts.app.id=your_app_id
alibaba.tts.app.key=your_app_key
# 大模型API
llm.api.endpoint=your_llm_endpoint
llm.api.key=your_llm_key
```
3. 构建项目
```bash
./gradlew assembleDebug
```
4. 安装到设备
```bash
./gradlew installDebug
```

### 权限配置

首次启动时需要授予以下权限：
- 录音权限（语音识别）
- 无障碍服务（自动化操作）
- 悬浮窗权限（系统控制）
- 修改系统设置权限
- 定位权限（位置相关功能）

详细配置请查看`docs/DESIGN.md`第4节。

## 使用说明

### 语音唤醒

1. 说出`傻妞`唤醒助手
2. 助手响应：`我在，请吩咐`
3. 说出指令：`今天北京天气怎么样？`

### 通用问答

```
用户：傻妞
助手：我在，请吩咐
用户：帮我翻译一下Hello World
助手：Hello World的中文意思是：你好，世界
```

### 自动化操作

```
用户：傻妞
助手：我在，请吩咐
用户：帮我点一份麻辣烫送到公司
助手：好的，正在为您下单...
[自动操作美团外卖]
助手：已为您下单麻辣烫，预计30分钟送达到公司
```

## 开发指南

### 构建命令

```bash
./gradlew assembleDebug       # 构建Debug版本
./gradlew assembleRelease     # 构建Release版本
./gradlew clean             # 清理构建产物
./gradlew check             # 运行所有检查（测试+Lint）
```

### 运行测试

```bash
./gradlew test                              # 运行单元测试
./gradlew connectedAndroidTest             # 运行仪器化测试
./gradlew :app:test --tests "*ASRModuleTest*"  # 运行特定模块测试
```

### 代码风格

项目遵循Kotlin编码规范，使用4空格缩进。

```bash
./gradlew lint    # 运行Lint检查
```

详细的开发指南请查看`AGENTS.md`。

## 安全说明

- 所有敏感数据使用Android KeyStore加密存储
- 语音数据默认本地处理，不上传云端（需用户授权）
- 支付等敏感操作需要人工二次确认
- 无障碍服务仅处理白名单应用
- 完整安全设计请查看`docs/DESIGN.md`第5节

## 文档

- [详细设计文档](docs/DESIGN.md) - 完整的技术架构和实现方案
- [贡献指南](AGENTS.md) - 开发规范和提交指南

## 贡献

欢迎贡献代码！请遵循以下步骤：

1. Fork本仓库
2. 创建特性分支（`git checkout -b feature/AmazingFeature`）
3. 提交更改（`git commit -m "feat(asr): Add streaming support"`）
4. 推送到分支（`git push origin feature/AmazingFeature`）
5. 提交Pull Request

提交规范请遵循[Conventional Commits](https://www.conventionalcommits.org/)。

## 开发路线

### Phase 1: 基础功能（进行中）
- [x] 项目架构搭建
- [ ] 语音唤醒模块实现
- [ ] 语音识别模块集成
- [ ] 语音合成模块集成
- [ ] 基础UI开发

### Phase 2: 通用场景（计划中）
- [ ] 对话历史管理
- [ ] 上下文记忆实现
- [ ] 意图识别优化

### Phase 3: 自动化核心（计划中）
- [ ] AccessibilityService开发
- [ ] UI元素定位器实现
- [ ] 操作执行器开发
- [ ] 应用适配器框架

### Phase 4: 场景扩展（计划中）
- [ ] 更多应用适配
- [ ] 复杂任务编排

## 许可证

本项目采用 [MIT License](LICENSE) 开源。

## 联系方式

- 项目主页：https://github.com/your-org/shaniu
- 问题反馈：https://github.com/your-org/shaniu/issues
- 邮箱：your-email@example.com

---

**傻妞AI智能手机助手** - 让手机更懂你！
