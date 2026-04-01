# Antigravity + Claude Code 自立型エージェント セットアップガイド

このドキュメントは、Mac環境で「Antigravity」と「Claude Code」を連携させ、**自分だけの仮想カンパニー（AI組織）を構築する**ためのセットアップ手順です。

最終ゴールは、Claude Code上で秘書・マーケティング・開発・営業など**仮想の組織を作り、AIに業務を任せられる環境**を完成させることです。

参考動画: https://youtu.be/cfoE_8Llde0
参考リポジトリ: https://github.com/Shin-sibainu/cc-company

---

## 全体の流れ

```
Step 1: 開発環境の準備（Homebrew → Node.js）
Step 2: Claude Codeのインストール・ログイン
Step 3: Antigravityのセットアップ・日本語化
Step 4: Claude Code拡張機能のインストール
Step 5: AIの振る舞い設定（CLAUDE.md）
Step 6: 仮想カンパニーの構築（cc-company プラグイン）
Step 7: 運用開始
```

---

## 1. 前提条件と必要なツール

以下のツールが必要です：

| ツール | 役割 |
|--------|------|
| Homebrew | Macの開発ツール管理（アプリストアのコマンド版） |
| Node.js / npm | Claude Codeの実行環境 |
| Claude Code | Anthropic提供のCLIベースAIエージェント |
| Antigravity | AIエージェント統合型の次世代IDE（エディタ） |
| cc-company | Claude Code用の仮想組織構築プラグイン |

**イメージ:**
```
Homebrew → Node.jsをインストール
Node.js  → npmが付いてくる
npm      → Claude Codeをインストール
Claude Code → cc-companyプラグインで仮想カンパニーを構築
```

---

## 2. インストール手順

### 2.1 Homebrewのインストール

ターミナルで以下を実行：

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

インストール完了後、**パスを通す**（これを忘れると `command not found` になる）：

```
echo >> ~/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv zsh)"
```

確認：
```
brew --version
```
→ バージョンが表示されればOK

### 2.2 Node.jsのインストール

```
brew install node
```

確認：
```
node -v
npm -v
```
→ 両方バージョンが表示されればOK（Node.js v18以上が必要）

### 2.3 Claude Codeのインストール

```
npm install -g @anthropic-ai/claude-code
```

確認：
```
claude --version
```

### 2.4 Claude Codeの初期設定とログイン

プロジェクトフォルダを作成して移動：
```
mkdir ~/Desktop/MyCompany
cd ~/Desktop/MyCompany
```

Claude Codeを起動：
```
claude
```

起動後の手順：
1. Enterキーを押してログイン方法を選択（APIキー認証 または Claude.ai認証）
2. ターミナル設定で **「1. Yes, use recommended settings」** を選択
3. フォルダ信頼確認で **「1. Yes, I trust this folder」** を選択してEnter

### 2.5 プロジェクトの初期化

Claude Codeのプロンプト（>）で以下を実行：
```
/init
```

これによりAIがプロジェクト構造を読み込み、指示書となる `CLAUDE.md` が自動生成されます。

---

## 3. Antigravityとの連携

### 3.1 Antigravityのセットアップ

1. Antigravityアプリを起動
2. 初期セットアップで **「Agent-driven development」** を選択してNext
3. エディタ設定はデフォルトのまま進める

### 3.2 日本語化（オプション）

1. `Cmd + Shift + P` でコマンドパレットを開く
2. `display` と入力し **「Configure Display Language」** を選択
3. **「日本語 (ja)」** を選択
4. 日本語パックがない場合は自動インストール後、再起動

### 3.3 Claude Code拡張機能のインストール

1. Antigravityで対象プロジェクトフォルダを開く
2. 左サイドバーの **「拡張機能」** アイコンをクリック
3. 検索窓に **「Claude Code」** と入力
4. **Anthropic公式拡張機能** をインストール
5. **「Spark（✦）」アイコン** をクリック → Claude Codeチャット画面が表示される

---

## 4. AIの振る舞い設定（CLAUDE.mdのカスタマイズ）

プロジェクトルートの `CLAUDE.md` を開き、末尾に以下を追記します。
**これはAIの「性格」と「行動ルール」を定義する最も重要な設定です。**

```markdown
## Agent Behavior & Communication Rules

- **Language:** ALWAYS communicate with the user in **Japanese (日本語)**. 
  All responses, explanations, and questions must be in Japanese.

- **User Profile:** The user's name is **[あなたの名前]**, and works as a 
  **[あなたの職種]**.

- **Communication Style:** Act as a frank, essential advisor. Do not just 
  agree or flatter. Thoroughly verify ideas, question assumptions, and point 
  out blind spots. If logic is weak, explain why clearly. Provide concrete, 
  prioritized suggestions for the next steps. Be honest and direct.
```

💡 **ポイント:** `[あなたの名前]` と `[あなたの職種]` を自分の情報に書き換えてください。

---

## 5. 仮想カンパニーの構築（cc-company プラグイン）

ここが本ガイドのゴールです。Claude Code上に**自分だけの仮想組織**を作ります。

### 5.1 cc-companyとは？

Claude Code用のプラグインで、AIが「秘書」として窓口になり、TODO管理・壁打ち・メモなどを担当します。仕事が増えてきたら、マーケティング部・開発部・営業部など**部署が自然に追加されていく**仕組みです。

