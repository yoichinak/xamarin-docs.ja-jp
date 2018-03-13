---
title: "MonoTouch.Dialog Json マークアップ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 843e66a7979fc1aaa86371a3406c89af3f9ba967
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json マークアップ

このページで説明 MonoTouch.Dialog のによって受け入れられる Json マークアップ[JsonElement](https://developer.xamarin.com/api/type/MonoTouch.Dialog.JsonElement/)

例を使用して開始しましょう。 JsonElement に渡すことができる完全な Json ファイルを次に示します。

```csharp
{     
  "title": "Json Sample",
  "sections": [ 
      {
          "header": "Booleans",
          "footer": "Slider or image-based",
          "id": "first-section",
          "elements": [
              { 
                  "type" : "boolean",
                  "caption" : "Demo of a Boolean",
                  "value"   : true
              }, {
                  "type": "boolean",
                  "caption" : "Boolean using images",
                  "value"   : false,
                  "on"      : "favorite.png",
                  "off"     : "~/favorited.png"
              }, {
                      "type": "root",
                      "title": "Tap for nested controller",
                      "sections": [ {
                         "header": "Nested view!",
                         "elements": [
                           {
                             "type": "boolean",
                             "caption": "Just a boolean",
                             "id": "the-boolean",
                             "value": false
                           },
                           {
                             "type": "string",
                             "caption": "Welcome to the nested controller"
                           }
                         ]
                       }
                     ]
                   }
          ]
      }, {
          "header": "Entries",
          "elements" : [
              {
                  "type": "entry",
                  "caption": "Username",
                  "value": "",
                  "placeholder": "Your account username"
              }
          ]
      }
  ]
}
```

上記のマークアップは、次の UI を生成します。

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "指定されたマークアップで作成される UI")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

