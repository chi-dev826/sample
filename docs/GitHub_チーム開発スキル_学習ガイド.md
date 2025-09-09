---
tags: ["GitHub", "チーム開発", "ハッカソン", "学習ガイド", "初心者向け"]
---

# GitHubチーム開発スキル 学習ガイド

> **対象**: Git初心者・ハッカソン参加者  
> **目的**: チーム開発で必要なGitHub操作を実践的に習得する

---

## 目次

### 準備編
- [1. 事前準備](#1-事前準備)
- [2. 基本用語の理解](#2-基本用語の理解)

### 必須スキル（ハッカソンで絶対必要）
- [3. リポジトリの基本操作](#3-リポジトリの基本操作)
- [4. 変更の保存と共有](#4-変更の保存と共有)
- [5. ブランチでの作業](#5-ブランチでの作業)
- [6. プルリクエスト](#6-プルリクエスト)

### 重要スキル（チーム開発でよく使う）
- [7. コンフリクトの解決](#7-コンフリクトの解決)
- [8. 作業の一時保存](#8-作業の一時保存)
- [9. 変更の取り消しと履歴確認](#9-変更の取り消しと履歴確認)

### オプション（あると便利）
- [10. フォークとテンプレート活用](#10-フォークとテンプレート活用)
- [11. Issue管理](#11-issue管理)

### トラブルシューティング
- [12. よくあるエラーと対処法](#12-よくあるエラーと対処法)

---

## よくある質問

### Q: どのコマンドから始めればいい？
A: まず「3. リポジトリの基本操作」から始めて、順番に進めてください。ハッカソンでは「必須スキル」セクション（3-6章）をマスターすれば十分です。

### Q: エラーが出た時はどうすればいい？
A: 「12. よくあるエラーと対処法」を確認してください。それでも解決しない場合は、エラーメッセージをコピーしてチームメンバーに相談しましょう。

### Q: ハッカソンで最低限必要なスキルは？
A: 「必須スキル」セクション（3-6章）をマスターすれば十分です。時間があれば「重要スキル」も学習してください。

### Q: コマンドを覚える必要はある？
A: 基本的なコマンドは覚えた方が効率的ですが、このガイドを見ながらでも作業できます。慣れるまではガイドを横に置いて作業しましょう。

### Q: もしチームメンバーと同時に同じファイルを編集してしまったら？
A: コンフリクトが発生します。「7. コンフリクトの解決」を参照してください。慌てずに一つずつ解決していきましょう。

### Q: なぜメインブランチで直接作業してはいけないの？
A: メインブランチは常に安定した状態を保つ必要があります。ブランチを使うことで、新機能の開発中でもメインブランチは安全に保たれ、チーム全体の作業が止まりません。

---

## 1. 事前準備

### チェックリスト
- [ ] GitHubアカウントを作成済み
- [ ] Gitをインストール済み
- [ ] エディタ（VSCode等）をインストール済み

### Gitの導入方法（初心者向け）

**Gitをまだインストールしていない場合**:
- [Git公式サイト](https://git-scm.com/) - 公式インストーラー
- [Progate Git入門](https://prog-8.com/docs/git-env) - 環境構築から丁寧に解説
- [ドットインストール Git入門](https://dotinstall.com/lessons/basic_git) - 動画で分かりやすく
- [サルでもわかるGit入門](https://backlog.com/ja/git-tutorial/) - 導入から基本操作まで

**GitHubアカウント作成**:
- [GitHub公式サイト](https://github.com/) - 無料アカウント作成

### 初期設定
```bash
# ユーザー情報の設定（初回のみ）
git config --global user.name "あなたの名前"
git config --global user.email "your-email@example.com"
```

---

## 2. 基本用語の理解

| 用語 | 説明 | 例 |
|------|------|-----|
| **リポジトリ** | プロジェクトのコードを保存する場所 | プロジェクトの「箱」 |
| **クローン** | リモートリポジトリをローカルにコピー | `git clone URL` |
| **ステージング** | コミットする変更を選ぶ作業（準備段階） | 「保存するファイルを選ぶ」 |
| **コミット** | 変更を記録する | 「保存」のようなもの |
| **プッシュ** | ローカルの変更をリモートに送信 | `git push` |
| **プル** | リモートの変更をローカルに取得 | `git pull` |
| **ブランチ** | 独立した開発ライン | メインから分岐した作業スペース |

---

## 3. リポジトリの基本操作

### 必須: リポジトリのクローン

**目的**: チームのプロジェクトに参加する

```bash
# 1. リポジトリのURLをコピー（GitHubの緑色の「Code」ボタンから）
# 2. ローカルにクローン
git clone https://github.com/username/project-name.git

# 3. プロジェクトフォルダに移動
cd project-name
```

**ポイント**: クローンは**1回だけ**行う操作です

### 必須: リモートリポジトリの確認

```bash
# どのリモートリポジトリと接続されているか確認
git remote -v
```

**期待される結果**:
```bash
$ git remote -v
origin  https://github.com/username/project-name.git (fetch)
origin  https://github.com/username/project-name.git (push)
```

** ポイント**: 
- `origin`はリモートリポジトリの名前（通常は変更不要）
- `fetch`は取得用、`push`は送信用のURL
- 両方とも同じURLが表示されるのが正常

---

## 4. 変更の保存と共有

### 必須: 変更の確認

```bash
# 現在の変更状況を確認
git status

# 変更内容の詳細を確認
git diff
```

**よく見る状態**:
- `Untracked files`: 新しく作成したファイル
- `Changes not staged`: 既存ファイルの変更
- `Changes to be committed`: コミット準備完了

**実際の出力例**:
```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   src/App.js

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new-file.txt

no changes added to commit (use "git add" or "git commit -a")
```

**ステージング後の状態**:
```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   src/App.js
        new file:   new-file.txt
```

**git diffの出力例**:
```bash
$ git diff
diff --git a/src/App.js b/src/App.js
index 1234567..abcdefg 100644
--- a/src/App.js
+++ b/src/App.js
@@ -1,5 +1,6 @@
 function App() {
   return (
     <div className="App">
+      <h1>Hello World!</h1>
       <p>Welcome to my app</p>
     </div>
   );
```

### 必須: 変更のステージング（コミット準備）

**ステージングとは**: コミット（保存）する変更を選ぶ作業

```bash
# 特定のファイルをステージング（コミット準備に追加）
git add filename.txt

# すべての変更をステージング
git add .

# ステージングの確認
git status
```

** ステージングのイメージ**: 
- 変更したファイルを「コミット用の箱」に入れる作業
- 箱に入れたファイルだけがコミットされる

### 必須: コミット（変更の記録）

```bash
# コミットメッセージと一緒に記録
git commit -m "feat: ログイン機能を追加"
```

** コミットメッセージのルール**:
- `feat:` 新機能
- `fix:` バグ修正
- `docs:` ドキュメント更新
- `style:` コードの見た目修正

### 必須: リモートへの送信

```bash
# 変更をGitHubに送信
git push origin main
```

** ポイント**: `main`はブランチ名。チームによっては`master`の場合も

---

## 5. ブランチでの作業

### なぜブランチを使うのか？

**疑問**: 「メインブランチだけで作業すればいいのでは？」

**答え**: ブランチを使うことで以下のメリットがあります：

1. **安全な開発**: メインブランチを壊すことなく新機能を開発できる
2. **並行作業**: 複数人が同時に異なる機能を開発できる
3. **品質管理**: プルリクエストでコードレビューができる
4. **問題の分離**: バグが発生しても他の機能に影響しない

**具体例**:
```
メインブランチ (main)
├── 機能Aの開発 (feature/login)
├── 機能Bの開発 (feature/payment)
└── バグ修正 (fix/header-bug)
```

** ポイント**: メインブランチは常に安定した状態を保ち、各機能は独立して開発します

### 必須: ブランチの作成と切り替え

```bash
# 新しいブランチを作成して切り替え
git checkout -b feature/login

# または（新しい書き方）
git switch -c feature/login
```

** ブランチ名のルール**:
- `feature/機能名`: 新機能開発
- `fix/バグ名`: バグ修正
- `hotfix/緊急修正名`: 緊急修正

### 必須: ブランチの確認

```bash
# 現在のブランチを確認
git branch

# すべてのブランチ（リモート含む）を確認
git branch -a
```

### 必須: ブランチの切り替え

```bash
# 別のブランチに切り替え
git checkout main
# または
git switch main
```

### 必須: 新しいブランチの初回プッシュ

```bash
# 新しいブランチを初回プッシュする時は -u オプションが必要
git push -u origin feature/login

# 2回目以降は -u なしでOK
git push
```

** ポイント**: `-u`オプションは上流ブランチを設定するため、初回のみ必要

---

## 6. プルリクエスト

### 必須: 最新の変更を取得

```bash
# メインブランチに切り替え
git checkout main

# 最新の変更を取得
git pull origin main
```

### 必須: プルリクエストの作成

1. **GitHubのWeb画面で**:
   - 「Compare & pull request」ボタンをクリック
   - タイトルと説明を記入
   - 「Create pull request」をクリック

2. **レビュー待ち**:
   - チームメンバーがコードを確認
   - コメントや修正依頼があれば対応

3. **マージ**:
   - 承認後、「Merge pull request」をクリック

### 必須: マージ後のブランチ整理

**目的**: 完了したブランチを整理してリポジトリをクリーンに保つ

```bash
# メインブランチに切り替え
git checkout main

# 最新の変更を取得（マージされた内容を含む）
git pull origin main

# ローカルの不要なブランチを削除
git branch -d feature/login

# リモートのブランチも削除
git push origin --delete feature/login
```

** ポイント**: ブランチの削除は必須ではありませんが、整理のために行うと良い

---

## 7. コンフリクトの解決

### 重要: コンフリクトの発生

**コンフリクトとは**: 同じファイルの同じ行を複数人が変更した時に発生

```bash
# プル時にコンフリクトが発生
git pull origin main
```

**エラーメッセージ例**:
```
Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

### 重要: コンフリクトの解決手順

1. **コンフリクトファイルを開く**
2. **以下のマーカーを探す**:
   ```
   <<<<<<< HEAD
   あなたの変更
   =======
   他の人の変更
   >>>>>>> branch-name
   ```
3. **手動で編集して統合**
4. **マーカーを削除**
5. **解決完了をコミット**:
   ```bash
   git add .
   git commit -m "resolve: コンフリクトを解決"
   ```

---

## 8. 作業の一時保存

### 重要: ステッシュ（一時保存）

```bash
# 作業中の変更を一時保存
git stash

# 保存した変更を復元
git stash pop

# 保存した変更の一覧を確認
git stash list
```

**使用場面**:
- 作業途中で急に別のブランチに切り替えたい
- 他のメンバーの変更を確認したい

---

## 9. 変更の取り消しと履歴確認

### 重要: ステージングの取り消し

```bash
# ステージングした変更を取り消し（箱から出す）
git reset HEAD filename.txt
```

### 重要: 直前のコミットの修正

```bash
# 直前のコミットメッセージを修正
git commit --amend -m "新しいメッセージ"
```

** 注意**: プッシュ前のみ使用可能

### 重要: コミット履歴の確認と移動

**目的**: 過去のコミットを確認したり、特定の時点から作業を再開したりする

```bash
# コミット履歴を確認
git log

# 簡潔な履歴を確認
git log --oneline

# 特定のコミットを確認（一時的、デタッチドヘッド状態）
git checkout <commit-hash>

# 元のブランチに戻る
git checkout main
```

**実際の出力例**:

**git log（詳細版）**:
```bash
$ git log
commit abc1234567890def1234567890abcdef12345678
Author: 田中太郎 <tanaka@example.com>
Date:   Mon Jan 15 14:30:25 2024 +0900

    feat: ログイン機能を追加

commit def0987654321fed0987654321fed0987654321
Author: 佐藤花子 <sato@example.com>
Date:   Mon Jan 15 10:15:10 2024 +0900

    fix: バグを修正

commit 1234567890abcdef1234567890abcdef12345678
Author: 田中太郎 <tanaka@example.com>
Date:   Sun Jan 14 16:45:30 2024 +0900

    docs: READMEを更新
```

**git log --oneline（簡潔版）**:
```bash
$ git log --oneline
abc1234 feat: ログイン機能を追加
def0987 fix: バグを修正
1234567 docs: READMEを更新
```

### 重要: 過去のコミットから新しいブランチを作成

**目的**: 過去の特定の時点から新しい開発ラインを開始する

```bash
# 特定のコミットから新しいブランチを作成
git checkout -b new-branch-name <commit-hash>

# または（新しい書き方）
git switch -c new-branch-name <commit-hash>
```

** 使用場面**:
- 仕様書の変更履歴を確認したい
- 過去のバージョンと現在の違いを比較したい
- 特定の時点のコードを確認したい
- **過去のコミットから新しい機能を開発したい**
- **バグが発生した時点から修正を開始したい**

** 注意**: 
- `git checkout <commit-hash>`は一時的な移動です。作業はしないでください
- 作業を再開したい場合は、必ず新しいブランチを作成してください

---

## 10. フォークとテンプレート活用

### オプション: フォークの基本

**フォークとは**: 他の人のリポジトリを自分のアカウントにコピー

1. **GitHubでフォーク**:
   - リポジトリページの「Fork」ボタンをクリック

2. **フォークしたリポジトリをクローン**:
   ```bash
   git clone https://github.com/your-username/forked-repo.git
   ```

---

## 11. Issue管理

### オプション: Issueの基本操作

**Issueとは**: バグ報告や機能要望を管理する機能

1. **Issueの作成**:
   - リポジトリの「Issues」タブから「New issue」

2. **Issueの参照**:
   - コミットメッセージで `#1` のように番号で参照可能

---

## 12. よくあるエラーと対処法

### エラー: `fatal: not a git repository`

**原因**: GitリポジトリでないフォルダでGitコマンドを実行

**対処法**:
```bash
# 正しいプロジェクトフォルダに移動
cd /path/to/your/project
```

### エラー: `fatal: Authentication failed`

**原因**: GitHubの認証に失敗

**対処法**:
1. GitHubのユーザー名とパスワードを確認
2. パーソナルアクセストークンを使用

### エラー: `fatal: The current branch has no upstream branch`

**原因**: 新しいブランチを初回プッシュする時

**対処法**:
```bash
# 上流ブランチを設定してプッシュ
git push -u origin branch-name
```

### エラー: `Your local changes would be overwritten by merge`

**原因**: ローカルに未コミットの変更がある状態でプル

**対処法**:
```bash
# 変更を一時保存
git stash

# プルを実行
git pull origin main

# 保存した変更を復元
git stash pop
```

---

## 練習用チェックリスト

### 初回参加時
- [ ] リポジトリをクローン
- [ ] ブランチを作成
- [ ] 簡単な変更をコミット
- [ ] プッシュしてプルリクエスト作成

### 日常作業
- [ ] 最新の変更をプル
- [ ] ブランチで作業
- [ ] 変更をコミット・プッシュ
- [ ] プルリクエストでマージ

### トラブル時
- [ ] コンフリクトを解決
- [ ] ステッシュで作業を一時保存
- [ ] エラーメッセージを確認

---

## ハッカソンでの典型的なワークフロー

### ブランチ戦略の基本
**ハッカソンでは以下のような流れで作業します**:
1. **メインブランチ**: 常に安定した状態を保つ
2. **機能ブランチ**: 各メンバーが独立して機能を開発
3. **プルリクエスト**: 完成した機能をメインに統合

### 1. プロジェクト開始時
```bash
# 1. リポジトリをクローン
git clone https://github.com/team/project.git
cd project

# 2. 自分のブランチを作成
git checkout -b feature/my-feature

# 3. 作業開始
```

### 2. 作業中
```bash
# 1. 変更を確認
git status

# 2. 変更をステージング
git add .

# 3. コミット
git commit -m "feat: 新機能を実装"

# 4. プッシュ（初回は -u オプションが必要）
git push -u origin feature/my-feature

# 5. 2回目以降のプッシュ
git push
```

### 3. チームとの同期
```bash
# 1. メインブランチに切り替え
git checkout main

# 2. 最新の変更を取得
git pull origin main

# 3. 自分のブランチに戻る
git checkout feature/my-feature

# 4. メインの変更をマージ
git merge main
```

---

## 13. その他の便利な機能

### オプション: .gitignoreファイルの作成

**目的**: 不要なファイルをGitの管理から除外する

**ハッカソンでよく使う`.gitignore`の例**:
```gitignore
# Node.js
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# ビルド生成物
dist/
build/
*.tgz
*.tar.gz

# 環境変数ファイル
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# IDE設定
.vscode/
.idea/
*.swp
*.swo

# OS生成ファイル
.DS_Store
Thumbs.db

# ログファイル
*.log
logs/

# 一時ファイル
*.tmp
*.temp
```

**作成方法**:
```bash
# プロジェクトのルートディレクトリに作成
touch .gitignore

# または、エディタで直接作成
```

** ポイント**: プロジェクトの種類に応じて適切な`.gitignore`を設定しましょう

---

## 参考資料

### 公式ドキュメント
- [Git公式ドキュメント](https://git-scm.com/doc)
- [GitHub公式ガイド](https://guides.github.com/)
- [Pro Git Book](https://git-scm.com/book)

### 初心者向け学習リソース
- [Progate Git入門](https://prog-8.com/docs/git-env) - 環境構築から基本操作まで
- [ドットインストール Git入門](https://dotinstall.com/lessons/basic_git) - 動画で分かりやすく
- [サルでもわかるGit入門](https://backlog.com/ja/git-tutorial/) - 導入から実践まで
- [GitHub Learning Lab](https://lab.github.com/) - インタラクティブな学習

---

** 学習のコツ**: 実際に手を動かしながら、このガイドのコマンドを試してみてください。エラーが出ても大丈夫です。エラーから学ぶことが最も効果的です！

---

## 改善履歴（編集ログ）

このドキュメントをより実践的で学びやすくするために行った主な改善点を記録します。

- 目次と本文から装飾アイコンを削除し、可読性を改善
- 「ステージング」の概念を初心者向けに注釈（コミット用の箱の比喩）で補足
- 「新しいブランチの初回プッシュ（git push -u）」を追加し、位置を「ブランチでの作業」に整理
- よくある質問（FAQ）を追加し、学習の迷いを低減
- コマンド出力例を多数追加（git status / git log / git diff / git remote -v）
- コンフリクト解決、コミット履歴の確認、過去コミットからのブランチ作成を明確化
- マージ後のブランチ整理（ローカル/リモート削除）を追加
- .gitignoreの実用例（ハッカソン想定）を追加
- ブランチ戦略の目的（安全・並行・品質・分離）を明記し、ワークフローに統合

今後の追加候補:
- 画面キャプチャの挿入（git status / log / diff / PR 画面など）
- 初学者向けの用語集（reset/revert/merge/rebase などの最小構成）
