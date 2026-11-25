====================================================================
リンクボタンコンポーネント 仕様書
====================================================================

■ 1. 概要
PC・タブレット・スマホの画面幅に応じて列数が変化する、アイコン付きのリンクボタン一覧コンポーネント。
テキストが複数行に改行された場合でも、行内のボタンの高さが自動で揃い、アイコンおよびテキストが上下中央に美しく配置される設計。

--------------------------------------------------------------------
■ 2. レイアウト・レスポンシブ仕様
--------------------------------------------------------------------
・レイアウト方式：Flexbox（display: flex; flex-wrap: wrap;）
・ボタン間の余白（gap）：縦横ともに 20px
・列数の切り替え（ブレイクポイント）：
  - PC（画面幅 1025px以上）      ：横 3列（幅: calc((100% - 40px) / 3)）
  - タブレット（画面幅 1024px以下）：横 2列（幅: calc((100% - 20px) / 2)）
  - スマホ（画面幅 640px以下）   ：縦 1列（幅: 100%）
・親要素への追従：
  親要素（div や nav など）の幅を 100% として等分計算するため、配置先のコンテナ幅に合わせて自動調整される。

--------------------------------------------------------------------
■ 3. デザイン・装飾仕様
--------------------------------------------------------------------
【通常時】
・背景色      ：#FFFFFF
・枠線        ：1px solid #c7ac61
・角丸        ：12px
・文字色      ：#c7ac61（※視認性確保のため枠線と同色に調整）
・文字サイズ  ：15px / 太字（bold） / 行間 1.5
・内側余白    ：上下 18px / 左右 20px
・下線        ：なし

【ホバー時（:hover）】
・背景色      ：#fffde7（トランジション 0.25s ease）
・下線        ：テキスト部分（.link-button-text）にのみ下線を表示
                ※アイコン（<i>）には下線をつけない

【アイコン仕様】
・使用アイコン：Font Awesome 7 無料版（右向き矢印丸アイコン：fa-solid fa-circle-arrow-right）
・色          ：文字色と同じ（親要素の color を継承）
・サイズ      ：1.2em
・余白        ：右側に 12px（テキストとの間隔）
・縮み防止    ：flex-shrink: 0; を指定し、長文時でもアイコンサイズを維持

--------------------------------------------------------------------
■ 4. テキスト改行・整列の制御仕様
--------------------------------------------------------------------
・上下中央揃え：
  ボタン本体（.link-button）に display: flex; align-items: center; を指定。
  テキストが1行でも2行以上でも、ボタンの外周に対して上下中央に配置される。
・高さの統一：
  リストアイテム（.link-button-item）に display: flex; を指定。
  同じ横列に1行のボタンと2行のボタンが混在しても、最も高いボタンに高さが揃う。
・改行時の左余白維持（インデント）：
  アイコン（<i>）とテキスト（.link-button-text）をFlexアイテムとして横並びにしているため、
  テキストが2行以上に改行されてもアイコンの下に潜り込まず、常にアイコン幅＋12pxの左余白が保たれる。
・改行位置のコントロール：
  - 改行候補位置に <wbr> タグを挿入。
  - 途中で改行させたくない単語・フレーズは <span class="nb">...</span>（display: inline-block;）で囲む。

--------------------------------------------------------------------
■ 5. 既存サイトへ組み込む際のポイント・注意点
--------------------------------------------------------------------
・親要素の幅へのフィット（max-widthの調整）：
  サンプルの .link-button-list に設定している「max-width: 1200px;」は、
  親要素の幅が1200pxより狭い場合は親要素の幅に自動フィットする。
  1200pxより広い親要素いっぱいに広げたい場合は、この1行を削除すること。
・動作確認用スタイルの除外：
  サンプルHTML内の body に対する padding や background-color は動作確認用のため、
  既存ページへ組み込む際はコピーしないこと。
・配置場所によるブレイクポイントの注意点（メインカラム vs サイドバー）：
  本仕様の列数切り替え（3列→2列→1列）は「ブラウザ全体の画面幅（@media）」を基準としている。
  - メインカラム等の広いエリアに配置する場合は問題なく機能する。
  - サイドバー等のもともと幅が狭いエリアに配置すると、PC閲覧時に狭い幅の中で無理やり3分割されてしまうため注意が必要。
  （狭いエリアで使う場合は、CSS Gridの auto-fit や @container クエリへの変更を検討すること）

--------------------------------------------------------------------
■ 6. 実装コード（HTML / CSS）
--------------------------------------------------------------------
【HTML構造例】
<ul class="link-button-list">
  <li class="link-button-item">
    <a href="#" class="link-button">
      <i class="fa-solid fa-circle-arrow-right"></i>
      <span class="link-button-text">中央歯科衛生士調理製菓専門学校<wbr> <span class="nb">調理製菓総合学科</span></span>
    </a>
  </li>
  <!-- 以降、必要な数だけ li を繰り返す -->
</ul>

【CSS（head内または外部CSSに記述）】
* {
  box-sizing: border-box;
}

.link-button-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  max-width: 1200px; /* 親要素いっぱいに広げる場合は削除可 */
  margin: 0 auto;
  padding: 0;
  list-style: none;
}

.link-button-item {
  width: calc((100% - 40px) / 3);
  display: flex;
}

.link-button {
  display: flex;
  align-items: center;
  width: 100%;
  padding: 18px 20px;
  background-color: #FFFFFF;
  border: 1px solid #c7ac61;
  border-radius: 12px;
  color: #c7ac61;
  text-decoration: none;
  font-weight: bold;
  font-size: 15px;
  line-height: 1.5;
  transition: background-color 0.25s ease;
}

.link-button:hover {
  background-color: #fffde7;
}

.link-button:hover .link-button-text {
  text-decoration: underline;
}

.link-button i {
  flex-shrink: 0;
  margin-right: 12px;
  font-size: 1.2em;
}

.link-button-text {
  display: block;
}

.nb {
  display: inline-block;
}

@media screen and (max-width: 1024px) {
  .link-button-item {
    width: calc((100% - 20px) / 2);
  }
}

@media screen and (max-width: 640px) {
  .link-button-item {
    width: 100%;
  }
}