ツリー内のすべての要素は、プロパティを含めることができます`"id"`です。 可能であれば実行時に個別のセクションまたは JsonElement インデクサーを使用して要素を参照します。 以下に例を示します。

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement
```

 <a name="Root_Element_Syntax" />


## <a name="root-element-syntax"></a>ルート要素の構文

ルート要素には、次の値が含まれています。

-  `title`
-  `sections` (省略可能)


ルート要素は、セクション内に入れ子になったコント ローラーを作成する、要素として表示できます。 その場合、余分なプロパティ`"type"`に設定する必要があります `"root"`

 <a name="url" />


### <a name="url"></a>url

場合、`"url"`プロパティを設定する場合は、ユーザーをこの RootElement タップ、コード ファイルを指定した url から要求は内容、新しい情報が表示されます。 これを使用して、作成することができます、ユーザーはタップ操作内容に基づいて、サーバーから、ユーザー インターフェイスを拡張します。

 <a name="group" />


### <a name="group"></a>グループ

かどうか設定すると、この設定は groupname、ルート要素です。 グループ名は、要素の入れ子になった要素の 1 つのルート要素の値として表示される概要の取得に使用されます。 これは、チェック ボックスの値またはラジオ ボタンの値のいずれかです。

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

入れ子になった要素で選択されているオプション項目を識別します。

 <a name="title" />


### <a name="title"></a>タイトル

存在する場合がなります、RootElement に使用されるタイトル

 <a name="type" />


### <a name="type"></a>型

設定する必要があります`"root"`とき (これは、コント ローラーを入れ子に使用される) セクションでは表示されます。

 <a name="sections" />


### <a name="sections"></a>sections

これは個別のセクションを含む Json 配列です。

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>セクションの構文

セクションが含まれています。

-  `header` (省略可能)
-  `footer` (省略可能)
-  `elements` 配列


 <a name="header" />


### <a name="header"></a>ヘッダー

存在する場合、ヘッダーのテキストは、セクションのキャプションとして表示されます。

 <a name="footer" />


### <a name="footer"></a>ページ フッター

存在する場合は、セクションの下部にあるフッターが表示されます。

 <a name="elements" />


### <a name="elements"></a>要素

これは、要素の配列です。 各要素が少なくとも 1 つのキーを含める必要があります、`"type"`に作成する要素の種類の識別に使用されるキー。
要素の一部がのようないくつかの一般的なプロパティを共有`"caption"`と`"value"`です。 サポートされている要素の一覧を次に示します。

-  `string` 要素 (の両方とスタイルなし)
-  `entry` 線 (標準またはパスワード)
-  `boolean` (スイッチまたはイメージを使用して) 値


文字列の要素として使用できますボタン メソッドを提供することで、セルと、アクセサーのいずれかで、ユーザーがタップしたときに呼び出される

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>レンダリング要素

レンダリング要素は、c# StringElement とに基づいて StyledStringElement と、さまざまな方法で情報を表示することができ、さまざまな方法でそれらをレンダリングすることがします。 最も単純な要素は、次のように作成できます。

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

すべての既定値の単純な文字列が表示されます: フォント、背景、テキストの色および装飾します。 これらの要素へのアクションをフックし、ボタンのような動作を設定してそれらをすることは、`"ontap"`プロパティまたは`"onaccessorytap"`プロパティ。

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

上記はクラス"Acme.PhotoLibrary"で"ShowPhotos"メソッドを呼び出します。 `"onaccessorytap"`は似ていますが、ユーザーがセルをタップする代わりのアクセサリをタップした場合は呼び出さのみが、します。 これには、アクセサーを設定することも必要があります。

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

要素を表示できる 2 つの文字列を一度に表示、1、キャプション、他にも値です。 これらの文字列が表示される方法は、スタイルに依存して、設定できるを使用して、`"style"`プロパティです。 既定値は、左側と右側の値にキャプションが表示されます。 詳細については、スタイル、セクションを参照してください。 色は、赤、緑、青、おそらくアルファ値の値を表す 16 進数の数字が続く '#' 記号を使用してエンコードされます。 内容は、RGB または RGBA のいずれかの値を表す短い形式 (3 または 4 16 進数字) でエンコードすることができます。 または RGB または RGBA のいずれかの値を表す長いフォーム (6 または 8 桁) です。 短い形式は、16 進数字の同じを 2 回作成するためのショートハンドです。 "#1bc"定数は、red としてなど、パターン、緑を = = 0 xbb です青 0xcc を = です。 アルファ値が存在しない場合、色は非透過的です。 以下に、いくつかの例を示します。

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>アクセサリ

指定できる値は、レンダリング要素内に表示されるアクセサリの種類を決定します。

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


値が存在しない場合は、付属品が何も表示します。

 <a name="background" />


### <a name="background"></a>バックグラウンド

バック グラウンド プロパティは、セルの背景色を設定します。 値は、イメージへの URL (この場合、呼び出される非同期のイメージ ダウンローダーとイメージがダウンロードされると、背景が更新されます) または色の構文を使用して指定された色を指定できます。

 <a name="caption" />


### <a name="caption"></a>キャプション

レンダリング要素上に表示されるメインの文字列です。 フォントと色を設定してカスタマイズすることができます、`"textcolor"`と`"font"`プロパティです。 表示スタイルはによって決定されます、`"style"`プロパティです。

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>色および detailcolor

メイン テキストや詳細テキストに使用する色です。

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>detailfont とフォント

キャプションまたは詳細テキストに使用するフォントです。 フォントの仕様の形式は、必要に応じてダッシュと続くポイントのサイズ、フォント名です。
有効なフォントの仕様を次に示します。

-  「Helvetica」
-  「Helvetica 14」


 <a name="linebreak" />


### <a name="linebreak"></a>改行

行の分割を決定します。 次の値を指定できます。

-  `character-wrap`
-  `clip`
-  `head-truncation`
-  `middle-truncation`
-  `tail-truncation`
-  `word-wrap`


両方`character-wrap`と`word-wrap`と組み合わせて使用することができます、`"lines"`プロパティ要素内の複数行にレンダリング要素を有効にするのには 0 に設定します。

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>ontap と onaccessorytap

これらのプロパティは、パラメーターとしてオブジェクトを取得するアプリケーションで静的メソッド名をポイントする必要があります。 JsonDialog.FromFile または JsonDialog.FromJson メソッドを使用して、階層を作成するときに、省略可能なオブジェクトの値を渡すことができます。 このオブジェクトの値は、メソッドに渡されます。 これを使用するには、静的メソッドにいくつかのコンテキストを渡す。 例:

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines" />


### <a name="lines"></a>線

これは、0 に設定されている場合は、含まれる文字列の内容に応じた要素の自動サイズこととなります。 これを行うには、設定する必要も、`"linebreak"`プロパティを`"character-wrap"`または`"word-wrap"`です。

 <a name="style" />


### <a name="style"></a>style

スタイルは、コンテンツを表示するために使用されるセルのスタイルの種類を決定 UITableViewCellStyle 列挙値に対応しているとします。
次の値を指定できます。

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : サブタイトルを含むテキスト。


 <a name="subtitle" />


### <a name="subtitle"></a>サブタイトル

字幕を使用する値。 これは、スタイルを設定ショートカット キーは`"subtitle"`設定と、`"value"`プロパティを文字列にします。
これは、1 つのエントリがどちらもします。

 <a name="textcolor" />


### <a name="textcolor"></a>textcolor

テキストに使用する色です。

 <a name="value" />


### <a name="value"></a>value

レンダリング要素上に表示されるセカンダリ値です。 このレイアウトの影響を受ける、`"style"`設定します。 フォントと色を設定してカスタマイズすることができます、`"detailfont"`と`"detailcolor"`です。

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>ブール型の要素

ブール型の要素は、種類を設定する必要があります`"bool"`、含めることができます、`"caption"`を表示して、`"value"`に true または false に設定します。 場合、`"on"`と`"off"`プロパティが設定されて、イメージをしたと見なされます。 イメージは、アプリケーションの現在の作業ディレクトリに対して相対的に解決されます。 バンドルの相対ファイルを参照する場合は、使用、`"~"`アプリケーション バンドルのディレクトリを表すへのショートカットとして。 たとえば`"~/favorite.png"`バンドル ファイルに含まれている favorite.png になります。 例:

```csharp
{ 
    "type" : "boolean",
    "caption" : "Demo of a Boolean",
    "value"   : true
},

