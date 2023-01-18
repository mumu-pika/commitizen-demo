# commitizen-demo
Vue 项目 commitizen + husky + commitlint，git commit 提交信息规范校验 demo，[conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) 实践

- `commitizen`：使用 git cz 代替 git commit，引导用户填写规范的 commit 信息
- `husky + commitlint`：git commit 动作时，校验 commit 信息，如果不满足 commitizen 规范，无法提交

## Project Start
```bash
npm install @vue/cli -g # 安装 @vue/cli
vue create comitizen-practice-demo # [Vue 3] typescript, router, vuex, eslint, unit-mocha)
```

## Installing the command line tool
[Commitizen](https://github.com/commitizen/cz-cli)
```bash
npm install -g commitizen # 安装commitizen cli
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
