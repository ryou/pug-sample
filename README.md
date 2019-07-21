# Pugサンプル

## 覚書

### コンポーネント化に関して

`include`機能があるが、柔軟性に乏しいので`mixin`機能を使うのがいい。

かつ、コンポーネントのオプションを指定する際は引数機能ではなくattributesを利用したほうがいい。

理由としては例えば、

```
mixin Section(heading=undefined, note=undefined)
  .Section
    if heading
      .Section_heading= heading
    if note
      .Section_note= note
    .Section_content
      block
```

のようなコンポーネントに関して、「headingがないけどnoteがある」の場合引数だと

```
+Section(heading=undefined, note="テキスト")
  p hogehoge
```

みたいな感じで`heading=undefined`をいちいち書かないといけない。

これを以下のようにattributesで指定しておくと

```
mixin Section
  .Section
    if attributes.heading
      .Section_heading= heading
    if attributes.note
      .Section_note= note
    .Section_content
      block
```

```
+Section(note="テキスト")
  p hogehoge
```

みたいな感じで済む。


### コンポーネントに複数のblockを与えたい場合

残念ながら、PugのmixinにはVueみたいな名前付きblockが存在しないみたい。

なので、コンポーネントを複数のコンポーネントで構成する形で対応する。

具体的には`Modal`コンポーネントを参考に。

### Prettierに関して

[GitHub - prettier/plugin-pug: Prettier Pug Plugin](https://github.com/prettier/plugin-pug)

2019/07/20現在`plugin-pug`というプラグインがあるが、開発開始から１ヶ月も経過しておらずPrettierのPlugin機能自体ベータという事もあり仕事で使うのは割と危険。（`plugin-pug`の公式にもProductionUseには適さないって書いている）

なので、潔く諦めてEditorConfigで妥協したほうが良さそう。

### dart-sassでwatchする際のinput/outputの指定

`dart-sass`で`watch`する際に

```
sass --watch ./src/scss/:./dist/assets/css/
```

ってな感じで`./`をつけてるとうまく監視できなかった

```
sass --watch src/scss/:dist/assets/css/
```

ってやったら出来た。理由不明。

### WebStormでstylelint

`Preference > Language > StyleSheets > stylelint`で`Enable`のチェックをONにしたらstylelintのチェックが有効になる。