```
あなた → 秘書（窓口） → 各部署（必要に応じて追加）
```

用意されている部署テンプレート：
- 秘書室（常設）
- PM（プロジェクト管理）
- リサーチ
- マーケティング
- 開発
- 経理
- 営業
- クリエイティブ
- 人事

### 5.2 GitHubのSSH設定（初回のみ）

cc-companyプラグインのインストールにはGitHub接続が必要です。

**SSHキーを作成：**
```
ssh-keygen -t ed25519 -C "your-email@example.com"
```
→ 全部Enterを押せばOK

**SSHキーをコピー：**
```
pbcopy < ~/.ssh/id_ed25519.pub
```

**GitHubに登録：**
1. GitHub（https://github.com）にログイン
2. 右上のアイコン → **Settings**
3. 左メニュー → **SSH and GPG keys**
4. **New SSH key** をクリック
5. Title: 任意（例: `My Mac`）
6. Key: `Cmd + V` で貼り付け
7. **Add SSH key** をクリック

**接続確認：**
```
ssh -T git@github.com
```
→ `Hi ○○! You've successfully authenticated` と出ればOK

### 5.3 cc-companyプラグインのインストール

Antigravity内のClaude Codeチャット欄、またはターミナルのClaude Codeで以下を実行：

```
/plugin marketplace add Shin-sibainu/cc-company
/plugin install company@cc-company
```

### 5.4 仮想カンパニーの初期セットアップ

```
/company
```

秘書との対話が始まります：

```
秘書: はじめまして！あなたの秘書になります。
      まず、あなたの事業や活動を教えてください。

あなた: （例）マーケティング担当です。業務効率化とデータ分析をやっています

秘書: ありがとうございます！
      今の目標や、日々困っていることがあれば教えてください。

あなた: （例）タスクが散らかるのが悩み。施策の優先順位をつけたい
```

セットアップ完了後、以下のフォルダが自動生成されます：

```
.company/
├── CLAUDE.md              ← 組織ルール（AIが参照する設定ファイル）
└── secretary/
    ├── CLAUDE.md           ← 秘書の振る舞い定義
    ├── inbox/              ← クイックメモ・とりあえずここに
    ├── todos/              ← 日次タスク管理
    │   └── 2026-04-01.md   ← 今日のTODO
    └── notes/              ← 壁打ち・相談メモ・意思決定ログ
```

### 5.5 部署の追加

最初は秘書室だけでスタート。仕事が増えたら、秘書が部署の追加を提案します。

**自動提案の場合：**
```
秘書: リサーチの依頼が増えていますね。
      リサーチ部門を作りましょうか？

あなた: 作って
→ .company/research/ が自動生成される
```

**手動で追加する場合：**
```
> マーケティング部を作って
→ .company/marketing/ が自動生成される
```

---

## 6. 運用方法と使い分け

### 6.1 日常の使い方

セットアップ完了後は、 `/company` で秘書に話しかけるだけです：

| やりたいこと | 話しかけ方（例） |
|-------------|-----------------|
| タスク追加 | 「今日のTODOに○○を追加して」 |
| 壁打ち・相談 | 「この施策について壁打ちしたい」 |
| メモを残す | 「さっきの会議で○○が決まった」 |
| 進捗確認 | 「ダッシュボード見せて」 |
| リサーチ依頼 | 「○○について調べて」 |
| ファイル編集 | 「○○のスクリプトを修正して」 |

### 6.2 ツール使い分けガイドライン

| ツール | 用途 |
|--------|------|
| Claude デスクトップアプリ / Web版 | アイデア出し、文章作成、戦略の相談など（実行環境を伴わない思考整理） |
| Antigravity内のClaude Code | ファイル編集、スクリプト実行、WordPress投稿など（自立型エージェントとして実作業を任せる） |
| `/company`（仮想カンパニー） | TODO管理、壁打ち、部署横断の業務管理（日々の業務の司令塔） |

### 6.3 ダッシュボード

「ダッシュボード」と話しかけると、組織の状況を一覧表示：

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Company ダッシュボード
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

秘書室:
  TODO（今日）: 3件 未完了 / 2件 完了
  Inbox: 1件 未整理

マーケティング:
  コンテンツ企画: 2件 進行中

何かありますか？
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 7. トラブルシューティング

### `command not found: brew`
→ パスが通っていない。2.1の「パスを通す」コマンドを再実行

### `command not found: node`
→ Homebrewのパスを通した後、`brew install node` を再実行

### プラグインインストール時に `Host key verification failed`
→ 5.2のSSH設定を実施してから再実行

### `/company` が反応しない
→ プラグインが正しくインストールされているか確認：
```
/plugin list
```

---

## まとめ

```
Homebrew → Node.js → Claude Code → Antigravity連携 → cc-company → 仮想カンパニー完成！
```

最初は秘書だけの小さな組織からスタートし、使いながら自然に組織が育っていきます。
「/company」で秘書に話しかけることから、すべてが始まります。

---

参考資料：
- cc-company リポジトリ: https://github.com/Shin-sibainu/cc-company
- 解説動画: https://youtu.be/cfoE_8Llde0
- Antigravity 公式サイト
- Claude Code ドキュメント
