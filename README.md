# Repository_Co-Construction_Note
## 代码仓库使用规范：
- 分支问题
  - main 为主分支，为确保主分支稳定性， main 分支一般由 develop 分支合并，任何时间都不能直接修改代码。
  - develop 为开发分支，始终保持最新完成以及bug修复后的代码，一般开发的新功能时，feature分支都是基于develop分支下创建的。平时我们提交代码都在 dev 分支。
  - feature 分支是开发新功能时，以develop为基础创建的特性分支。
- 提交规范问题
  - 为了便于追溯，我们提交规范格式为```type(scope): body```参考以下提交规范：
  - type：用于指定commit的类型，约定了feat、fix两个主要type，以及docs、style、build、refactor、revert五个特殊type。
  ```
    # 主要type
    feat:     增加新功能
    fix:      修复bug
    
    # 特殊type
    docs:     只改动了文档相关的内容
    style:    不影响代码含义的改动，例如去掉空格、改变缩进、增删分号
    build:    构造工具的或者外部依赖的改动，例如webpack，npm
    refactor: 代码重构时使用
    revert:   执行git revert打印的message
    
    # 其他type
    test:     添加测试或者修改现有测试
    perf:     提高性能的改动
    ci:       与CI（持续集成服务）有关的改动
    chore:    不修改src或者test的其余修改，例如构建过程或辅助工具的变动
    
    当一次改动包括主要type与特殊type时，统一采用主要type
  ```
    - scope：用于描述改动的范围，格式为项目名/模块名，如果一次commit修改多个模块，建议拆分成多次commit，以便更好追踪和维护。
    - body：填写详细描述，主要描述改动之前的情况及修改动机，对于小的修改不作要求，但是重大需求、更新等必须添加body来作说明。
- 示例：
  ```
    feat(tasks/chassis): add supercapacitor control module
  ```
  ```
    docs(README): add explanation
  ```
  ```
    docs(Xmind): Fixed a bug with the chassis branch
  ```
## 代码编写规范
- 见飞书-->软件组知识库
