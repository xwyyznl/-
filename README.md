# Course Interaction Platform
Course Interaction Platform 是面向高校课程互动与社区治理场景的一体化后端服务，核心覆盖三大业务域：  课程资源管理 — 资源上传/下载、作业布置与提交、作品设计评审、电子期刊、项目报告 社区治理平台 — 志愿任务发布/认领/签到、时间银行积分、意见征集、投票、活动动态、仪表盘统计 AI 智能辅助 — 阿里云百炼 RAG 知识库检索、通义千问对话、ASR 语音转文字、TTS 语音合成、用户画像标签自动生成 系统通过微信小程序端与用户交互，后端提供 RESTful API，支持 Docker 容器化部署与 Jenkins CI/CD 自动构建。
技术栈
类别	技术 / 框架
核心框架	Spring Boot 2.2.6 + Java 8
ORM	MyBatis-Plus（逻辑删除、自动填充）
数据库	MySQL（阿里云 RDS） + Flyway 版本化迁移
缓存	Redis
文件存储	阿里云 OSS
AI / LLM	阿里云 DashScope（通义千问）、百炼知识库 RAG
AI 语音	阿里云 ASR（语音转文字）、NLS（实时语音）、TTS
登录认证	微信小程序 OAuth + 验证码（Kaptcha）
工具库	Hutool 5.7.22、Lombok
部署	Docker（多阶段构建）+ Jenkins Pipeline
CI	Gitee → Jenkins → 阿里云 ACR
项目结构
course-interaction-platform/
├── common/                  # 公共模块：DTO、工具类、注解、异常处理
├── course-resourse/         # 核心业务模块（Spring Boot 应用入口）
│   ├── src/main/java/com/peking/courseresourse/
│   │   ├── controller/      # REST API 控制器（49 个）
│   │   ├── entity/          # 数据实体（66 个）
│   │   ├── service/         # 业务接口 + 实现
│   │   ├── dao/             # MyBatis-Plus Mapper
│   │   ├── config/          # CORS、异步、验证码、全局异常等配置
│   │   └── util/            # GeoUtil 等工具
│   │   └── support/         # OperationLogRecorder 等辅助
│   ├── src/main/resources/
│   │   ├── mapper/          # MyBatis XML 映射
│   │   ├── db/migration/    # Flyway SQL 迁移脚本（16 个版本）
│   │   └── application.yml  # 主配置文件
│   └── runtime-data/        # 本地文件上传目录（不入库）
├── renren-generator/        # 代码生成器（基于 renren-fast）
├── Dockerfile               # 多阶段构建：Maven 编译 → 精简 JRE 镜像
├── Jenkinsfile              # CI/CD 流水线（Gitee → Docker Build → ACR Push）
├── pom.xml                  # 父 POM（聚合模块）
└── mvnw                     # Maven Wrapper
核心功能模块
🏫 课程资源
功能	说明
资源管理	上传/下载课程资源文件，OSS 云存储
作业系统	布置作业、批量提交、多版本记录、评审打分
作品设计	产品设计提交与评审
电子期刊	期刊管理与下载
项目报告	项目报告提交与管理
问卷调查	问卷创建、数据收集与统计
案例分析	Case Table 管理
🏘️ 社区治理
功能	说明
志愿任务	发布/认领/签到/完成/逾期自动结算
时间银行积分	积分规则、积分流水、礼品兑换
意见征集	意见发布、官方回复
投票系统	社区投票与选项统计
社区动态	活动发布与签到记录
消息通知	社区消息推送
仪表盘	数据可视化：签到热力图、统计报表
冲突检测	AI 自动检测任务时间冲突
角色切换	居民/管理员角色申请与审批
关键词监控	AI 关键词命中追踪
🤖 AI 能力
功能	说明
AI 对话	通义千问多轮对话，上下文窗口管理
RAG 知识库	阿里云百炼知识库检索，向量化语义匹配
用户画像	AI 自动分析聊天记录生成用户标签
语音转文字	阿里云 ASR 文件转录 + 实时 NLS 流式识别
语音合成	阿里云 TTS 文字转语音
任务审核	AI 批量审核任务内容合规性
快速开始
前置条件
JDK 8+
MySQL 5.7+（或阿里云 RDS）
Redis
Maven 3.6+
本地运行
bash
# 1. 克隆仓库
git clone https://github.com/your-org/course-interaction-platform.git
cd course-interaction-platform

# 2. 修改配置（course-resourse/src/main/resources/application.yml）
#    - 数据库连接信息
#    - Redis 地址
#    - 阿里云 OSS / ASR / 百炼配置
#    - 微信小程序 AppID / Secret

# 3. 编译运行
./mvnw clean package -DskipTests=true
java -jar course-resourse/target/course-resourse-0.0.1-SNAPSHOT.jar
服务默认启动在 http://localhost:8080。

Docker 部署
bash
docker build -t course-interaction-platform .
docker run -d -p 8080:8080 \
  -e FILE_SAVE_DIR=/data/files \
  -e AI_API_KEY=your-api-key \
  -e WECHAT_MINIAPP_APPID=your-appid \
  -e WECHAT_MINIAPP_SECRET=your-secret \
  course-interaction-platform
环境变量
关键配置均可通过环境变量覆盖，无需硬编码到 application.yml：

环境变量	说明	默认值
FILE_SAVE_DIR	本地文件存储路径	./runtime-data
AI_API_ENDPOINT	AI API 地址	DashScope 兼容模式 endpoint
AI_API_KEY	AI API 密钥	—
AI_MODEL	LLM 模型	qwen-turbo
AI_KB_ENABLED	知识库开关	true
WECHAT_MINIAPP_APPID	微信小程序 AppID	—
WECHAT_MINIAPP_SECRET	微信小程序 Secret	—
ALIBABA_CLOUD_ACCESS_KEY_ID	阿里云 AK（知识库）	—
ALIBABA_CLOUD_ACCESS_KEY_SECRET	阿里云 SK（知识库）	—
API 概览
系统提供约 49 个 REST 控制器，覆盖完整业务流程。主要 API 路径：

/api/community/** — 社区治理相关（任务、积分、投票、意见、动态等）
/api/ai/** — AI 对话、ASR、TTS、知识库检索
/api/resource/** — 课程资源管理
/api/homework/** — 作业系统
/api/user/** — 用户注册/登录/角色管理
/api/file/** — 文件上传下载
/api/survey/** — 问卷调查
数据库迁移
项目使用 Flyway 管理 DB 版本（src/main/resources/db/migration/），共 16 个迁移脚本。首次部署 Flyway 自动执行；如需跳过可设置 spring.flyway.enabled=false 并手动初始化。

许可证
本项目采用 [LICENSE] 许可证，详见仓库根目录 LICENSE 文件。

参与贡献
Fork 本仓库
新建功能分支 (git checkout -b feature/your-feature)
提交改动 (git commit -m 'Add your feature')
推送分支 (git push origin feature/your-feature)
提交 Pull Request
