# NextJS > Yarn Workspacesを使う



Next公式Exampleの以下を参考にします。

https://github.com/vercel/next.js/tree/canary/examples/with-yarn-workspaces



How to useに記載がある以下のコマンドでプロジェクトを作成します。

```
yarn create next-app --example with-yarn-workspaces with-yarn-workspaces-app
```



yarnを実行

```
yarn
```


初期状態ではpackage.jsonの設定がnpm用になっているので、warningが出ます。

```
warning Missing name in workspace at "/with-yarn-workspaces-app-yarn/packages/bar", ignoring.
warning Missing name in workspace at "/with-yarn-workspaces-app-yarn/packages/foo", ignoring.
warning Missing name in workspace at "/with-yarn-workspaces-app-yarn/packages/web-app", ignoring.
```

## package.jsonに設定追加

yarn用に設定を追加していきます。


- packages/web-app/package.json

`name`と`version`を追加します。

```:packages/web-app/package.json
{
  "name": "@project/web-app",
  "version": "1.0.0",
  "private": true,
  "scripts": {
割愛
```


- packages/bar/package.json
- packages/foo/package.json
同様に、`name`と`version`を追加します。

```:packages/bar/package.json
{
  "name": "@project/bar",
  "version": "1.0.0",
  "private": true,
割愛
```



yarnを実行

```
% yarn
yarn install v1.22.17
[1/4] 🔍  Resolving packages...
中略
[4/4] 🔨  Building fresh packages...
success Saved lockfile.
✨  Done in 0.56s.
```

yarnが問題なく完了しました。



## next

#### next.config.js


['bar'] -> ['@project/bar'] に変更します。


```:packages/web-app/next.config.js
const withTM = require('next-transpile-modules')(['@project/bar'])

module.exports = withTM()
```

#### pages

packages/web-app/pages/index.js

```
import foo from '@project/foo'
import Bar from '@project/bar'

export default function Home() {
```




## package.json scriptsの変更

yarnのworkspaceを使うように変更します。
（yarn --cwdのままでも動作はするようです）

```:package.json
"scripts": {
  "dev": "yarn workspace @project/web-app dev",
  "build": "yarn --cwd packages/web-app build",
  "start": "yarn --cwd packages/web-app start"
}
```



# Deploy

Deploy it to the cloud with Vercel (Documentation).

Choose packages/web-app as root directory when deploying.



# 補足

next-transpile-modulesはNextのバージョンと対になるバージョンを選択する必要があります。

https://github.com/martpie/next-transpile-modules#compatibility-table

