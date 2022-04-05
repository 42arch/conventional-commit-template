[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)


## commitizen

```sh
yarn add -D commitizen

commitizen init cz-conventional-changelog --yarn --dev --exact
```

在pacakge.json会自动添加：

```json
"config": {
  "commitizen": {
    "path": "./node_modules/cz-conventional-changelog"
  }
}
```

提交时使用命令`git cz` 代替 `git commit`

https://www.conventionalcommits.org/zh-hans/v1.0.0/

主要提交类型有：

```
feat: 新增feature
fix: 修复bug
docs: 仅仅修改了文档，如readme.md
style: 仅仅是对格式进行修改，如逗号、缩进、空格等。不改变代码逻辑。
refactor: 代码重构，没有新增功能或修复bug
perf: 优化相关，如提升性能、用户体验等。
test: 测试用例，包括单元测试、集成测试。
chore: 改变构建流程、或者增加依赖库、工具等。
revert: 版本回滚
```

## husky

运行：
```sh
npx husky-init && yarn
```

会自动在package.json中增加script:

```json
"prepare": "husky install"
```

添加 pre-commit 钩子，在提交前执行一些操作，如lint和test

```sh
npx husky add .husky/pre-commit "yarn lint:fix && yarn test"
```


## lint-staged

安装
```sh
yarn add lint-staged -D
```

在`./husky/pre-commit`文件中修改

```sh
yarn lint-staged && yarn test
```

同时创建 `.lintstagedrc`文件，并添加：

```json
{
  "*.(js|ts)": "eslint --fix"
}
```

## commitlint

使用commitlint在husky上添加约定式提交规范hook，要求commit信息必须满足约定式规范。

```
yarn add @commitlint/cli @commitlint/config-conventional -D

echo module.exports = {extends: ['@commitlint/config-conventional']} > commitlint.config.js

npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
```

## 生成changelog

### 手动生成（不推荐）

``` sh
yarn add conventional-changelog-cli
conventional-changelog -p angular -i CHANGELOG.md -s
```

package.json中添加script:
```
"changelog": "conventional-changelog -p angular -i CHANGELOG.md -s"
```

版本变更 运行`yarn version` 或者 `npm version 0.1.1`变更版本号，再运行 `yarn changelog`生成changelog。

问题：变更版本时会带上提交信息，如果不符合规范会导致husky校验失败，生成的log会有问题。

## standard-version （推荐）

```sh
yarn add standard-version -D
```

package.json 添加script:
```json
"release": "standard-version"
```

运行`yarn release`，自动变更版本号、生成tag和changelog。

也可以手动指定版本号：

```sh
# 0.3.0
npm run release # 0.3.0 to 0.3.1
npm run release -- --prerelease # 0.3.0 to 0.3.0-0
npm run release -- --release-as 0.3.3 # 0.3.0 to 0.3.3
npm run release -- --prerelease alpha # 0.3.0 to 0.3.0-alpha.0

# patch、minor、major
npm run release -- --release-as minor  # 0.4.4-alpha.0 to 0.5.0
npm run release -- --release-as patch # 0.5.0 to 0.5.1
npm run release -- --release-as major # 0.5.1 to 1.0.0
npm run release -- --release-as prepatch # 1.0.0 to 1.0.1-0
npm run release -- --release-as preminor # 1.0.1-0 to 1.1.0-0
npm run release -- --release-as premajor # 1.1.0-0 to 2.0.0-0

```

## git-flow的分支策略

参考：
https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy

