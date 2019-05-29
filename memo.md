# 概要
- App.jsがマークアップ画面（ほぼhtml）
- index.cssがcss部分
- index.js
- Appシリーズがフロント系
- indexシリーズがコンポーネント系

# どうやって勉強したか
下記に書いているメモと、index-js-memo.mdにすべて記載した

# reactのルーティング？を理解する
- public/index.htmlで初期設定を行なっている
    - ショートカットアイコン
    - ビューポート
    - theme-color
    - manifest（以下に説明あり）
- ルーティングがpugっぽくてすごく理解しやすい〜

    
# create-react-appが裏でなにをしている？
- create-react-appを利用することで膨大なpackage.jsonを必要とするreactの環境構築をしている
- 試しに`$react-scripts eject`をしてみると、`react-scripts`にまとめられていたモジュールが全てpackage.jsonに解放してくれるので試してみては？
- 要するに、おすすめの環境を作ってくれているだけ。

# manifest
- manifest.json でWebアプリを「ホーム画面に追加」自動バナー表示に対応させる
    - https://qiita.com/tmtysk/items/2c5da83feec45b4ee36f
- Webアプリの下にホーム画面に追加ボタンを表示をバナー表示として表示させるやつ
- serviseWorker.js で

# serviseworker?
https://app.codegrid.net/entry/2016-service-worker-1
- Service Workerが一度Webページからインストールされると、Webページとは独立したライフサイクルの中で動作します。たとえば、オフライン状態でも、Webページを表示していたタブが閉じていても、必要があればService Workerが作動します。
- Servise Workerもライフサイクルを持っているので、javascriptとはまた別のライフサイクルを持つことができる
- Service Workerが持つ6つの状態
    - parsed
        - 初期状態
    - installing
        - servise workerをインストールしている状態
    - ちょっとめんどくさくなってきたので割愛（気が向いたら）

# メモ
- Reactでは、親から子へとpropsを渡すことで、アプリ内を情報が流れる
- すべてコンストラクタを super(props) の呼び出しから始める
- setState をコンポーネント内で呼び出すと、React はその内部の子コンポーネントも自動的に更新します。
- 複数の子要素からデータを集めたい、または 2 つの子コンポーネントに互いにやりとりさせたいと思った場合は、代わりに親コンポーネント内で共有の state を宣言する必要があります。親コンポーネントは props を使うことで子に情報を返すことができます。こうすることで、子コンポーネントが兄弟同士、あるいは親との間で常に同期されるようになります。

# 終わってから感想
- まず各コンポーネントでどんな設計ができるかを想像できるようにする
- returnはそのクラスの役割を示す
    - Boardで考えると
        - squareのstateをvalueに代入する
        - onClick :ボタンを押すと、このクラス内のhandleClick内の処理を行う
