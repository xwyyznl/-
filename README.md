# Course Interaction Platform 🎓

> 北京大学课程互动与社区治理综合服务平台 — 集课程资源管理、社区志愿服务、AI 智能辅助于一体的 Spring Boot 后端系统。

## 项目简介

Course Interaction Platform 是面向高校课程互动与社区治理场景的一体化后端服务，核心覆盖三大业务域：

- **课程资源管理** — 资源上传/下载、作业布置与提交、作品设计评审、电子期刊、项目报告
- **社区治理平台** — 志愿任务发布/认领/签到、时间银行积分、意见征集、投票、活动动态、仪表盘统计
- **AI 智能辅助** — 阿里云百炼 RAG 知识库检索、通义千问对话、ASR 语音转文字、TTS 语音合成、用户画像标签自动生成

系统通过微信小程序端与用户交互，后端提供 RESTful API，支持 Docker 容器化部署与 Jenkins CI/CD 自动构建。

## 技术栈

| 类别       | 技术 / 框架                                |
|------------|---------------------------------------------|
| 核心框架   | Spring Boot 2.2.6 + Java 8                  |
| ORM        | MyBatis-Plus（逻辑删除、自动填充）           |
| 数据库     | MySQL（阿里云 RDS） + Flyway 版本化迁移      |
| 缓存       | Redis                                        |
| 文件存储   | 阿里云 OSS                                  |
| AI / LLM   | 阿里云 DashScope（通义千问）、百炼知识库 RAG  |
| AI 语音    | 阿里云 ASR（语音转文字）、NLS（实时语音）、TTS |
| 登录认证   | 微信小程序 OAuth + 验证码（Kaptcha）         |
| 工具库     | Hutool 5.7.22、Lombok                        |
| 部署       | Docker（多阶段构建）+ Jenkins Pipeline       |
| CI         | Gitee → Jenkins → 阿里云 ACR                |

## 项目结构

