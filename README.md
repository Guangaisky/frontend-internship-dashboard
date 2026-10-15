# 前端实习任务看板

一个使用 Vue 3 构建的本地任务管理应用，帮助求职者记录前端学习和实习投递任务。

## 功能

- 新增、完成和删除任务
- 查看全部、未完成或已完成任务
- 自动统计任务完成进度
- 使用 localStorage 保存任务，刷新页面后数据仍保留
- 使用 Vue 组件拆分任务列表，父子组件通过 props 与 emits 通信

## 技术栈

- Vue 3
- Vite
- JavaScript
- CSS
- localStorage

## 本地运行

```bash
npm install
npm run dev
```

在浏览器打开终端显示的本地地址。

## 项目结构

```text
src/
├── App.vue                 # 数据、筛选和页面主结构
└── components/
    └── TaskList.vue        # 任务列表展示与交互事件
```

## 后续计划

- 支持任务优先级和截止日期
- 适配更小的手机屏幕
- 部署在线演示地址