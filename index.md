# 六大中心质控管理平台框架设计

## 背景
无。
## 名词解释
无。
## 整体设计
#### 技术选型
系统采用 SPA 单页应用模式，通过 javaScript 来判断当前浏览器地址 url，将内容渲染到页面上。
此项目技术包含但不限于：
- Vue3全家桶（vue3 + vue-router + pinia）
- TypeScript（静态类型检查）
- Element-plus（UI组件库）
- Vite（前端构建工具）
- axios（基于 Promise 的 HTTP 库）
- Echarts（可视化图表库）
- Sass(层叠样式表)
- Eslint（校验JS代码规则）
- prettier（代码格式化工具）
- wangEditor（富文本编辑器）
- Husky（pre-commit 钩子校验工具）
- Sentry（报错收集平台）

#### 项目框架
- dist 打包后文件输出目录
- node_modules 依赖包
- public 静态文件目录
- src 主程序目录
    - api 后端 API 接口封装
    - assets 图片目录
    - components 共用组件目录
    - style 样式目录
    - router 路由管理
    - store 状态管理
    - utils 工具管理
    - views 页面代码目录
    - App.vue vue入口文件
    - main.ts vue入口文件
- .eslintrc.js ESLint规则
- .gitignore git过滤文件
- .prettierrc 美化格式标准
- index.html 首页文件
- package.json 依赖包注册文件
- tsconfig.json TS配置文件
- vite.config.ts 打包编译文件
#### 总体布局
在技术选型的基础上，定义系统公有页面布局，共分为「顶栏容器、侧边栏容器、内容容器」三个容器区域。
系统将延续此布局来搭建页面，当前文档中涉及到的页面布局都以「内容容器」来做设计。
贴张图片 - https://element-plus.gitee.io/zh-CN/component/container.html
##### 顶栏容器「头部导航」
内容包含系统名称、可视化按钮、通知公告按钮、用户名和退出登录子菜单。
文件位置：components/Layout/TitleBar

##### 侧边栏容器「侧边栏菜单」
全页面左侧的菜单栏，实现了整体模块展示，点击切换到对应视图。
根据不同权限来区分侧边栏展示的菜单名称：
- 超级管理员；
- 质控管理员；
- 医院人员「管理、审核、填报」；
- 未注册成功；
```

```
文件位置：components/Layout/Sidebar
##### 内容区域
内容区域不做特殊定义，只留下空白布局，以系统中页面内容来填充。
## 全局配置
#### 全局 API 接口
axios 的配置；
请求超时时间为：5000 毫秒。
目录位置：src/api
#### 全局图片
目录位置：src/assets
根目录为全局使用图片；
子目录图片使用以「子目录」名字命名为文件夹，来做二级目录。
注：不考虑「雪碧图」，在 HTTP2 中多路复用已做优化。
注：菜单、按钮背景图标内联成 base64 data URL。 如果图片质量大小超出4kb（Vite默认4096），则找UI同事商议降低图片质量。
#### 全局样式
目录位置：src/style
全局的颜色文件 -> 所有颜色值的维护；
全局的样式文件 -> main.scss：维护全局的共有样式「字体大小、默认样式、默认颜色、全局布局样式」，全局设置的 class 都增加「basic-」标识符；
全局的按钮图标 -> 根据 assets 中按钮的图标，来做图片的统一处理。class前缀名采用「icon-」规范。
全局滚动条样式优化 -> 滚动条样式「背景色、宽度、圆角、选中色」来适配系统整体的颜色搭配。
常用间距、常用宽度、常用字体大小、A标签默认样式定义好，以供系统中其他模块使用。
#### 全局 Router
引入 vue-router 官方路由，创建 hash 的路由来处理 URL 跳转。全局「导航守卫」都在当前文件中编写逻辑。
注：涉及到独立页面的「导航守卫」逻辑，需要根据具体情况在具体页面编写。
目录位置：src/router
#### 全局 Store 数据
引入 vue 官方组件 pinia，来做系统的全局数据管理和维护。
目录位置：src/store
#### 全局 Utils
##### 自定义权限指令
根据当前用户权限集合，来区分判断是否有按钮级别的显示权限。
```
代码
```
文件位置：src/utils/btnPermissions.ts
##### 常量定义文件
系统中涉及到有业务意义的「数字、字母、可枚举变量」都以常量形式提供，减少代码编写时「1、2、3、a、b、c」心智负担。
贴代码；
文件位置：src/utils/cont.ts
##### 按需引入 Echarts 图表类型和组件
为减少打包体积，提升加载速度，按需引入 Echarts 图片的类型和组件，用到哪些组件在当前页面注册后并使用。
贴代码。
文件位置：src/utils/echarts.ts

#### 全局ElementPlus
elementPlus样式：
1.汉化；
2.修改默认的按钮、边框；
3.表格样式 -> 标题颜色、边框颜色。
4.确认对话框（删除、审核、提交）-> 操作行为统一，自定义 class 覆盖 elementPlus 默认样式即可。
文件位置：src/elementplus
## 全局对话框
#### 带 title 的组件
统一使用 Element-plus 库提供的 Dialog 对话框样式。需要做一些自定义的修改「圆角、X号、间距、边距」等。
#### 不带 title 的弹窗组件，实现以下功能：
1.支持显示「取消、确定」一个或者多个按钮，并对外提供多个方法；
2.支持自定义时间 -> 自动关闭；
3.支持默认传值，也支持插槽自定义内容；
组件位置：components/Toast/ToastDialog
#### 切换中心
自定义组件弹窗；
补充。
组件位置：components/Toast/SwitchCenter
## 全局分页
分页样式参考 element-plus UI库。
## 全局 toast 提示信息
提示信息全采用 element-plus 全局提示信息。

