# VPM パッケージリストテンプレート

独自のパッケージリストを作成するためのスターターです。

すべてのセットアップが完了したら、[`source.json`](source.json) ファイルを更新し、リストアップされたすべてのパッケージのアップデートを配信するための VPM で動作するリストを生成できるようになります。

## ▶ はじめに

- [![このテンプレートを使う](https://user-images.githubusercontent.com/737888/185467681-e5fdb099-d99f-454b-8d9e-0760e5a6e588.png)](https://github.com/vrchat-community/template-package-listing/generate) を押してください。
  をクリックして、このテンプレートに基づいた新しい GitHub プロジェクトを立ち上げ、その指示に従ってください。
  - 適切なリポジトリ名と説明を選んでください。
  - 可視性を 'Public' に設定します。'Private'を選択し、後で変更することもできます。
  - 「すべてのブランチを含める」を選択する必要はありません。
- ウェブブラウザで GitHub のプロジェクトを編集するか、Git を使ってローカルにリポジトリをクローンしてください。
  - Git や GitHub に詳しくない場合は、[GitHub のドキュメントを参照](https://docs.github.com/en/get-started/quickstart/)。

## オートメーションのセットアップ

このテンプレートでは、[`source.json`](source.json)から始まるいくつかのファイルを編集する必要があります：

- [`name`](source.json#L2)、[`id`](source.json#L3)、[`author`](source.json#L5)、[`description`](source.json#L10)などのリスティングに関する一般的な情報を入力してください。
- 4 行目の[「url」](source.json#L4)フィールドを更新し、「vrchat-community」をあなたの GitHub ユーザー名に、「template-package-listing」をあなたのリポジトリ名に置き換えてください。このリンクは、GitHub で公開されたときにあなたのリストをダウンロードするために使われます。例えば、「thupper-listing」 というリポジトリを作ったユーザー 「thupper」 は、url を 「https://thupper.github.io/thupper-listing/index.json」 に更新します。
- [「infoLink」](source.json#L11) (11 行目) 内の "url" を、新しく作成したリポジトリの url で更新します。
- GitHub でホストされているパッケージを含めたい場合は、[`githubRepos`](source.json#L16)で指定します。
- 他の場所でホストされているパッケージを `.zip` ファイルとしてインクルードしたい場合は、[`packages`](source.json#L19) に指定します。
  - [`githubRepos`](source.json#L16) または [`packages`](source.json#L19) を使用しない場合は、安全に削除することができます。
- 最後に、リポジトリの 「Settings」 ページに行き、「Pages」 を選択し、「Build and deployment」 という見出しを探します。Source」のドロップダウンを「Deploy from a branch」から「GitHub Actions」に変更します。

## 📃 リストの再構築

`main` ブランチに変更を加えるたびに、あるいは手動でトリガーするたびに、'Build Repo Listing' アクションは利用可能なすべてのリリースの新しいインデックスを作成し、GitHub Pages に無料でホストされるウェブサイトとして公開します。このリストは VPM があなたのパッケージを最新の状態に保つために使うことができ、生成されたインデックスページはあなたのパッケージの情報を掲載したシンプルなランディングページとして機能します。あなたのパッケージの URL は `https://username.github.io/repo-name` のような形式になります。

## 🌎 ランディングページのカスタマイズ

ランディングページの外観を変更するために、`Website`ディレクトリの 内容をカスタマイズすることができます。ほとんどの情報は[`source.json`](source.json)から自動的に入力されます。ランディングページを手作業でカスタマイズする必要はありません。

## 技術的なこと

自動化プロセスをあなたのニーズに合うように変更することは歓迎されますし、私たちが採用すべきだと思う変更があれば、Pull Request を作成することもできます。また、採用すべき変更があれば Pull Request を作成することもできます：

### ビルドリスト

[build-listing.yml](.github/workflows/build-listing.yml)これは、あなたが作成した[`source.json`](source.json)ファイルに追加した項目に基づいて、vpm 互換の[Repo Listing](https://vcc.docs.vrchat.com/vpm/repos)をビルドする複合アクションです。
すべてのリリースを検索してリストにまとめるために、[別のリポジトリ](https://github.com/vrchat-community/package-list-action) をチェックアウトします。このリポジトリには VPM コア lib を含む [Nuke](https://nuke.build/) プロジェクトがあり、その型とメソッドにアクセスできるようになっています。このプロジェクトは将来もっと多くの機能を含むように拡張される予定です ―― 今のところ、アクションは `BuildRepoListing` ターゲットを呼び出すだけで、それが完了すると `RebuildHomePage` を呼び出します。もしホームページを再構築するだけのアクションを作りたかったら、代わりにそれを直接呼び出すことができます - 既存の呼び出しをコピーしてターゲット名を置き換えるだけです。
