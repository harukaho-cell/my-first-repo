# 公開手順

このリポジトリの `public/` フォルダを Web サイトとして公開するための手順です。

現在は **GitHub Pages** で公開しています（下記）。Cloudflare Pages で公開したい場合の手順も後半に残してあります。

---

## 現在の公開方法: GitHub Pages（設定済み・自動）

`.github/workflows/deploy-pages.yml` が `main` への push を検知して、`public/` の中身を自動で公開します。
**追加の設定やアカウント連携は不要です。** 公開URLはこちら。

```
https://harukaho-cell.github.io/my-first-repo/
```

### 更新のしかた

`public/` の中身を変更して `main` に反映するだけです。1〜2分で公開ページに反映されます。

```bash
git add .
git commit -m "ページを更新"
git push
```

### 公開状況の確認

リポジトリの **Actions** タブを開くと、公開処理の進行状況と結果が見られます。
緑のチェックが付けば公開完了、赤い×が付いていればそこにエラー内容が表示されます。

---

## この構成について

公開されるのは **`public/` フォルダの中身だけ** です。

```
my-first-repo/
├── public/                      ← ここが公開される（＝Webサイトの中身）
│   ├── index.html               ← トップページ（床屋ぐっさん）
│   └── sakura-seikotsuin.html   ← 以前の練習ページ（残してあります）
├── hello.py         ← 公開されない（練習用ファイル）
├── hello.txt        ← 公開されない
└── *.jpg            ← 公開されない
```

`public/index.html` が `https://（サイト名）.pages.dev/` として表示されます。
以前の整骨院のページは `https://（サイト名）.pages.dev/sakura-seikotsuin.html` で見られます。
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

---

# （参考）Cloudflare Pages で公開する場合

GitHub Pages ではなく Cloudflare Pages を使いたい場合の手順です。
独自ドメインや配信速度、アクセス解析などで Cloudflare を使いたくなったときに参照してください。
※ 両方同時に公開することもできます（URLが2つになります）。

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
| Project name | 好きな名前（例: `tokoya-gussan`）※ URL になります |
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

## 公開前に直したほうがよい箇所

現在のページには、埋め込み用のプレースホルダー（仮の文字）がそのまま残っています。
このまま公開すると、ページ上に次の文字が見えてしまいます。

| 場所 | 現在の表示 | やること |
| --- | --- | --- |
| ヘッダー下の3項目 | `【定休日】` | 実際の定休日を入れる |
| スタッフ紹介 | `【店長名】` `【スタッフ名】` | 実際のお名前を入れる |
| アクセス | 駐車場が `【要確認】` | 有無・台数を入れる |
| お知らせ | `お知らせのタイトルをここに` | 実際のお知らせに差し替えるか、その行を削除する |
| お客様の声 | `いただいた感想をそのまま掲載します。実際の口コミ文をここに入れてください。` | 実際の声に差し替えるか、セクションごと削除する |

また、写真が入る場所は現在グレーの枠になっています（`店内・施術中の写真（縦）`、`作例 01` など）。
写真を用意できたら `public/` に画像を置いて差し替えられます。この作業もお手伝いできます。

掲載されている住所・電話番号・料金が実際のお店の情報と合っているかも、公開前に一度ご確認ください。

## そのほかの注意点

- Cloudflare Pages が見るのは **Production branch に設定したブランチ（通常は `main`）** です。
  作業ブランチに push しただけではサイトは更新されません。`main` にマージして初めて反映されます。
  （作業ブランチへの push は「プレビュー版」として別URLで確認できます）
- ページのフォントは Google Fonts、地図は Google マップを外部から読み込んでいます。
  公開後のページでは通常どおり表示されます。
