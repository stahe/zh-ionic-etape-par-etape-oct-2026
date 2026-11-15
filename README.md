# Ionic 移动框架分步入门

📖 **阅读教程：[https://stahe.github.io/zh-ionic-etape-par-etape-oct-2026/](https://stahe.github.io/zh-ionic-etape-par-etape-oct-2026/)**

本课程教你使用 [Ionic](https://ionicframework.com) 9 框架，结合 [Angular](https://angular.dev) 22 和 [Capacitor](https://capacitorjs.com) 8，构建一个**移动**应用：这是一个 Android 应用，其界面**在手机上**渲染，数据来自服务器的 JSON。它是一个 Web 应用（HTML、CSS、TypeScript），由 Capacitor 打包成 Android 应用——同一份代码也可以在浏览器中运行。

课程沿用 [Flutter 移动框架分步入门](https://stahe.github.io/zh-flutter-etape-par-etape-sept-2026/)课程的大纲（以及 [React](https://stahe.github.io/zh-react-etape-par-etape-sept-2026/)、[Vue.js](https://stahe.github.io/zh-vuejs-etape-par-etape-sept-2026/) 和 [Angular](https://stahe.github.io/zh-angular-etape-par-etape-sept-2026/) 课程）：同一个服务器、同样的界面，只是用 Ionic 的风格为智能手机编写。

| Flutter 课程 | Ionic 课程 |
|---|---|
| Dart 语言 | TypeScript |
| Flutter 引擎渲染每一个像素 | 外观接近原生的 Web 组件（Android 上为 Material Design），由手机的 WebView 显示 |
| widget、`build()` 方法、`setState` | Angular 组件、HTML 模板、信号（`signal`、`computed`、`effect`） |
| `InheritedWidget`、provider（`ChangeNotifier`） | Angular 依赖注入、带信号的服务 |
| `Form` 和 `FormField` | Angular 响应式表单 |
| go_router、`ShellRoute` + `NavigationBar` | Angular 路由器、`ion-tabs`、`CanActivateFn` 守卫 |
| `Future`、`Stream` | `Promise`、`Observable`（RxJS）、`resource()` |
| `shared_preferences` | `@capacitor/preferences` |
| 处理 Cookie 的 HTTP 客户端（在手机上） | `HttpClient`；在手机上，由 CapacitorHttp 插件设置并返回 Cookie |

不过，服务器保持不变：它就是 Flutter、React、Vue.js 和 Angular 客户端已经在使用的 **RdvMedecins** 应用的 JSON 服务器。

## 方法：大量简短示例，然后是案例研究

课程围绕 **25 个简短示例**展开，每个示例只聚焦一个概念，其编号（以及展示的界面）与 Flutter、React、Vue.js 和 Angular 课程中的相同。它们构成同一个 Ionic 项目：只需一条 `npm install` 命令，然后 `npm start`；你可以从应用菜单（或通过其 URL：http://localhost:8100/ex05）选择示例。

| 章节 | 内容 | 示例 |
|---|---|---|
| 入门 | Ionic + Angular + Capacitor 项目、Ionic 组件、信号和计算属性、模板（`@if`、`@for`）、事件、所有字段类型、reducer、验证、响应式表单、格式化（管道、`Intl`） | 01–09 |
| 组件 | 输入（`input`）、输出（`output`、`model`）、内容投影（`ng-content`、`ng-template`）、泛型数组、生命周期、依赖注入、指令和动画、确认对话框（`AlertController`） | 10–16 |
| 路由 | Angular 路由器与 Ionic：路由、参数、`ion-tabs`、懒加载、404 页面、守卫 | 17–18 |
| 异步操作与共享状态 | `Promise`、`Observable`、异步生成器、定时器、防抖、过期响应（`switchMap`）、`resource()`、信号服务、`@capacitor/preferences`、深色模式 | 19–20 |
| 国际化 | JSON 字典、参数、复数（`Intl.PluralRules`）、日期、金额、本地化日历（`ion-datetime`） | 21 |
| 服务器：一个黑盒 | 搭建 JSON 服务器、其 API、48 个 `curl` 示例、移动客户端有哪些不同 | – |
| 与服务器通信 | `HttpClient`、开发代理、服务器地址（模拟器、手机、Web）、CapacitorHttp 与令牌 Cookie、API 访问层、视图模型、附加到字段的服务器错误 | 22–25 |

每个示例都附有完整代码（逐行注释）以及运行时的截图（由 [Playwright](https://playwright.dev) 脚本自动生成，位于 `captures/` 文件夹）。

## 服务器：一个黑盒

该服务器就是前面课程中的 NestJS 服务器，其控制器返回 **JSON**。本课程把它当作一个**黑盒**：我们安装它、研究它的 API，并用 `curl` 查询它——但不需要阅读它的代码（代码已提供并带有注释，供感兴趣的读者查看）。

- 所有错误都采用相同的格式：`{ "statusCode": 409, "cle": "ERRORS. LOGIN_TAKEN", "params": {...}, "fields": {...} }`——是翻译**键**，绝不是纯文本；
- 通过 `httpOnly` Cookie 中的 JWT 令牌进行身份验证：在浏览器中由浏览器保存；在手机上由 CapacitorHttp 插件处理；
- 验证码（CAPTCHA）的“测试模式”，使你可以用 `curl` 查询 API（并自动生成截图）。


## 案例研究：RdvMedecins Ionic 客户端

一个完整的**诊所预约**应用，**所有**文件都已列出并带有注释。

- **现代 Ionic 和 Angular**：独立组件、信号、不使用 Zone.js 的应用、新的模板控制流、`inject()`、函数式 HTTP 拦截器、`CanActivateFn` 守卫，以及分层架构（API 访问、状态、UI）。
- **三种角色**：`ADMIN`（管理医生和客户）、`DOCTOR`（安排和取消预约）、`USER`（患者：为自己预约、管理自己的账户）。
- **隐私**：患者绝不会收到其他患者的姓名——服务器不会发送它们。
- **整个屏幕状态都包含在 URL 中**：`/agenda?idMedecin=1&jour=2026-10-13&reserver=7`；在手机上，“返回”按钮会关闭预约窗口；在 Web 上，F5 和后退/前进均可使用。
- **服务器端验证**：表单在每个字段下方显示 API 返回的错误；乐观锁、重名、用户名已被占用……
- **会话**：启动时重新建立（`GET /api/auth/moi`），过期处理集中在一处（401 响应，在拦截器中）。
- **适合移动设备**：抽屉菜单（`ion-menu`），在大屏幕上始终可见（`ion-split-pane`），用列表代替表格。
- **法语 / 英语**，包括日历。
- **部署**：编译后的 Web 版本，由 JSON 服务器本身提供服务，以及 Android 应用（APK）。

## 仓库内容

```
exemples_ionic/            25 个简短示例（一个 Ionic 项目）
rdvmedecins-nestjs-json/   RdvMedecins JSON 服务器（“黑盒”）
rdvmedecins_ionic/         案例研究的 Ionic 客户端
```

每个项目的 `android/` 文件夹未提供：它由 `npx cap add android` 创建（见课程）。

## 技术

Ionic 9 · Angular 22 · Capacitor 8 · TypeScript 6 · RxJS 7 · @capacitor/app · @capacitor/preferences · ionicons · Playwright（截图）· 服务器端：NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## 先决条件

- TypeScript 和 Angular 的基础知识，参见课程[通过示例入门 TypeScript](https://stahe.github.io/typescript-sept-2026/)和 [Angular 框架分步入门](https://stahe.github.io/angular-etape-par-etape-sept-2026/)；不过，所用到的概念会在各个示例中复习。
- Node.js 24（或 22.22.3 及更高版本，Angular CLI 22 需要）、Visual Studio Code 及其 Angular Language Service 和 Ionic 扩展、Chrome、Android Studio（用于 Android SDK 和模拟器）或一部 Android 手机；JSON 服务器需要一个 MySQL 服务器（例如 Windows 上的 Laragon）。课程中提供了安装说明。

## 作者

本课程、其示例、Ionic 客户端以及 JSON 服务器的适配由 [Anthropic](https://www.anthropic.com) 的 AI **Claude** 编写（2026 年 10 月），应 **Serge Tahé** 的请求完成。
