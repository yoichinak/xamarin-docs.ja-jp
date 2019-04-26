---
title: MonoTouch.Dialog Json マークアップ
description: このドキュメントでは、MonoTouch.Dialog を使用して Xamarin.iOS ユーザー インターフェイスを構築するために使用する JSON 構文について説明します。
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 2bd45c5482a8f0367bffa21f301bb631c3429a21
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61395154"
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json マークアップ

このページの説明の MonoTouch.Dialog で受け入れられる Json マークアップ[JsonElement](xref:MonoTouch.Dialog.JsonElement)

例を使用して開始しましょう。 次は JsonElement に渡すことができる完全な Json ファイルです。

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

上記のマークアップでは、次の UI が生成されます。

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "特定のマークアップで作成される UI")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

ツリー内のすべての要素は、プロパティを含めることができます`"id"`します。 個々 のセクションまたは JsonElement インデクサーを使用して要素を参照する実行時にことができます。 以下に例を示します。

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


ルート要素は、セクション内で入れ子になったコント ローラーを作成する要素として表示できます。 その場合、追加のプロパティで`"type"`に設定する必要があります `"root"`

 <a name="url" />


### <a name="url"></a>url

場合、`"url"`プロパティを設定する場合はこの RootElement をタップ、コード ファイルを指定した url から要求は、内容、新しい情報が表示されます。 作成に使用することができます、ユーザーがタップに基づいて、サーバーからユーザー インターフェイスを拡張します。

 <a name="group" />


### <a name="group"></a>グループ

かどうか設定すると、この設定のルート要素のグループ名。 グループの名前は、要素の入れ子になった要素の 1 つのルート要素の値として表示される概要の選択に使用されます。 これは、チェック ボックスの値またはラジオ ボタンの値のいずれか。

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

入れ子になった要素で選択されているオプション項目を識別します。

 <a name="title" />


### <a name="title"></a>タイトル

存在する場合、RootElement を使用するタイトルがあります。

 <a name="type" />


### <a name="type"></a>種類

設定する必要があります`"root"`とき (これは入れ子にコント ローラーを使用) セクションに表示されます。

 <a name="sections" />


### <a name="sections"></a>sections

これは、個々 のセクションでは、Json 配列

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>セクションの構文

セクションが含まれます。

-  `header` (省略可能)
-  `footer` (省略可能)
-  `elements` 配列


 <a name="header" />


### <a name="header"></a>ヘッダー

存在する場合、ヘッダーのテキストが、セクションのキャプションとして表示されます。

 <a name="footer" />


### <a name="footer"></a>フッター

存在する場合は、セクションの下部にフッターが表示されます。

 <a name="elements" />


### <a name="elements"></a>要素

これは、要素の配列です。 各要素が少なくとも 1 つのキーを含める必要があります、`"type"`を作成する要素の種類を識別するために使用されるキー。
一部の要素のようないくつかの一般的なプロパティを共有`"caption"`と`"value"`します。 サポートされている要素の一覧を次に示します。

-  `string` (どちらも、スタイル設定なしで) 要素
-  `entry` 行 (標準またはパスワード)
-  `boolean` 値 (スイッチまたはイメージを使用)


文字列の要素として使用できますボタン メソッドを提供することで、セルと、アクセサリのいずれかで、ユーザーがタップしたときに呼び出される

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>レンダリング要素

レンダリング要素がに基づいて、 C# StringElement と StyledStringElement およびそれらがさまざまな方法で情報を表示し、さまざまな方法でそれらをレンダリングすることができます。 最も単純な要素は、次のように作成できます。

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

これで、既定値のすべての単純な文字列が表示されます。 フォント、背景、テキストの色と飾り。 これらの要素への操作をフックし、設定してボタンのように動作できるようにすることは、`"ontap"`プロパティまたは`"onaccessorytap"`プロパティ。

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

上記はクラス"Acme.PhotoLibrary"の"ShowPhotos"メソッドを呼び出します。 `"onaccessorytap"`も同様ですが、ユーザーがセルをタップする代わりのアクセサリをタップした場合のみ呼び出されます。 これを有効にするには、アクセサリを設定することも必要があります。

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

要素をレンダリングできる 2 つの文字列を一度に表示、1 つは、キャプション、およびもう 1 つは、値。 これらの文字列がレンダリングされる方法スタイルに依存して設定できる、これを使用して、`"style"`プロパティ。 既定で、左側と右側の値、キャプションが表示されます。 詳細については、スタイル セクションを参照してください。 色は、'#' 記号の後に赤、緑、青、おそらくアルファ値の値を表す 16 進数でエンコードされます。 RGB または RGBA のいずれかの値を表す短い形式 (3 または 4 16 進数です) では、内容をエンコードすることができます。 または RGB または RGBA のいずれかの値を表す long フォーム (6 か月または 8 桁) です。 短いバージョンは、同じの 16 進数字を 2 回記述する短縮形です。 "#1bc"定数が red としてなどでは、パターン、緑の = = 0 xbb です青 = 0 xcc します。 アルファ値が存在しない場合はカラーが不透明にします。 以下に、いくつかの例を示します。

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>アクセサリ

指定できる値は、表示要素内に表示されるアクセサリの種類を決定します。

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


値が存在しない場合は、付属品は表示されません。

 <a name="background" />


### <a name="background"></a>バックグラウンド

背景のプロパティは、セルの背景色を設定します。 値がイメージへの URL (ここでは、非同期のイメージのダウンローダーが呼び出され、イメージがダウンロードされると、バック グラウンドが更新されます) か、または色の構文を使用して指定された色があることができます。

 <a name="caption" />


### <a name="caption"></a>キャプション

