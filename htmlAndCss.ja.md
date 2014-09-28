# HTML/CSS Coding Guide
for Rails project of LOUPE

## 参考
[「Google HTML/CSS Style Guide」 適当に和訳してみた](http://re-dzine.net/2012/05/google-htmlcss-style-guide/)  
[GitHubのHTML/CSS Styleguideを適当に和訳してみた](http://re-dzine.net/2012/09/github-htmlcss-styleguide/)  
[CodeGrid : SMACSSによるCSSの設計](https://app.codegrid.net/entry/smacss-1)  
[CyberAgent : メンテナブルCSS](https://www.cyberagent.co.jp/recruit/techreport/report/id=7926)  
[ちゃんとCSSを書くために - CSS/Sass設計の話](http://www.slideshare.net/hiloki/a-good-css-and-sass-architecture)  
[BEMという命名規則とSass3.3の新しい記法](http://blog.ruedap.com/2013/10/29/block-element-modifier)

## 基本
LOUPEの[Ruby Coding Guide / 基本](https://github.com/lo-upe/styleguide/blob/master/ruby.ja.md)の項目に準じる。

## ファイル配置
Ruby on Rails Guides に従う。
参考：[#2.2 Asset Organization](http://guides.rubyonrails.org/asset_pipeline.html#asset-organization)
* app/assets は対象アプリケーションの独自作成ファイル置き場
* lib/assets は複数アプリケーションにまたがって利用されるライブラリファイル置き場
* vendor/assets はVendor(サードパーティ)製ライブラリファイル置き場

※ app/assets 内のファイル内でさらにどのようにディレクトリ構成を行うかは今後の検討事項となる。

## マークアップ方針
* 新規に書く場合はSMACSSを意識する。
* 今後柔軟にするため、要検討事項となる。
* 参考：[今必要なCSSアーキテクチャ](http://www.slideshare.net/MayuKimura/css-25547100)
* 参考：[HOW-I-IMPROVED-MY-WORKFLOW-WITH-SMACSS-SASS](http://bramsmulders.com/how-i-improved-my-workflow-with-smacss-sass.html)
* また、以下に記載するコーディングルールに縛られすぎないこと。

## HTML（slim）について
* マルチメディアの要素には代替コンテンツを提供する(alt="説明"を書く)こと。
  * 装飾的な要素の画像にはalt=""としておくこと。

```slim
  img src="cat.png" alt="かわいい猫"
```

* 他、随時気づいた点から追記

## CSS（Sass）について
* プロパティ宣言の```:```の後ろにはスペースを入れる。
* ルール宣言の```{```の前にはスペースを入れる。
* すべてのプロパティの終端にはセミコロンを書く。
* 階層がわかるようにブロック単位でコードをインデントする。（SCSS）

```sass
// good
body {
  font-size: 10px;
  @media(max-width: 767px) {
    padding-left: 0;
  }
}
```

* 別々のセレクタとプロパティは改行して書く。
* 別々のCSSルールは改行して一行間を空けて書く。

```sass
// good
h1,
h2,
h3 {
  font-weight: normal;
  line-height: 1.2;
}

p {
  font-weight: bold;
}

// bad
h1,h2,h3 {
  font-weight: normal; line-height: 1.2;
}
p {
  font-size: 10px;
}
```

* rgbaを使用しない限り、16進数のカラーコードを使う。
* HEX形式のカラーコードで3文字で表現できるものは3文字にする。

```sass
// good
color: #fff;
color: rgba(0, 0, 0, 0.8);

//bad
color: #ffffff;
```

* アルファベットの順に記述する。
* ベンダープレフィックスは無視するが、接頭辞の中での順序は保つこと。
* ```font-size```プロパティには、```px```を使うこと。
* ```line-height```は単位なしが良い。
  * 親要素のパーセンテージ値を継承しないため。
  * 単位なしの場合、値は```font-size```の乗数に基づく。

```sass
// good
.menu {
  border: 1px solid;
  -moz-border-radius: 4px;
  -webkit-border-radius: 4px;
  display: inline-block;
  font-size: 10px;
  font-weight: normal;
  margin: 1px 0;
  padding: 5px 2px 5px 3px;
  vertical-align: middle;
  width 100%;
}
```

* コメントにTODOを記載しておくときは、TODOキーワードを用いる。

```sass
/* TODO: 中央寄せにする */
.menu {
  font-size: 10px;
}
```


## Sassの特徴的な利用方法について
* 参考：[CSS-TRICKS / Sass Style Guide](http://css-tricks.com/sass-style-guide/)
* @extend(s)は最初に書く。
* 次に通常のスタイルを書く。
* 次に@include(s)を書く。
* セレクタは常に入れ子の内部（できるだけ最後）に書くよう心がける。

```sass
// good
.weather {
  @extends  %module;
  background: LightCyan;
  @include transition(all 0.3x ease-out);
  > h3 {
    border-bottom: 1px solid white;
    @include transform(rotate(90deg));
  }
}
```

* ネストは３つまでに抑える。
  * 3つ以上になるときはやり過ぎで修正する必要があると考える。
  * SASSなので必要に応じて継承(@extend)を用いて解決する。
* ネスト表記は50行以内に抑えるようにする。
  * ネストの利点は1画面でみやすくなること、それを傷つけないようにする。

```sass
// good
.weather {
  > h3 {
    @extend %line-under;
  }
}
```

* @importの表記順は以下のようにする。
  1. Vendor製依存ライブラリ
  2. 自作依存ライブラリ
  3. 自作各Patternsの外部ファイル
  4. 自作各Sectionsの外部ファイル

```sass
/* Vendor Dependencies */
@import "compass";

/* Authored Dependencies */
@import "global/colors";
@import "global/mixins";

/* Patterns */
@import "global/tabs";
@import "global/modals";

/* Sections */
@import "global/header";
@import "global/footer";
```

* 積極的に変数を用いて定義すること。
  * 0 もしくは 100% 以外の数値を繰り返して使用する場合は変数として定義する。
  * 白もしくは黒以外の色の定義も可能な限り変数を用いる。
  * 変数名の命名時はBEMっぽさを意識して考える（以下のように心がける）。
  * blockName_elementName-modifierName
    * blockName → 大きな塊の定義名（メインページ、フォームなど）
    * _elementName → 要素名（ボタン、タイトルなど）
    * -modifierName → 状態（押した、オンマウスなど）
    * 参考：[BEMを参考にしたCSS設計](http://qiita.com/mrd-takahashi/items/07dc3b4bad027daa2884)
    * ただし既存定義に明確なルールがある場合は既存のものを優先する。

```sass
//数値
$zIndex-top: 5050;
$zIndex-middle: 5000;
$zIndex-bottom: 4090;
//色
$red: #d83d36;
$blue: #00859b;
$color_btn: $red;
$color_btn-pushed: darken($color_btn, 10%);

.header {
  z-index: $zIndex-bottom;
}
.overlay {
  z-index: $zIndex-top;
}
.message_btn {
  z-index: $zIndex-middle;
  background-color: $color_btn;
  &:hover {
    background-color: $color_btn-pushed;
  }
}
```

* MediaQueryについて、必要に応じて以下のような@mixinの活用を心がける。
  * 参考：[CSS-TRICKS / Naming Media Queries](http://css-tricks.com/naming-media-queries/)
  * ```$point```には抽象的な名前をつけるべきと上記参考にあるので注意。

```sass
@mixin breakpoint($point) {
  @if $point == large {
    @media (max-width: 1600px) { @content; }
  }
  @else if $point == middle {
    @media (max-width: 1250px) { @content; }
  }
  @else if $point == small {
    @media (max-width: 650px) { @content; }
  }
}

.module {
  width: 100%;
  @include breakpoint(middle) {
    width: 50%;
  }
}
```

## 制作時メモ
Google Document(リンク共有)  
https://docs.google.com/document/d/1FCapUqci-wAkBxSu3tFD_3cucoWzlCA-XNlQzHJ81gw/edit?usp=sharing