## 项目目录（views目录）
###### 注册、登录模块(login)
单独布局，不沿用总体布局
    登录(login)
    忘记密码(forget)
    审核、未通过、注册状态(hosptail_status)
    医院信息注册(hosptail_register)
    医院信息提交(hosptail_submit)
    > 登录模块共有组件「Setup步骤条」，待补充
###### 数据上报模块(report)
    ###### 自定义组件
    六大中心数据上报编辑页相似度高，编辑页面包含两个组件「顶部组件、编辑组件」。
    六大中心填报页面(edit)
    六大中心填报页面-查看页面公用(管理端多[审核记录和审核按钮])(info)
    数据列表(list)
    审核页面(verify)
    > 涉及到「填报模块的封装」和「查看页面的封装」
    #### 质控数据搜索栏
    血液专病模块 -> 注意：列表页有「质控数据填报」和「专病数据填报」
##### 血液专病模块(blood)
    列表(list)
    填报页(edit)
    详情页(info)
    > 填报页是否要分页面?
    > 搜索栏的封装

##### 普查表模块(census)
    列表(list)
    填报页(edit)
    详情页(info)

##### 通知公告(notice)
    ###### 文件上传大小限制
    公告列表(医院、管理端共用)(list)
    公告详情(医院、管理端共用)(info)
#### 数据看板(board)
    六大中心查看页面「医院端」(hospital)
    六大中心查看页面「管理端」(manage)
#### 系统设置(system)
    科室管理(医院端独有)(department)
    医院信息管理(医院端独有)(hospital)
    帐号管理(account)
    管理员设置(administrator)
    角色管理(role)
    修改密码(password)
    功能配置(setting)
#### 成员管理（**管理端**）(member)
    医院管理(hospital)
    - 列表(list)
    - 查看(info)
    - 权限中心(limit)
    管理员帐号(admin)
    - 列表(list)
    - 审核(examine)
#### 可视化模块(**医院端独有**)(visual)
    基本
###### 备注 - 可视化组件（视工期而定）
可视化组件可封装成公司可视化组件库，技术实现以 pnpm + monorepo。pnpm 内置了对单一存储库（也称为多包存储库、多项目存储库或单体存储库）的支持，可以创建一个 workspace 以将多个项目合并到一个仓库中。
具体的设计需要以「可视化组件库详情设计」文档提供。

### 第三方库
**Element-plus**

UI组件库，简单介绍...
需要做一个 汉化 配置，补充代码。

**Echarts**

补充饼图、线图、拆线图 目录和组件位置...
按需引入，贴上代码。讲下这样做的好处，在解决什么问题。

**Husky**

Husky 添加 Git 钩子，来验证代码是否符合规范。在 package.json 里 scripts 中定义的任何脚本，只需创建另一个具有匹配名称的脚本，并将「pre」或「post」添加到它们的开头。
统一规范代码编写标准，讲下这样做的好处，在解决什么问题。

**Prettier**

在编辑器点击保存的时候自动格式化代码，在提交代码 commit 的时候格式化代码的风格都是依据这个文件。在 VSCode 应用商店里增加 Prettier 插件，然后在 .vscode/setting.json 文件里直接配置让 VSCode 来遵循 .prettierrc 的规则。

**Jest单元测试**
Jest 是一款优雅、简洁的 JavaScript 测试框架。系统全局公用代码最好保持到 80% 的覆盖率。

**wangEditor**

补充资料...

**Sentry**
Sentry 是错误监控和收集工具，也可接入公司内部「ERK」，后期通过平台分析常见问题。

### 体验优化
+ 加载页面的时候，采用 Loading 动画来优化用户等待的心理状态。
+ 涉及到登录、注册、查询按钮的在按钮上增加 Loading 状态，来给用户及时反馈；
+ 个人主观判断「接口返回时间较长」，判断接口返回时间是否大于 200毫秒，适当的增加 Loading 动画。

### 项目脚本
```
npm run dev 开发项目
npm run test 单元测试
npm run build 构建项目
```

### 代码规范
组件名称大写 均使用驼峰命名
文件夹和文件名称使用小写 均使用驼峰命名
变量名称使用小写驼峰
方法名称使用小写驼峰
class类名使用-连接
统一缩进4个空格
减少行内样式
参考雅虎军规；

### 表单验证
表单验证的逻辑，可采用策略模式：验证数字大小，验证长度、大小、数字。

### 持续集成
根据 gitlab 来配置持续集成功能；

备注：
1.前端传什么值给到后端；
2.后端编辑接口返回什么值给前端；
3.详情页接口如何返回；

## 第三方库 和 参考资料
[vue3官网](https://v3.cn.vuejs.org/)
[Husky](https://github.com/typicode/husky)
[pinia](https://github.com/vuejs/pinia)
[wangeditor](https://github.com/wangeditor-team/wangEditor) 
[axios](https://github.com/axios/axios) 
[husky验证](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit)