{
    "type": "boolean",
    "caption" : "Boolean using images",
    "value"   : false,
    "on"      : "favorite.png",
    "off"     : "~/favorited.png"
}
```

 <a name="type" />


### <a name="type"></a>型

種類はどちらかに設定できます`"boolean"`または`"checkbox"`です。 かどうかはブール値に設定、またはを使用して、UISlider イメージ (両方`"on"`と`"off"`設定されます)。 かどうかはチェック ボックスをオンに設定すると、これはチェック ボックスを使用します。 `"group"`プロパティは、特定のグループに属しているというブール型の要素のタグを使用できます。 含むルートもある場合に役立ちます。 これは、`"group"`ルートとしてのプロパティは、同じグループに属しているすべてのブール値 (または対応するチェック ボックス) の数を持つ結果を集計します。

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>Entry 要素

データを入力するのにユーザーを許可するのにには、入力要素を使用します。 Entry 要素の型はいずれかの`"entry"`または`"password"`です。 `"caption"`プロパティが、右側に表示するテキストに設定され、`"value"`エントリを設定する初期値に設定されています。 `"placeholder"` (表示されなかったり薄く) 空のエントリのユーザーにヒントを表示するために使用します。 次にいくつかの例を示します。

```csharp
{
        "type": "entry",
        "caption": "Username",
        "value": "",
        "placeholder": "Your account username"
}, {
        "type": "password",
        "caption": "Password",
        "value": "",
        "placeholder": "You password"
}, {
        "type": "entry",
        "caption": "Zip Code",
        "value": "01010",
        "placeholder": "your zip code",
        "keyboard": "numbers"
}, {
        "type": "entry",
        "return-key": "route",
        "caption": "Entry with 'route'",
        "placeholder": "captialization all + no corrections",
        "capitalization": "all",
        "autocorrect": "no"
}
```

 <a name="autocorrect" />


### <a name="autocorrect"></a>オート コレクト

エントリを使用する自動修正のスタイルを決定します。 使用可能な値は true または false (または文字列`"yes"`と`"no"`)。

 <a name="capitalization" />


### <a name="capitalization"></a>大文字使用

エントリの使用に大文字のスタイル。 次の値を指定できます。

-  `all`
-  `none`
-  `sentences`
-  `words`


 <a name="caption" />


### <a name="caption"></a>キャプション

キャプション、エントリに使用するには

 <a name="keyboard" />


### <a name="keyboard"></a>キーボード

データ エントリに使用するキーボードの種類。 次の値を指定できます。

-  `ascii`
-  `decimal`
-  `default`
-  `email`
-  `name`
-  `numbers`
-  `numbers-and-punctuation`
-  `twitter`
-  `url`


 <a name="placeholder" />


### <a name="placeholder"></a>プレース ホルダー

エントリが空の値を持つ場合に表示されるヒント テキストです。

 <a name="return-key" />


### <a name="return-key"></a>return-key

戻り値のキーを使用するラベル。 次の値を指定できます。

-  `default`
-  `done`
-  `emergencycall`
-  `go`
-  `google`
-  `join`
-  `next`
-  `route`
-  `search`
-  `send`
-  `yahoo`


 <a name="value" />


### <a name="value"></a>value

エントリの初期値

 <a name="Radio_Elements" />


## <a name="radio-elements"></a>ラジオ要素

ラジオ要素型である`"radio"`です。 選択されている項目を選択、`radioselected`そのコンテナーのルート要素のプロパティです。
さらの値が設定されている場合、`"group"`プロパティ、このオプション ボタンは、そのグループに属しています。

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>日付と時刻の要素

要素の型`"datetime"`、`"date"`と`"time"`時刻と日付、日付または時刻を表示するために使用します。 これらの要素は、キャプションと、値パラメーターとして取得します。 値は、.NET DateTime.Parse 関数でサポートされている任意の形式で記述できます。 例:

```csharp
"header": "Dates and Times",
"elements": [
        {
                "type": "datetime",
                "caption": "Date and Time",
                "value": "Sat, 01 Nov 2008 19:35:00 GMT"
        }, {
                "type": "date",
                "caption": "Date",
                "value": "10/10"
        }, {
                "type": "time",
                "caption": "Time",
                "value": "11:23"
                }                       
]
```

 <a name="Html/Web_Element" />


## <a name="htmlweb-element"></a>Html または Web 要素

作成、セルをタップされたときに、指定された URL の内容を表示する UIWebView を埋め込むを使用してローカルまたはリモート、`"html"`型です。 この要素の 2 つのプロパティは`"caption"`と`"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "http://tirania.org/blog" 
}
```