レンダリング要素上に表示される主な文字列。 フォントと色を設定してカスタマイズできます、`"textcolor"`と`"font"`プロパティ。 描画スタイルが続く、`"style"`プロパティ。

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>色と detailcolor

メイン テキストや詳細テキストに使用する色です。

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>detailfont とフォント

キャプションまたは詳細なテキストに使用するフォントです。 フォントの仕様の形式は、必要に応じてその後にダッシュとポイントのサイズ、フォント名です。
次に、有効なフォントの仕様を示します。

-  「Helvetica」
-  「Helvetica 14」


 <a name="linebreak" />


### <a name="linebreak"></a>改行

行を分割する方法を決定します。 次の値を指定できます。

-  `character-wrap`
-  `clip`
-  `head-truncation`
-  `middle-truncation`
-  `tail-truncation`
-  `word-wrap`


両方`character-wrap`と`word-wrap`と組み合わせて使用することができます、`"lines"`プロパティが複数行の要素にレンダリング要素を有効にする 0 に設定します。

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>ontap と onaccessorytap

これらのプロパティは、パラメーターとしてオブジェクトを取得するアプリケーションで静的メソッド名をポイントする必要があります。 JsonDialog.FromFile または JsonDialog.FromJson メソッドを使用して、階層を作成するときに、省略可能なオブジェクトの値を渡すことができます。 このオブジェクトの値は、メソッドに渡されます。 これを使用するには、静的メソッドにいくつかのコンテキストを渡す。 例えば:

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

これは、0 に設定されている場合、要素の自動サイズに含まれている文字列の内容に応じたがなります。 これを機能させるには、設定する必要も、`"linebreak"`プロパティを`"character-wrap"`または`"word-wrap"`します。

 <a name="style" />


### <a name="style"></a>style

スタイルは、コンテンツを表示するために使用されるセル スタイルの種類を決定 UITableViewCellStyle 列挙値に対応しています。
次の値を指定できます。

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : サブタイトルのテキスト。


 <a name="subtitle" />


### <a name="subtitle"></a>サブタイトル

字幕に使用する値。 これは、ショートカット スタイルを設定する`"subtitle"`設定と、`"value"`プロパティを文字列にします。
これは、1 つのエントリがどちらもします。

 <a name="textcolor" />


### <a name="textcolor"></a>textcolor

テキストに使用する色です。

 <a name="value" />


### <a name="value"></a>値

レンダリング要素上に表示されるセカンダリの値。 このレイアウトを受ける、`"style"`設定します。 フォントと色を設定してカスタマイズできます、`"detailfont"`と`"detailcolor"`します。

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>ブール型要素

ブール型要素に型を設定する必要があります`"bool"`、含めることができます、`"caption"`を表示して、 `"value"` true または false に設定されます。 場合、`"on"`と`"off"`プロパティを設定、イメージをしたと見なされます。 イメージは、アプリケーションでは、現在の作業ディレクトリを基準として解決されます。 バンドルの相対ファイルを参照する場合は、使用、`"~"`アプリケーション バンドル ディレクトリ表してへのショートカットとして。 たとえば`"~/favorite.png"`バンドル ファイルに含まれる favorite.png になります。 例:

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


### <a name="type"></a>種類

種類はいずれかに設定できます`"boolean"`または`"checkbox"`します。 かどうかはブール値に設定はまたはを使用して、UISlider イメージ (両方`"on"`と`"off"`設定されます)。 かどうかはチェック ボックスをオンに設定すると、それはチェック ボックスを使用します。 `"group"`プロパティは、特定のグループに属するものとしてブール型の要素のタグを使用できます。 コンテナーのルートもある場合に役立ちます。 これは、`"group"`ルートとしてのプロパティは、同じグループに属しているすべてのブール値 (またはチェック ボックス) の数は、結果を集計します。

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>エントリ要素

データを入力するのにユーザーを許可するのにには、エントリの要素を使用します。 エントリの要素の型は`"entry"`または`"password"`します。 `"caption"`プロパティは、右側に表示するテキストに設定し、`"value"`エントリを設定する初期値に設定されています。 `"placeholder"` (灰色に表示されます) 空のエントリをユーザーにヒントを表示するために使用します。 次にいくつかの例を示します。

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


### <a name="autocorrect"></a>オート コレクトする

エントリを使用する自動修正のスタイルを決定します。 使用可能な値は true または false (または文字列`"yes"`と`"no"`)。

 <a name="capitalization" />


### <a name="capitalization"></a>大文字使用

大文字のスタイル、エントリに使用します。 次の値を指定できます。

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

エントリに空の値がある場合に表示されるヒント テキスト。

 <a name="return-key" />


### <a name="return-key"></a>return キー

戻り値のキーに使用するラベル。 次の値を指定できます。

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


### <a name="value"></a>値

エントリの初期値

 <a name="Radio_Elements" />


## <a name="radio-elements"></a>ラジオ要素

ラジオ要素型である`"radio"`します。 選択されている項目を選択、`radioselected`そのコンテナーのルート要素プロパティ。
さらの値が設定されている場合、`"group"`プロパティ、そのグループに属するをこのラジオ ボタンをクリックします。

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>日付と時刻の要素

要素の型`"datetime"`、`"date"`と`"time"`時刻と日付、日付または時刻を表示するために使用します。 これらの要素は、キャプションと値をパラメーターとして取得します。 値は、.NET DateTime.Parse 関数でサポートされている任意の形式で記述できます。 例:

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

セルを作成できますがタップされたときに、指定した URL のコンテンツをレンダリングする UIWebView の埋め込みがローカルまたはリモートのいずれかを使用して、`"html"`型。 この要素の 2 つのプロパティは`"caption"`と`"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "https://tirania.org/blog" 
}
```
