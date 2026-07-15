# Twitter Skill

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md)

Codex、Claude Code、その他の AI Agent から、自分のローカル X/Twitter セッションを利用して X を操作できます。

Twitter Skill は、Windows/macOS デスクトップアプリ、JSON CLI、標準 Agent Skill で構成されています。Agent は X の検索、複数アカウントの切り替え、画像アップロード、投稿、返信、投稿削除を実行できます。X Cookie が Agent に渡ることはありません。

## 機能

| 機能 | 内容 |
| --- | --- |
| 検索 | X の検索演算子、取得件数、結果モード、カーソルページングに対応 |
| アカウント | 接続済みアカウントの一覧、デフォルトまたは指定アカウントの利用 |
| 投稿 | テキスト投稿、返信、最大 4 枚の画像付き投稿 |
| メディア | JPEG、PNG、WebP 画像のアップロード |
| 管理 | 投稿の削除とローカルアカウント状態の確認 |
| Agent 連携 | Codex、Claude Code、Cursor、および `skills` CLI 対応 Agent に同じ標準 Skill をインストール |

## 必要なもの

1. [GitHub Releases](https://github.com/chadyi/twitter-skill-releases/releases/latest) から最新の Twitter Skill デスクトップアプリをインストールします。
2. アプリを開き、1 つ以上の X アカウントを接続して、システムトレイで実行したままにします。
3. `npx` を利用できるように [Node.js](https://nodejs.org/) をインストールします。

## Agent Skill のインストール

次のどちらかを選択してください。どちらも同じ Skill をグローバルにインストールします。

### 自分で実行する

ターミナルで次のコマンドを実行します。

```bash
npx skills add chadyi/twitter-skill-releases -s twitter-skill -g -y
```

### Agent にインストールを依頼する

次のメッセージを Codex、Claude Code、または対応 Agent にコピーしてください。

```text
https://github.com/chadyi/twitter-skill-releases から Twitter Skill をグローバルにインストールしてください。次のコマンドを実行します。
npx skills add chadyi/twitter-skill-releases -s twitter-skill -g -y

インストール後、twitter-skill がインストール済みであることを確認し、SKILL.md を読み、ローカルの Twitter Skill デスクトップアプリが利用可能か確認してください。X Cookie を要求、表示、保存しないでください。
```

手動で確認する場合：

```bash
npx skills list -g
```

後で更新する場合：

```bash
npx skills update twitter-skill -g
```

## 使い方

インストール後、Agent に自然な言葉で依頼します。

```text
X で GPT Image 2 に関する最新の投稿を検索し、20 件返してください。
```

```text
デフォルトの X アカウントで、この画像を添付して「hello」と投稿してください。
```

```text
この X 投稿に短い要約で返信してください。
```

Agent は Skill を読み、ローカル CLI を呼び出します。CLI は構造化 JSON で結果とエラーを返します。

## 動作の仕組み

```text
Codex / Claude Code / その他の Agent
  -> インストール済み twitter-skill/SKILL.md
  -> ローカル Twitter Skill CLI
  -> 実行中のデスクトップトレイプロセス
  -> サーバーからの短期リクエストプラン
  -> ユーザーのローカル Electron セッションとネットワークから X にリクエスト
```

- X のログイン状態は、デスクトップアプリ内の分離されたローカルアカウント領域に保存されます。
- X へのリクエストは共有サーバー IP ではなく、ユーザーの端末から送信されます。
- Skill には CLI の利用手順だけが含まれ、Cookie や非公開の X リクエスト実装は含まれません。
- 公開リポジトリに含まれるのは Agent Skill とリリースファイルだけです。アプリとサーバーのソースコードは非公開です。

Twitter Skill はログイン済みの X Web セッションを利用し、X 公式 API は使用しません。

## デスクトップアプリのダウンロードと更新

最新版は [Releases](https://github.com/chadyi/twitter-skill-releases/releases/latest) からダウンロードできます。

| プラットフォーム | パッケージ |
| --- | --- |
| Windows x64 | `Twitter-Skill-<version>-x64.exe` |
| macOS Universal | `Twitter-Skill-<version>-universal.dmg` |

各 Release には自動更新メタデータ、blockmap、macOS 更新用 ZIP、`SHA256SUMS.txt` も含まれます。デスクトップアプリはこの公開リポジトリで更新を確認します。

## トラブルシューティング

### macOS で「アプリケーションが壊れているため開けません」と表示される

まず、同じ Release の `SHA256SUMS.txt` とダウンロードしたファイルを照合してください。アプリを `/Applications` に移動し、右クリックして **開く** を選択します。新しい macOS では、**システム設定 > プライバシーとセキュリティ** から **このまま開く** を選択することもできます。

検証済みのアプリが引き続き壊れていると表示される場合は、このアプリだけの隔離属性を削除します。

```bash
sudo xattr -dr com.apple.quarantine "/Applications/Twitter Skill.app"
```

コマンド完了後にもう一度アプリを開いてください。Gatekeeper をシステム全体で無効にしないでください。画像付きの説明と詳細は、[macOS のトラブルシューティングまとめ](https://sysin.org/blog/macos-if-crashes-when-opening/)を参照してください。

### Agent が `twitter-skill` を検出できない

`npx skills list -g` を実行し、Skill を再インストールしてから Agent を再起動してください。

### ローカル CLI を利用できない

Twitter Skill デスクトップアプリを一度起動し、トレイで実行したままにしてください。ローカル CLI ランチャーはアプリ起動時に作成されます。

### ローカル X ログインの有効期限が切れた

デスクトップアプリを開き、対象アカウントを再接続してください。X Cookie を Agent やターミナルに貼り付けないでください。

### 投稿がタイムアウトした

再試行する前に、X 上のアカウントと投稿を確認してください。タイムアウトしていても投稿が完了している場合があります。

## リポジトリの内容

```text
skills/twitter-skill/SKILL.md  標準 Agent Skill
GitHub Releases                Windows/macOS インストーラーと更新メタデータ
```

アプリまたはリリースに関する問題は [GitHub Issues](https://github.com/chadyi/twitter-skill-releases/issues) で報告できます。
