# Cloudflare Pages での公開手順

このリポジトリを Cloudflare Pages に連携して、Web ページを公開するための手順です。
一度つなげてしまえば、あとは `git push` するだけでサイトが自動で更新されます。

---

## この構成について

公開されるのは **`public/` フォルダの中身だけ** です。

```
my-first-repo/
├── public/          ← ここが公開される（＝Webサイトの中身）
│   └── index.html   ← トップページ
├── hello.py         ← 公開されない（練習用ファイル）
├── hello.txt        ← 公開されない
└── *.jpg            ← 公開されない
```

`public/index.html` が `https://（サイト名）.pages.dev/` として表示されます。
練習用のファイルや画像は公開対象から外れるので、そのまま置いておいて問題ありません。

---

## 手順1: 公開したいファイルを `public/` に入れる

ダウンロードフォルダにある HTML ファイルを公開したい場合は、それを `public/` の中に入れます。

- **トップページにしたい場合** → ファイル名を `index.html` にして `public/index.html` を置き換える
- **別のページとして追加したい場合** → 例えば `public/about.html` として置くと
  `https://（サイト名）.pages.dev/about.html` で表示される

画像や CSS を使っている場合は、それらも一緒に `public/` の中に入れてください。
HTML から参照するパスは `public/` の中での位置関係で書きます（例: `public/img/logo.png` なら `<img src="img/logo.png">`）。

### GitHub の画面からアップロードする方法

コマンドを使わずにブラウザだけで追加できます。

1. https://github.com/harukaho-cell/my-first-repo を開く
2. `public` フォルダをクリックして開く
3. 右上の **Add file** → **Upload files** をクリック
4. ダウンロードフォルダのファイルをドラッグ＆ドロップ
5. 下の **Commit changes** をクリック

---

## 手順2: Cloudflare にサインアップ／ログイン

1. https://dash.cloudflare.com/ を開く
2. アカウントがなければ **Sign up** で作成（無料プランで公開できます）

---

## 手順3: Cloudflare Pages と GitHub をつなぐ

1. 左メニューの **Compute (Workers)** → **Workers & Pages** を開く
2. **Create** ボタンをクリック
3. **Pages** タブを選び、**Connect to Git** をクリック
4. **GitHub** を選んで連携を許可する
   - 初回は GitHub の認証画面が出ます
   - リポジトリの選択で **Only select repositories** → `my-first-repo` を選ぶのがおすすめです
5. リポジトリ一覧から **my-first-repo** を選び、**Begin setup** をクリック

---

## 手順4: ビルド設定を入力する

入力画面が出たら、次のように設定します。

| 項目 | 入力する値 |
| --- | --- |
| Project name | 好きな名前（例: `sakura-seikotsuin`）※ URL になります |
| Production branch | `main` |
| Framework preset | `None` |
| Build command | **空のまま**（何も入力しない） |
| Build output directory | `public` |

> HTML ファイルをそのまま公開するだけなので、ビルドコマンドは不要です。
> **Build output directory に `public` と入れる**のがいちばん大事なポイントです。

入力したら **Save and Deploy** をクリックします。

---

## 手順5: 公開されたか確認する

1分ほどで公開が完了し、URL が表示されます。

```
https://（Project name）.pages.dev
```

このURLを開いてページが表示されれば公開成功です。

---

## 公開したあとの更新方法

`public/` の中身を変更して push すると、Cloudflare が自動で検知して数十秒で反映されます。

```bash
git add .
git commit -m "ページを更新"
git push
```

---

## 独自ドメインを使いたい場合

1. Cloudflare Pages のプロジェクト画面 → **Custom domains** タブ
2. **Set up a domain** をクリックし、使いたいドメイン名を入力
3. 画面の指示に従って DNS を設定

Cloudflare でドメインを管理している場合は自動で設定されます。
他社で取得したドメインの場合は、表示された CNAME レコードを、そのドメインの管理画面に登録します。

---

## 注意点

- 現在の `public/index.html` は練習用のページで、電話番号・住所は `XXX` のダミーのままです。
  実際に公開する前に、実在する情報に置き換えるか、ダミーだと分かる状態にしておいてください。
- ページ内の写真は Pexels の外部URLを直接読み込んでいます。公開後もそのまま表示されますが、
  自前の画像に差し替える場合は `public/` に画像を置いてパスを書き換えてください。
- Cloudflare Pages が見るのは **Production branch に設定したブランチ（通常は `main`）** です。
  作業ブランチに push しただけではサイトは更新されません。`main` にマージして初めて反映されます。
  （作業ブランチへの push は「プレビュー版」として別URLで確認できます）
