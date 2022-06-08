# 前端自动化测试框架jest

####优点

- 速度快
- api简单
- 易配置
- 隔离性好
- 监控模式
- IDE整合
- Snapshot
- 多项目并行
- 覆盖率
- Mock丰富

####功能（要测试的方法必须模块化的形式才可以引入测试用例）

1. 单元测试
2. 集成测试

####安装使用

```
// 安装
npm install jest vue-jest babel-jest @vue/test-utils -D
// 支持es import
npm install @babel/core@7.4.5 @babel/preset-env@7.4.5 -D
// 初始化jest配置
npx jest --init

// 输出整个代码测试覆盖率（图表形式）
npx jest --coverage


```



