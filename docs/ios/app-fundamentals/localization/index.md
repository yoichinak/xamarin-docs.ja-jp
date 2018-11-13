---
title: Xamarin.iOS でのローカライズ
description: このドキュメントでは、iOS のローカリゼーションの機能と Xamarin.iOS アプリでこれらの機能を使用する方法について説明します。 これは、言語、ロケール、文字列のファイル、起動イメージ、および詳細について説明します。
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/28/2017
ms.openlocfilehash: 906489aa3947df24662cbbd0473333caccc032c7
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527262"
---
# <a name="localization-in-xamarinios"></a>Xamarin.iOS でのローカライズ

_このドキュメントでは、iOS SDK のローカライズ機能と Xamarin を使用してアクセスする方法について説明します。_

参照してください、[国際化エンコーディング](encodings.md)手順については、Unicode 以外のデータを処理する必要があるアプリケーションでの文字セット/コード ページのようにします。

## <a name="ios-platform-features"></a>iOS プラットフォーム機能

このセクションでは、iOS のローカライズ機能の一部について説明します。 スキップする、[次のセクション](#basics)に固有のコードと例を参照してください。

### <a name="language"></a>言語

ユーザーの言語での選択、**設定**アプリ。 この設定は、言語識別文字列と、オペレーティング システムおよびアプリで表示されるイメージに影響します。 

アプリで使用されている言語を判断するには、最初の要素を取得`NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

この値は言語コードをなどにある`en`英語を`es`、スペイン語用`ja`日本語など。返される値は、(最適な一致を判断するにはフォールバック規則を使用して) アプリケーションでサポートされているローカライズ版のいずれかに限定されます。

アプリケーション コードは常に Xamarin – この値を確認する必要がないと、iOS の両方が自動的にユーザーの言語の正しい文字列またはリソースを提供するのに役立つ機能を提供します。 これらの機能は、このドキュメントの残りの部分で説明します。

> [!NOTE]
> 使用`NSLocale.PreferredLanguages`をアプリでサポートされているローカライズ版に関係なく、ユーザーの言語設定を確認します。 IOS 9; に変更された、このメソッドによって返される値参照してください[テクニカル ノート TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html)詳細についてはします。

### <a name="locale"></a>ロケール

ユーザーのロケールでの選択、**設定**アプリ。 この設定では、日付、時刻、数字、および通貨がフォーマットされている方法に影響します。

これにより、ユーザーが、小数点区切り文字がコンマまたはポイント、および日、月、日付の表示の年の順序であるかどうか、12 時間制または 24 時間制の時刻書式を参照するかどうかを選択します。

Xamarin を使用した両方の Apple の iOS クラスへのアクセスがある (`NSNumberFormatter`) の .NET クラスと同様です。 それぞれで使用できるさまざまな機能が、これは、自分のニーズに適していますが、開発者に評価してください。 具体的には、取得して StoreKit を使用してアプリ内購入価格を表示する場合は、返される価格情報を Apple の書式設定のクラスを使用する必要があります。

現在のロケールは、2 つの方法のいずれかでクエリできます。

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

最初の値は、オペレーティング システムでキャッシュできる、したがって常に反映していないこと、ユーザーの現在選択されているロケール。 2 番目の値を使用すると、現在選択されているロケールを取得できます。

> [!NOTE]
> Mono (Xamarin.iOS の基になる .NET ランタイム)、Apple の iOS Api はまったく同じ言語/地域の組み合わせのセットをサポートしていません。
> このため、iOS で言語/地域の組み合わせを選択することは**設定**Mono で有効な値にマップされていないアプリです。 たとえば、iPhone の言語を英語にし、そのリージョンをスペインに設定には、異なる値を生成する次の Api が発生します。
> 
> - `CurrentThead.CurrentCulture`: EN-US (Mono API)
> - `CurrentThread.CurrentUICulture`: EN-US (Mono API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (Apple API)
>
> Mono を使用しているため`CurrentThread.CurrentUICulture`リソースを選択して`CurrentThread.CurrentCulture`日付および通貨の書式を設定する (たとえば、.resx ファイル) のローカリゼーションの Mono ベースとしてはこれらの言語/地域の組み合わせの結果を予想します。 このような場合に、必要に応じてをローカライズする Apple の Api に依存します。

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS の生成、`NSCurrentLocaleDidChangeNotification`ユーザーがそのロケールを更新したとき。 アプリケーションは、それらを実行しているし、UI に適切な変更を加えるときにこの通知をリッスンできます。

## <a name="localization-basics-in-ios"></a>IOS でのローカライズの基本事項

IOS の次の機能は、ユーザーに表示するローカライズされたリソースを提供する Xamarin で簡単に活用されます。 参照してください、 [TaskyL10n サンプル](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)にこれらのアイデアを実装する方法を参照してください。

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>既定値を指定して、Info.plist でサポートされる言語

[テクニカル Q & A QA1828: iOS でのアプリの言語の決定方法](https://developer.apple.com/library/content/qa/qa1828/_index.html)Apple は iOS アプリで使用する言語を選択する方法について説明します。 次の要因に影響する言語が表示されます。

- ユーザーの言語を優先 (で見つかった、**設定**アプリ)
- ローカライズ版のアプリ (.lproj フォルダー) にバンドルされています
- `CFBundleDevelopmentRegion` (**Info.plist**アプリの既定の言語を指定する値)
- `CFBundleLocalizations` (**Info.plist**配列はサポートされているすべてのローカライズ版を指定)

技術的な Q & A に記載されている`CFBundleDevelopmentRegion`アプリの既定の地域と言語を表します。 アプリがユーザーの好みの言語のいずれかを明示的にサポートは、このフィールドで指定された言語が使用されます。 

> [!IMPORTANT]
> iOS 11 では、この言語の選択方法が、オペレーティング システムの以前のバージョンよりも厳密に適用されます。 このため、そのサポートされているローカライズ – .lproj フォルダーを含むかの値の設定のいずれかを明示的に宣言しないすべての iOS 11 アプリ`CFBundleLocalizations`– ios 10 と比べて、iOS 11 で別の言語を表示可能性があります。

場合`CFBundleDevelopmentRegion`がで指定されていない、 **Info.plist**ファイル、Xamarin.iOS ビルド ツールは現在の既定値を使用`en_US`します。 これは、将来のリリースで変更が、既定の言語は英語ことを意味します。

アプリが、予想される言語を選択することを確認するには、以下の手順を実行します。

- 既定の言語を指定します。 開いている**Info.plist**を使用して、**ソース**の値を設定するビュー、`CFBundleDevelopmentRegion`キー; XML では、次のような場合がなります。

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

この例では、優先なしのユーザーの言語がサポートされますを指定するには、スペイン語に既定の"es"を使用します。

- サポートされているすべてのローカライズを宣言します。 **Info.plist**を使用して、**ソース**の配列を設定するビュー、`CFBundleLocalizations`キー; XML では、次のような場合がなります。

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Xamarin.iOS アプリを .resx ファイルは、これらを指定する必要がありますなど、.NET のメカニズムを使用してローカライズした**Info.plist**値も同様です。

これらの詳細については**Info.plist**を Apple のキーで見て[情報プロパティ リスト キー参照](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html)します。

### <a name="getlocalizedstring-method"></a>GetLocalizedString メソッド

`NSBundle.MainBundle.GetLocalizedString`メソッドに格納されているローカライズされたテキストを検索 **.strings**プロジェクト内のファイル。 これらのファイルは、特別に指定されたディレクトリでの言語で分類された、 **.lproj**サフィックス (拡張機能の最初の文字が小文字の"L"に注意してください)。

#### <a name="strings-file-locations"></a>.strings ファイルの場所

- **Base.lproj**は既定の言語のリソースが含まれるディレクトリです。
  プロジェクトのルートにある多くの場合、(にも配置できますが、**リソース**フォルダー)。
- **&lt;言語&gt;.lproj**ディレクトリは、サポートされている各言語の通常で作成された、**リソース**フォルダー。

さまざまなできる **.strings**各言語ディレクトリ内のファイル。

- **Localizable.strings** : ローカライズされたテキストのメインのリスト。
- **InfoPlist.strings** – 特定のアプリケーション名などを変換する特定のキーがこのファイルで許可されています。
- **< のストーリー ボード名 > .strings** – 省略可能なファイルのストーリー ボードでのユーザー インターフェイス要素の翻訳が含まれています。

**ビルド アクション**これらのファイルを指定する必要があります**バンドル リソース**します。

#### <a name="strings-file-format"></a>.strings ファイルの形式

ローカライズされた文字列値の構文です。

```console
/* comment */
"key"="localized-value";
```

文字列では、次の文字をエスケープする必要があります。

* `\"` 見積もり
* `\\` 円記号
* `\n` 改行

これは、例では**es/Localizable.strings** (ie します。サンプルからのファイルをスペイン語):

```console
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

IOS でイメージをローカライズするには。

1. たとえば、コードでは、イメージを参照してください。

    ```csharp
    UIImage.FromBundle("flag");
    ```

2. 既定のイメージ ファイルを配置**flag.png**で**Base.lproj** (ネイティブ開発言語ディレクトリ)。

3. 内のイメージのローカライズされたバージョンを必要に応じて配置 **.lproj** (例: 各言語のフォルダー **es.lproj**、 **ja.lproj**)。 同じファイル名を使用して、 **flag.png**各言語ディレクトリにします。

イメージが特定の言語の存在しない場合は、iOS が既定のネイティブ言語フォルダーに戻るし、そこからイメージを読み込みます。

#### <a name="launch-images"></a>起動画像

起動イメージ (と XIB またはストーリー ボードの iPhone 6 モデル) の名前付け規則を使用してそれらを配置するときに、 **.lproj**各言語用のディレクトリ。

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>アプリ名

配置すること、 **InfoPlist.strings**ファイル、 **.lproj**ディレクトリでは、アプリのいくつかの値をオーバーライドできます。 **Info.plist**、アプリケーション名を含みます。

```console
"CFBundleDisplayName" = "LeónTodo";
```

その他のキーを使用できる[アプリケーション固有の文字列をローカライズ](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21)は。

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>日付と時刻

組み込みの .NET の日付と時刻の関数を使用することはできますが (現在と共に`CultureInfo`) は、ロケール固有のされるユーザー設定 (言語から個別に設定することができます) を無視してこのロケールの日付と時刻の書式を設定する。

IOS を使用して、`NSDateFormatter`ユーザーのロケールの設定と一致する出力を生成します。 次のサンプル コードでは、基本的な日付と時刻の書式設定オプションを示しています。

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

英語、米国での結果:

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

スペインのスペイン語の結果:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Apple を参照してください[日付フォーマッタ](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html)詳細についてはドキュメントです。 ロケール依存型の日付と時刻の書式設定をテストする場合は両方をチェック**iPhone 言語**と**リージョン**設定します。

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>右から左 (RTL) のレイアウト

iOS では、さまざまな右から左に対応するアプリの構築を支援する機能が用意されています。

- 自動レイアウトの`leading`と`trailing`(を英語を左側と右側に対応していますが、RTL 言語には逆に) コントロールの配置の属性。
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md)コントロールが右から左に対応するコントロールをレイアウトするために特に便利です。
- 使用`TextAlignment = UITextAlignment.Natural`のテキストの配置 (これは多くの言語が右から左に残ります)。
- `UINavigationController` 自動的に [戻る] ボタンを反転し、スワイプ方向を反転させます。

次のスクリーン ショットに示す、 [Tasky サンプルのローカライズされた](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)アラビア語やヘブライ語で (ただし、英語は、フィールドに入力された)。

[![](images/rtl-ar-sml.png "アラビア語のローカリゼーション")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "ヘブライ語のローカリゼーション")](images/rtl-he.png#lightbox "Hebrew")

iOS を自動的に反転、 `UINavigationController`、およびその他のコントロールは、内側に配置されます`UIStackView`または自動レイアウトに揃えて配置します。
使用して右から左へテキストをローカライズ **.strings** LTR テキストと同じ方法でファイル。

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>コードでは、UI のローカライズ

[(コードのローカライズ版) Tasky](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)サンプルは、コード (ではなく Xib またはストーリー ボード) でユーザー インターフェイスを構築する場所というアプリケーションをローカライズする方法を示します。

### <a name="project-structure"></a>プロジェクトの構造

![](images/solution-code.png "リソースのツリー")

### <a name="localizablestrings-file"></a>Localizable.strings ファイル

上記の説明に従って、 **Localizable.strings**ファイル形式は、キーと値のペアで構成されています。 キー文字列の意図を説明して、値は、アプリで使用される、翻訳されたテキスト。

スペイン語 (**es**)、サンプルのローカライズ版を以下に示します。

```console
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

アプリケーション コードで (これは、ラベルのテキスト、または入力のプレース ホルダーなど) かどうかをユーザー インターフェイスの表示のテキストが設定されている限り、コードを使用して、iOS`GetLocalizedString`を表示する適切な変換を取得します。

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>ストーリー ボード Ui のローカライズ

サンプル[Tasky (ローカライズされたストーリー ボード)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)ストーリー ボード内のコントロールのテキストをローカライズする方法を示しています。

### <a name="project-structure"></a>プロジェクトの構造

**Base.lproj**ディレクトリは、ストーリー ボードが含まれていて、アプリケーションで使用するイメージを含める必要があります。

その他の言語ディレクトリが含まれて、 **Localizable.strings**コードでは、参照されている任意の文字列リソースのファイルと同様に、 **MainStoryboard.strings**内のテキストの翻訳を含むファイル、ストーリー ボード。

![](images/solution-storyboard.png "リソースのツリー")

言語ディレクトリに存在する 1 つをオーバーライドする、ローカライズされているすべてのイメージのコピーを含める必要があります**Base.lproj**します。

### <a name="object-id--localization-id"></a>オブジェクト ID/ローカリゼーション ID

作成すると、ストーリー ボードでのコントロールの編集、各コントロールを選択し、ローカリゼーションに使用する ID を確認してください。

- 場所で Visual Studio for Mac では、 **Properties Pad**と呼びます**ローカリゼーション ID**します。
- Xcode で呼び出されます**オブジェクト ID**します。

この文字列値では、次のスクリーン ショットに示すように"NF3-h8-xmR"などのフォームが多くの場合があります。

![](images/xs-designer-localization-id.png "ストーリー ボードのローカライズの Xcode ビュー")

この値が使用される、 **.strings**ファイルを各コントロールに翻訳されたテキストを自動的に割り当てます。

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

ストーリー ボードの翻訳ファイルの形式と似ています、 **Localizable.strings**ファイルが、キー (左側の値) がユーザー定義することはできませんが、代わりに、非常に特定の形式でなければなりません:`ObjectID.property`します。

この例で**Mainstoryboard.strings**以下`UITextField`s が、`placeholder`ローカライズ可能なテキスト プロパティ`UILabel`s が、`text`プロパティと`UIButton`を使用して既定のテキストを設定`normalTitle`:

```console
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> サイズ クラスをストーリー ボードを使用すると、翻訳、アプリケーションで表示されない可能性があります。 [Apple の Xcode のリリース ノート](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html)いるストーリー ボードまたは XIB がローカライズ正しく次の 3 つが該当する場合を示します。 サイズ クラスを使用して、基本のローカライズおよびビルド ターゲットがユニバーサル、に設定およびビルドには iOS 7.0 が対象とします。 修正プログラムは、2 つの同一ファイルに、文字列のストーリー ボード ファイルが重複しています: **MainStoryboard~iphone.strings**と**MainStoryboard~ipad.strings**の次のスクリーン ショットに示すようにします。
> 
> ![](images/xs-dup-strings.png "文字列のファイル")

<a name="appstore" />

## <a name="app-store-listing"></a>アプリ ストアの一覧

Apple のよく寄せられる質問に依存して[アプリ ストアのローカリゼーション](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization)販売のアプリは、各国の翻訳を入力します。 そのアプリも含まれている場合、ローカライズされた翻訳のみ表示される警告に注意してください **.lproj**言語のディレクトリ。

## <a name="summary"></a>まとめ

この記事では、組み込みのリソースの処理とストーリー ボードの機能を使用して iOS アプリケーションのローカライズの基本について説明します。

で学習する i18n および L10n について iOS、Android、およびクロス プラットフォームのアプリ (Xamarin.Forms を含む) の[このクロスプラット フォーム対応ガイド](~/cross-platform/app-fundamentals/localization.md)します。

## <a name="related-links"></a>関連リンク

- [Tasky (コードのローカライズ版) (サンプル)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (ローカライズされたストーリー ボード) (サンプル)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple のローカリゼーションのガイド](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [クロスプラット フォームのローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms のローカライズ](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android のローカライズ](~/android/app-fundamentals/localization.md)
