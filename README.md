# commitizen-demo
> 记录commitizen 代码规范的使用

Vue 项目 commitizen + husky + commitlint，git commit 提交信息规范校验 demo，[conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) 实践。

- `commitizen`：使用 git cz 代替 git commit，引导用户填写规范的 commit 信息
- `husky + commitlint`：git commit 动作时，校验 commit 信息，如果不满足 commitizen 规范，无法提交

## 1、Initialize project 初始化项目 (这里以vue3项目为例)

```bash
npm install @vue/cli -g # 安装 @vue/cli
vue create comitizen-practice-demo # [Vue 3] typescript, router, vuex, eslint, unit-mocha)
```

## 2、Installing commitizen 使用 commitizen

[commitizen](https://github.com/commitizen/cz-cli)是一个 cli 工具，用于规范化 git commit 信息，可以代替 git commit。

### installing the command line tool

```bash
npm install -g commitizen # 全局安装commitizen cli
```

### making your repo commitizen friendly

After installing the Commitizen CLI tools, it is necessary to initialize your project to use the cz-conventional-changelog adapter by typing.

```bash
commitizen init cz-conventional-changelog --save-dev --save-exact
```

Or if you are using Yarn:

```bash
commitizen init cz-conventional-changelog --yarn --dev --exact
```

Or if you are using Pnpm:
```bash
commitizen init cz-conventional-changelog --pnpm --save-dev --save-exact
```

## 3、Installing husky + commitlint

[commitlint](https://github.com/conventional-changelog/commitlint) 结合 husky 可以在 git commit 时校验 commit 信息是否符合规范。

### husk 安装
husky 可以帮助我们在 执行 git commit 提交的时候，按照eslint 规范进行修复代码。
husky是一个git hook工具，可以帮助我们触发git提交的各个阶段：pre-commit、commit-msg、pre-push 支持所有的Git 钩子。
#### 1. 安装 husky

More see: [husky install](https://typicode.github.io/husky/#/?id=install)
```bash
# Install Husky v6
npm install husky --save-dev
# or
yarn add husky --dev
# or
pnpm add husky --save-dev
```

#### 2. 激活 husky git hooks
> 注意！如果这里激活报错husky -.git 尝试打开项目根目录, 而不要在其他目录使用下面的命令

```bash
# Activate hooks
npx husky install
# or
yarn husky install
# or
pnpm husky install
# husky - Git hooks installed
```

#### 3. （这步选做）测试用 husky 钩子作用，添加 pre-commit 钩子

```bash
npx husky add .husky/pre-commit "npm test"
# 查看当前目录 .husky 目录是否有生成 pre-commit 文件
# 如果需要删除这个钩子，直接 删除 .husky/pre-commit 文件即可
```

### commitlint 安装
按照cz来规范了提交风格，但是依然可以通过 git commit 来提交不规范的格式,这个时候就需要通过commitlint来阻止不规范提交了。
More See: [commitlint](https://commitlint.js.org/)
#### 安装commitlint
Install `commitlint` and a `commitlint-config-*` of your choice as devDependency and configure `commitlint` to use it.
``` bash
# Install and configure if needed
npm install --save-dev @commitlint/{cli,config-conventional}
# For Windows:
npm install --save-dev @commitlint/config-conventional @commitlint/cli

yarn add --save-dev @commitlint/config-conventional @commitlint/cli

# Configure commitlint to use conventional config
echo "module.exports = { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

#### 添加husky钩子
为的是husky调用钩子函数时候会去检查commitlint的配置
``` bash
npx husky add .husky/commit-msg  "npx --no -- commitlint --edit ${1}"
```

#### 配置commitlint
``` bash
# Add the path to the configuration file
commitlint --config commitlint.config.js
```


## 4、根据commit信息生成changelog
### standard version (自动生成)
More See: [conventional-changelog](https://github.com/conventional-changelog/standard-version)
相比手动安装, standard-version只需要`npm run release`, 就可以有`npm run version`的功能，而且提交信息是标准的commitizen 规范, 而且自动生成changelog 自动打tag, 自动commit, 我们只需要push即可。
#### install dependencies 安装
```bash
npm i --save-dev standard-version
```
#### package.json中的scripts设置
``` json
{
  "scripts": {
    "release": "standard-version"
  }
}
```

#### release version 发布版本
##### First Release

To generate your changelog for your first release, simply do:

```sh
# npm run script
npm run release -- --first-release
# global bin
standard-version --first-release
# npx
npx standard-version --first-release
```

This will tag a release **without bumping the version `bumpFiles`[1]()**.

When you are ready, push the git tag and `npm publish` your first release. \o/

##### Cutting Releases

If you typically use `npm version` to cut a new release, do this instead:

```sh
# npm run script
npm run release
# or global bin
standard-version
```

As long as your git commit messages are conventional and accurate, you no longer need to specify the semver type - and you get CHANGELOG generation for free! \o/

After you cut a release, you can push the new git tag and `npm publish` (or `npm publish --tag next`) when you're ready.

##### Release as a Pre-Release

Use the flag `--prerelease` to generate pre-releases:

Suppose the last version of your code is `1.0.0`, and your code to be committed has patched changes. Run:

```bash
# npm run script
npm run release -- --prerelease
```
This will tag your version as: `1.0.1-0`.

If you want to name the pre-release, you specify the name via `--prerelease <name>`.

For example, suppose your pre-release should contain the `alpha` prefix:

```bash
# npm run script
npm run release -- --prerelease alpha
```

This will tag the version as: `1.0.1-alpha.0`


### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
