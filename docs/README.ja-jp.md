# GitHub Profile 3D Contrib.

![svg](https://raw.githubusercontent.com/yoshi389111/yoshi389111/main/profile-3d-contrib/profile-green-animate.svg)

## 概要

この GitHub Action は GitHub のコントリビュートカレンダーの 3D 版を SVG で作成します。

## 使い方 (GitHub Actions)

このアクションは、GitHub プロファイルの3D 版のコントリビュートカレンダー（いわゆる芝生）の 3D 版の SVG を GitHub プロフィール用に作成し、リポジトリにコミットします。

このアクションを追加した後、自分でアクションをトリガーすることもできます。

### 手順 1. スペシャルなリポジトリを作る

ユーザー名と同じ名前で GitHub にリポジトリを作成してください。

* 例：ユーザー名が `octocat`の場合は、`octocat/octocat` という名前のリポジトリを作成します。

このリポジトリで、以降の手順を実行します。

### 手順 2. ワークフローファイルを作る

以下のようなワークフローファイルを作成します。

* `.github/workflows/profile-3d.yml`

スケジュールは1日1回開始するように設定されています。
起動時間を都合の良い時間に修正してください。

```yaml:.github/workflows/profile-3d.yml
name: GitHub-Profile-3D-Contrib

on:
  schedule: # 03:00 JST == 18:00 UTC
    - cron: "0 18 * * *"
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    name: generate-github-profile-3d-contrib
    steps:
      - uses: actions/checkout@v2
      - uses: yoshi389111/github-profile-3d-contrib@0.1.0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          USERNAME: ${{ github.repository_owner }}
      - name: Commit & Push
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add -A .
          git commit -m "generated"
          git push
```

注：プライベートリポジトリも集計対象とする場合は、「personal access token」をリポジトリに登録し、ワークフローファイルで指定されている GITHUB_TOKEN に設定します。

これにより、アクションがリポジトリに追加されます。

### 手順 3. アクションを手動起動する

追加したアクションを起動してください。

* `Actions` -> `GitHub-Profile-3D-Contrib` -> `Run workflow`

プロフィール画像は以下のパスで生成されます。

* `profile-3d-contrib/profile-green-animate.svg`
* `profile-3d-contrib/profile-green.svg`
* `profile-3d-contrib/profile-season-animate.svg`
* `profile-3d-contrib/profile-season.svg`

### 手順 4. README.md を追加

生成された画像のパスを readme ファイルに追加します。

例：

```md
![](./profile-3d-contrib/profile-green-animate.svg)
```

## Licence

MIT License

Copyright (C) 2021 SATO Yoshiyuki