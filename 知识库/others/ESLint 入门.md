# ESLint 入门

## 安装开发环境下的依赖：

``` bash
npm install eslint --save-dev
```

## 项目文件夹下初始化：

``` bash
npx eslint --init
```

注意：初始化过程中有选择配置的过程，根据喜好选择

## 配置：

### rules: 

定义需要符合的规则

### parser: 

指定一个不同的解析器，解析器的作用是将 JS 代码解析成  AST ，ESLint 将通过遍历该 AST  来触发各个检查规则

### plugins: 

官方提供的规则毕竟有限，当我们想自定义规则的时候，就需要**自定义**一个  ESLint  的插件，然后将规则写到自定义的  ESLint`插件中，在配置文件中通过`plugins 字段引入

### extends: 

手动配置的工作量很大，所以一般会使用 extends 扩展包来预设配置

## 原理：

此处待以后添加

## 搭配使用：

### VSCODE:

通常我们在开发中，不会频繁使用`npx eslint`命令执行代码检查，而是在 IDE 中自动提醒 Eslint 的错误。在 VSCode 中，需要安装 ESLint 插件。

### prettier:

prettier 是一个代码格式化工具，专注于代码格式自动调整，prettier 比 eslint 更擅长格式效验。

两者功能有所交叉，一起使用有所冲突，下面进行一些配置：

1. 用 eslint-config-prettier 来关掉 (disable) 所有和 Prettier 冲突的 ESLint 的配置，方法就是在 .eslintrc 里面将 prettier 设为最后一个 extends，需要安装 eslint-config-prettier

```json
// .eslintrc    

{      

    "extends": ["prettier"] // prettier 一定要是最后一个，才能确保覆盖    

}
```

2. （可选）然后再安装 eslint-plugin-prettier 和 prettier，将 prettier 的 rules 以插件的形式加入到 ESLint 里面。

```json
// .eslintrc    

{      

    "plugins": ["prettier"],      

    "rules": {        

        "prettier/prettier": "error"      

    }    

}
```

将上面两个步骤合在一起就是下面的配置，也是**官方的推荐配置**

```json
// .eslintrc

{

  "extends": ["plugin:prettier/recommended"]

}
```

### husky:

现在我们已经能做到了在开发时检测出来错误并且方便及时修复问题，但这依赖于开发同学自觉，不通过eslint代码检测的代码依然能被提交到仓库中去。此时我们需要借助**husky**[19]来拦截 git 操作，在 git 操作之前再进行一次代码检测。

新版的 husky 使用有些变化，不再是直接在 package.json 中进行配置。

```json
// package.json

{  

  "husky": {

    "hooks": {

      "pre-commit": "eslint src/** --fix"

    }

  }

}
```

***新版用法***：

```bash
npm install -D husky

# husky 初始化，创建.husky/目录并指定该目录为git hooks所在的目录

husky install 

# .husky/目录下会新增pre-commit的shell脚本

# 在进行 git commit 之前运行 npx eslint src/** 检查

npx husky add .husky/pre-commit "npx eslint src/**"
```

关于 husky install 官网推荐的是在 packgae.json 中添加 prepare 脚本，prepare 脚本会在 npm install（不带参数）之后自动执行

```json
{

  "scripts": {

    "prepare": "husky install"

  }

}
```

### lint-staged:

对于单次提交而言，如果每次都检查 src 下的所有文件，可能不是必要的，特别是对于有历史包袱的老项目而言，可能无法一次性修复不符合规则的写法。所以我们需要使用**lint-staged**[20]工具只针对当前修改的部分进行检测。

```json
// package.json

{

  "lint-staged": {

    "*.{js,ts}": [

      "npx eslint --fix"

    ]

  },

}
```

配置表示的是，对当前改动的 .js 和 .ts文件在提交时进行检测和自动修复，自动修复完成后 lint-staged默认会把改动的文件再次 add 到暂存区，如果有无法修复的错误会报错提示。

同时还需要改动一下之前的 husky 配置，修改 .husky/pre-commit，在 commit 之前运行`npx lint-staged`来校验提交到暂存区中的文件：

```bash
#!/bin/sh

. "$(dirname "$0")/_/husky.sh"



npx lint-staged
```

