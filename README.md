# なんじゃこれ？（なんじゃもんじゃ風オンライン対戦ゲーム）

スマホから遊べる、なんじゃもんじゃ風のオンライン対戦カードゲームです。
単一の `najamonja.html` だけで動く静的サイトなので、GitHubリポジトリ＋GitHub Pages（または Vercel）でそのまま公開できます。

## 1. Firebase Realtime Database を用意する

1. https://console.firebase.google.com/ でプロジェクトを新規作成
2. 左メニュー「構築」→「Realtime Database」→ データベースを作成（ロケーションは任意、開始モードは「テストモード」でOK）
3. 左メニュー「プロジェクトの概要」の歯車 →「プロジェクトを設定」→「全般」タブの下の方にある「マイアプリ」で「</>（ウェブ）」を追加
4. 表示された `firebaseConfig` の中身を、`najamonja.html` の中の以下の部分に貼り付ける

   ```js
   var FIREBASE_CONFIG = {
     apiKey: "...",
     authDomain: "...",
     databaseURL: "...",
     projectId: "...",
     storageBucket: "...",
     messagingSenderId: "...",
     appId: "..."
   };
   ```

5. Realtime Database の「ルール」タブに、このリポジトリの `database.rules.json` の中身を貼り付けて公開する
   - 4文字の部屋コードの部屋だけ読み書きでき、ゲームが使う形のデータ（名前は8文字まで、など）しか書き込めないようにしています
   - 部屋の一覧を取得したり、全部の部屋をまとめて消したりはできません

## 2. GitHubリポジトリを作る

```bash
git init
git add najamonja.html README.md
git commit -m "なんじゃもんじゃオンライン対戦ゲーム"
git branch -M main
git remote add origin https://github.com/<あなたのユーザー名>/<リポジトリ名>.git
git push -u origin main
```

## 3. 公開する（どちらか）

### GitHub Pages（一番手軽）
1. リポジトリの Settings → Pages
2. Source を「Deploy from a branch」、Branch を `main` / `(root)` に設定
3. 数十秒〜数分で `https://<ユーザー名>.github.io/<リポジトリ名>/najamonja.html` が公開されます

### Vercel
1. https://vercel.com/ にログインしてこのGitHubリポジトリをインポート
2. ビルド設定は不要（静的HTMLなのでそのままデプロイ可能）
3. デプロイ後に発行されるURLで公開されます

## 補足

- サーバー側のプログラムは不要です。オンライン対戦の同期はブラウザから直接 Firebase Realtime Database に読み書きすることで実現しています。
- `FIREBASE_CONFIG` はクライアントに公開される値ですが、Firebaseの仕組み上これは想定内です（アクセス制御は Realtime Database の「ルール」側で行います）。荒らし対策や利用制限をしたい場合は、ルールをもう少し厳格にすることをおすすめします。
