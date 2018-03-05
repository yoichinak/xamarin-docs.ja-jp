---
title: "iOS のローカライズ"
description: "このドキュメントでは、iOS SDK のローカリゼーション機能および Xamarin とそれらにアクセスする方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: ea91dbcf7148651cb5d10acae4ada8bb6758c39e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="ios-localization"></a>iOS のローカライズ

_このドキュメントでは、iOS SDK のローカリゼーション機能および Xamarin とそれらにアクセスする方法について説明します。_

参照してください、[国際化エンコーディング](encodings.md)については Unicode 以外のデータを処理する必要があるアプリケーションで文字セットおよびコード ページが含まれます。

## <a name="ios-platform-features"></a>iOS プラットフォームの機能

このセクションでは、iOS のローカリゼーション機能の一部について説明します。 進んで、[次のセクション](#basics)に特定のコードと例を参照してください。

### <a name="language"></a>言語

ユーザーがその言語でを選択、**設定**アプリ。 この設定は、言語識別文字列と言語設定を検出するアプリケーションや、オペレーティング システムが表示される画像に影響します。

これは、英語、スペイン語、日本語、フランス語、またはそれぞれのアプリに表示されるその他の言語を表示するかどうかをユーザーが決定されます。

実際の iOS 7 でサポートされる言語の一覧: 英語 (米国)、英語 (英国)、中国語 (簡体字)、中国語 (繁体字)、フランス語、ドイツ語、イタリア語、日本語、韓国語、スペイン語、アラビア語、カタロニア語、クロアチア語、チェコ語、デンマーク語、オランダ語、フィンランド語、ギリシャ語、ヘブライ語ハンガリー語、インドネシア語、マレー語、ノルウェー語、ポーランド語、ポルトガル語、ポルトガル語 (ブラジル)、ルーマニア語、ロシア語、スロバキア語、スウェーデン語、タイ語、トルコ語、ウクライナ語、ベトナム語、英語 (オーストラリア)、スペイン語 (メキシコ)。

現在の言語の最初の要素へのアクセスが照会できる、`PreferredLanguages`配列。

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

この値は、言語コードをなどにある`en`英語、 `es` 、スペイン語用`ja`日本語などです。_返される値は、(最適な一致を判断するにはフォールバック規則を使用して) アプリケーションでサポートされているローカライズ版のいずれかに限定されます。_

アプリケーション コードは常にする必要はありません Xamarin この値を確認し、両方の iOS が自動的にユーザーの言語の正しい文字列またはリソースを提供するのに役立つ機能を提供します。 これらの機能は、このドキュメントの残りの部分で説明します。

> [!NOTE]
> **注:** 、推奨されるコードが iOS 9 の場合は、前に`var lang = NSLocale.PreferredLanguages [0];`です。
>
> Ios 9 - 変更されたこのコードによって返される結果を参照してください[テクニカル ノート TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html)詳細についてはします。
>
> 使用を続行できます`NSLocale.PreferredLanguages [0]`ユーザーが選択されている実際の値を確認する (、ローカライズ版に関係なく、アプリでサポート)。

### <a name="locale"></a>ロケール

ユーザーの選択 で、ロケール、**設定**アプリ。 この設定では、日付、時刻、数字、通貨がフォーマットされている方法に影響します。

これにより、ユーザーは、小数点区切り文字が、コンマまたはポイント、および日、月および年の日付の表示の順序であるかどうかは、12 時間、または 24 時間制の時刻書式を参照するかどうかを選択します。

Xamarin を使用した両方の Apple の iOS クラスへのアクセスがある (`NSNumberFormatter`) System.Globalization で .NET クラスとします。 各で使用できるさまざまな機能があると、これは、自分のニーズに適していますが、開発者に評価してください。 具体的には、取得と StoreKit を使用してアプリ購入の価格を表示している場合を必ず使用して Apple の書式指定クラスの価格情報が返されます。

現在のロケールは、いずれかでクエリできます。

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

最初の値は、オペレーティング システムをキャッシュすることができます、したがって常に反映していないこと、ユーザーの現在選択されているロケール。 2 番目の値を使用すると、現在選択されているロケールを取得できます。


### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS の生成、`NSCurrentLocaleDidChangeNotification`ユーザーがそのユーザーのロケールを更新したとき。 アプリケーションは、それらが実行され、UI に適切な変更を加えるときに、この通知のリッスンできます。

<a name="basics" />

## <a name="localization-basics-in-ios"></a>IOS でのローカライズの基本事項

IOS の次の機能は、ユーザーに表示するローカライズされたリソースを提供する Xamarin で簡単に利用されます。 参照してください、 [TaskyL10n サンプル](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)これらのアイデアを実装する方法を確認します。

### <a name="infoplist"></a>Info.plist

作業を開始する前に、構成、 **Info.plist**に次のキー ファイル。

- `CFBundleDevelopmentRegion` -アプリケーション (通常、言語、開発者が読み上げるし、ストーリー ボードや文字列とイメージ リソースを使用) の既定の言語です。 次の例で**en** (英語) を指定します。
- `CFBundleLocalizations` -配列のような言語コードを使用しても、アプリケーションでサポートされているその他のローカライズ版の**es** (スペイン語) と**PT-PT** (ポルトガル ポルトガルで話される)。

```xml
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
<key>CFBundleLocalizations</key>
<array>
  <string>de</string>
  <string>es</string>
  <string>ja</string>
  ...
</array>
```

### <a name="localizedstring-method"></a>LocalizedString メソッド

`NSBundle.MainBundle.LocalizedString`メソッドに格納されているローカライズされたテキストを検索**.strings**プロジェクト内のファイルです。 これらのファイルごとに作成された特別に指定されたディレクトリに、言語、 **.lproj**サフィックス。

#### <a name="strings-file-locations"></a>.strings ファイルの場所

- **Base.lproj**既定の言語のリソースを含むディレクトリです。
  プロジェクトのルートにある多くの場合、(にも配置できますが、**リソース**フォルダー)。
- **<language>.lproj**ディレクトリに作成されます、サポートされている各言語の通常の**リソース**フォルダーです。

別の数があります**.strings**各言語ディレクトリ内のファイル。

- **Localizable.strings**のローカライズされたテキストのメインのリスト。
- **InfoPlist.strings** - などに、アプリケーション名を変換する特定のキーがこのファイルで許可されていることを特定します。
- **< ストーリー ボードの名前 > .strings** -をストーリー ボードのユーザー インターフェイス要素の翻訳を含む省略可能なファイルです。

**ビルド アクション**をこれらのファイルにする必要があります**バンドル リソース**です。

#### <a name="strings-file-format"></a>.strings ファイル形式

ローカライズされた文字列値の構文です。

```csharp
/* comment */
"key"="localized-value";
```

文字列では、次の文字をエスケープする必要があります。

* `\"`  引用符
* `\\`  円記号
* `\n`  改行

これは、例では**es/Localizable.strings** (ie です。次のサンプルのスペイン語) ファイル

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="images"></a>イメージ

IOS のイメージをローカライズするには。

1. たとえば、コードでは、イメージを参照してください。

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. 既定のイメージ ファイルを配置**flag.png**で**Base.lproj** (ネイティブな開発言語ディレクトリ)。

3. 内のイメージのローカライズされたバージョンを必要に応じて配置**.lproj** (各言語のフォルダー **es.lproj**, **ja.lproj**). 同じファイル名を使用して**flag.png**各言語ディレクトリにします。

イメージが特定の言語の存在しない場合は、iOS は既定のネイティブ言語フォルダーに戻るし、そこからイメージを読み込みます。


#### <a name="launch-images"></a>イメージを起動します。

起動イメージ (XIB や iPhone 6 モデルのストーリー ボード) の名前付け規則を使用してそれらを配置するとき、 **.lproj**各言語用のディレクトリ。

```csharp
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>アプリ名

配置すること、 **InfoPlist.strings**ファイルで、 **.lproj**ディレクトリでは、アプリからいくつかの値をオーバーライドできます。 **Info.plist**、アプリケーション名を含みます。

```csharp
"CFBundleDisplayName" = "LeónTodo";
```

その他のキーを使用できる[アプリケーション固有の文字列をローカライズ](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21)は。

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright


<!--
## App icon

Does not seem to be possible (although it definitely used to be!).
-->

### <a name="localization-native-development-region"></a>ローカリゼーション ネイティブ開発領域

既定の文字列 (内にある、 **Base.lproj**フォルダー) は 'フォールバック' の言語であると想定されます。 つまり、翻訳は、コードで要求し、が見つからない場合、現在の言語、 **Base.lproj**フォルダーを使用する既定の文字列が検索されます (一致するものが見つからない場合、翻訳識別子文字列自体が表示されます)。

開発者は plist キーを設定して、フォールバックを有効にする別の言語を選択できます`CFBundleDevelopmentRegionKey`です。 ネイティブな開発言語の言語コードには、その値を設定する必要があります。 このスクリーン ショットは、Xamarin Studio のネイティブ開発領域とで plist がスペイン語 (es) に設定を示しています。

![](images/cfbundledevelopmentregion.png "InfoPList.strings property")

ストーリー ボードでは、コード全体で使用される既定の言語が英語でない場合は、アプリのコード全体で使用されているネイティブ言語を反映するようにこの値を設定します。

### <a name="dates-and-times"></a>日付と時刻

.NET の日付と時刻の関数は、組み込みを使用することはできますが (現在と共に`CultureInfo`) ロケールの日付と時刻の書式を設定するこれはロケール固有ユーザー-の設定が無視 (これは、言語を個別に設定することができます)。

IOS を使用して`NSDateFormatter`ユーザーのロケールの設定と一致する出力を生成します。 次のサンプル コードでは、基本的な日付と時刻の書式設定オプションを示しています。

```csharp
var date = NSDate.Now;
var df = new NSDateFormatter ();
df.DateStyle = NSDateFormatterStyle.Full;
df.TimeStyle = NSDateFormatterStyle.Long;
Debug.WriteLine ("Full,Long: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Short;
df.TimeStyle = NSDateFormatterStyle.Short;
Debug.WriteLine ("Short,Short: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Medium;
df.TimeStyle = NSDateFormatterStyle.None;
Debug.WriteLine ("Medium,None: " + df.StringFor(date));
```

米国の英語の結果:

```csharp
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

スペインのスペイン語の場合の結果:

```csharp
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Apple を参照してください[日付フォーマッタ](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html)詳細についてはドキュメントです。 ロケール依存型の日付と時刻の書式設定をテストするときに両方を確認**iPhone 言語**と**地域**設定します。

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>右から左 (RTL) のレイアウト

iOS では、いくつかの右から左に対応するアプリの構築を支援する機能を提供します。

* 使用して**自動レイアウト**`leading`と`trailing`コントロール揃えの属性 (に対応する*左*と*右*英語、たとえばが元に戻す RTL 言語用)。
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md)コントロールが右から左に対応するコントロールをレイアウトするために特に便利です。
* 使用して`TextAlignment = UITextAlignment.Natural`テキストの配置の (されます*左*多くの言語が、*右*右から左への)。
* `UINavigationController` 自動的に [戻る] ボタンを反転し、方向にスワイプ方向を反転させます。

次のスクリーン ショットに示さ、[ローカライズ**Tasky**サンプル](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)アラビア語やヘブライ語で (ただし、英語は、フィールドに入力された)。

[ ![](images/rtl-ar-sml.png "アラビア語のローカライズ")](images/rtl-ar.png "Arabic") 

[ ![](images/rtl-he-sml.png "ヘブライ語のローカライズ")](images/rtl-he.png "Hebrew")

iOS を自動的に反転、 `UINavigationController`、内の他のコントロールを配置していると`UIStackView`するか、自動レイアウトに配置します。
使用して右から左へテキストをローカライズ**.strings** LTR テキストと同じ方法でファイル。


<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>コードで UI をローカライズします。

[(コードのローカライズ版) Tasky](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)サンプルは、ユーザー インターフェイスがコードではなく XIBs (ストーリー ボードなど) で構成されているアプリケーションをローカライズする方法を示します。

### <a name="project-structure"></a>プロジェクトの構造



![](images/solution-code.png "リソースのツリー")

### <a name="localizablestrings-file"></a>Localizable.strings ファイル

上記の説明に従って、 **Localizable.strings**ファイル形式は、ここで、キーは、ユーザーが選択した文字列を示す、キーと値のペアで構成されています

スペイン語 (**es**) サンプルのローカライズ版を以下に示します。

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="performing-the-localization"></a>ローカライズを実行します。

アプリケーション コードで (ラベルのテキストまたは入力のプレース ホルダーなどである) かどうかをユーザー インターフェイスの表示テキストが設定されている限り、コードを使用して、iOS`LocalizedString`を表示する適切な翻訳を取得します。

```csharp
var localizedString = NSBundle.MainBundle.LocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>ストーリー ボード Ui のローカライズ

サンプル[Tasky (ローカライズされたストーリー ボード)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)ストーリー ボード内のコントロールのテキストをローカライズする方法を示しています。

### <a name="project-structure"></a>プロジェクトの構造

**Base.lproj**ディレクトリ、ストーリー ボードを含み、アプリケーションで使用するイメージを含める必要があります。

その他の言語ディレクトリが含まれて、 **Localizable.strings**コードでは、参照されている任意の文字列リソースのファイルと同様に、 **MainStoryboard.strings**内のテキストの翻訳を格納しているファイル、ストーリー ボードです。

![](images/solution-storyboard.png "リソースのツリー")

言語ディレクトリ内の 1 つを上書きする、ローカライズされているすべてのイメージのコピーを含める必要があります**Base.lproj**です。

### <a name="object-id--localization-id"></a>オブジェクト ID/ローカリゼーション ID

を作成し、ストーリー ボードでのコントロールの編集時に各コントロールを選択し、ローカリゼーションに使用する ID を確認します。

* Visual Studio for Mac がプロパティ パッドにあると呼ばれる**ローカリゼーション ID**です。
* Xcode で呼び出されます**オブジェクト ID**です。

多くの場合の形式を持つように文字列値では**"NF3 h8 xmR"**:

![](images/xs-designer-localization-id.png "ストーリー ボード ローカリゼーションの Xcode ビュー")

この値が使用される、 **.strings**ファイルを各コントロールに翻訳済みテキストを自動的に割り当てます。

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

ストーリー ボードの翻訳ファイルの形式は次のような**Localizable.strings**ファイル キー (左側の値) がユーザー定義することはできませんが、代わりに、非常に特定の形式でなければなりません:`ObjectID.property`です。

例では**Mainstoryboard.strings**以下`UITextField`s が、`placeholder`ローカライズ可能なテキスト プロパティ`UILabel`s が、`text`プロパティおよび`UIButton`s 既定のテキストは、使用して設定される`normalTitle`:

```csharp
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> **クラスのサイズとストーリー ボードを使って**翻訳が表示されない場合があります。 これに関連する可能性があります[この問題](http://stackoverflow.com/questions/24989208/xcode-6-does-not-localize-interface-builder)Apple のドキュメントが表示: ストーリー ボードまたは XIB はローカライズしない正しくのすべての次の 3 つの条件に該当する場合のローカライズ: ストーリー ボードまたは XIB サイズ クラスを使用します。 基本のローカリゼーションおよび build ターゲットは、世界に設定されます。 ビルドは、iOS 7.0 以降をターゲットです。
修正プログラムは、次の 2 つのファイルが同一にストーリー ボードの文字列は、ファイルが重複して: **MainStoryboard~iphone.strings**と**MainStoryboard~ipad.strings**:

> ![](images/xs-dup-strings.png "文字列のファイル")


<!--
# Native Formatting

Xamarin.iOS applications can take advantage of the .NET framework's formatting options for localizing
numbers and dates.

### Date string formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html#//apple_ref/doc/uid/TP40002369-SW1


NSDateFormatter formatter = new NSDateFormatter ();
formatter.DateFormat = "MMMM/dd/yyyy";
NSString dateString = new NSString (formatter.ToString (d));


### Number formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfNumberFormatting10_4.html#//apple_ref/doc/uid/TP40002368-SW1

-->

<a name="appstore" />

## <a name="app-store-listing"></a>アプリ ストアの一覧

Apple のよく寄せられる質問に依存して[アプリ ストアのローカリゼーション](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization)で販売アプリは、各国の翻訳を入力します。 翻訳は、アプリも含まれているローカライズされた場合のみ表示されます。 それらの警告に注意してください**.lproj**言語のディレクトリ。

<!--

Once you’ve entered your application into iTunes Connect the default language
metadata and screenshots will appear as shown:

![]( "itunes connect 1")

Use the language list on the right to select other languages to provide
translated application name, description, search keywords, URLs and screenshots.
The complete list of languages is shown in this screenshot:

![]( "itunes connect 2")
-->

## <a name="summary"></a>まとめ

この記事では、組み込みのリソースの処理とストーリー ボードの機能を使用する iOS アプリケーションのローカライズの基本について説明します。

できますで学習する i18n および L10n の詳細 (Xamarin.Forms を含む)、iOS、Android、およびクロスプラット フォームのアプリの[このクロスプラット フォーム ガイド](~/cross-platform/app-fundamentals/localization.md)です。


## <a name="related-links"></a>関連リンク

- [(コードのローカライズ版) Tasky (サンプル)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (ローカライズされたストーリー ボード) (サンプル)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple のガイドでのローカライズ](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [クロスプラット フォームのローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms ローカリゼーション](~/xamarin-forms/app-fundamentals/localization.md)
- [Android のローカライズ](~/android/app-fundamentals/localization.md)
