---
title: Xamarin. iOS でのローカライズ
description: このドキュメントでは、iOS のローカライズ機能と、Xamarin iOS アプリでこれらの機能を使用する方法について説明します。 言語、ロケール、文字列ファイル、起動イメージなどについて説明します。
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/28/2017
ms.openlocfilehash: 6171066c8a6ceed7314cc50510ce4813d5ce1c0c
ms.sourcegitcommit: e27e29c14b783263e063baaa65d4eecb8dd31f57
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98628983"
---
# <a name="localization-in-xamarinios"></a>Xamarin. iOS でのローカライズ

_このドキュメントでは、iOS SDK のローカライズ機能と、Xamarin を使用してそれらにアクセスする方法について説明します。_

Unicode 以外のデータを処理する必要があるアプリケーションに文字セットやコードページを含める方法については、「 [国際化エンコーディング](encodings.md) 」を参照してください。

## <a name="ios-platform-features"></a>iOS プラットフォームの機能

ここでは、iOS のローカライズ機能の一部について説明します。 特定のコードと例については、 [次のセクション](#localization-basics-in-ios) に進んでください。

### <a name="language"></a>Language

ユーザーは、 **設定** アプリで言語を選択します。 この設定は、オペレーティングシステムおよびアプリによって表示される言語の文字列とイメージに影響します。

アプリで使用されている言語を確認するには、の最初の要素を取得し `NSBundle.MainBundle.PreferredLocalizations` ます。

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

この値は、 `en` 英語、 `es` スペイン語、日本語などの言語コードになり `ja` ます。返される値は、アプリケーションでサポートされているローカライズの1つに制限されます (フォールバック規則を使用して最適な一致を判断します)。

アプリケーションコードは常にこの値をチェックする必要はありません。 Xamarin と iOS は、ユーザーの言語に合った正しい文字列またはリソースを自動的に提供するのに役立つ機能を提供します。 これらの機能については、このドキュメントの残りの部分で説明します。

> [!NOTE]
> `NSLocale.PreferredLanguages`アプリでサポートされているローカライズに関係なく、ユーザーの言語設定を決定するには、を使用します。 IOS 9 では、このメソッドによって返された値が変更されています。詳細については、「 [テクニカルノート TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) 」を参照してください。

### <a name="locale"></a>Locale

ユーザーは、 **設定** アプリでロケールを選択します。 この設定は、日付、時刻、数値、通貨の書式設定に影響します。

これにより、ユーザーは、12時間形式と24時間形式のどちらであるか、小数点区切り文字がコンマまたはポイントかどうか、および日付の日、月、年の順序を表示するかどうかを選択できます。

Xamarin では、Apple の iOS クラス ( `NSNumberFormatter` ) と、System の .net クラスの両方にアクセスできます。 開発者は、それぞれの機能が異なるため、それぞれのニーズに適した方法を評価する必要があります。 特に、StoreKit を使用して In-App 購入価格を取得して表示する場合、返される価格情報には Apple の書式設定クラスを使用する必要があります。

現在のロケールは、次の2つの方法のいずれかで照会できます。

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

最初の値はオペレーティングシステムによってキャッシュされる可能性があるため、ユーザーが現在選択しているロケールを常に反映するとは限りません。 2番目の値を使用して、現在選択されているロケールを取得します。

> [!NOTE]
> Mono (Xamarin のベースになっている .NET ランタイム) と Apple の iOS Api は、同一の言語とリージョンの組み合わせをサポートしていません。
> このため、Mono の有効な値にマップされていない iOS **設定** アプリで言語/リージョンの組み合わせを選択することができます。 たとえば、iPhone の言語を英語に設定し、その地域をスペインに設定すると、次の Api によって異なる値が生成されます。
>
> - `CurrentThead.CurrentCulture`: en-us (Mono API)
> - `CurrentThread.CurrentUICulture`: en-us (Mono API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (Apple API)
>
> Mono はリソースを選択し、 `CurrentThread.CurrentUICulture` 日付と通貨の書式を設定するためにを使用するため `CurrentThread.CurrentCulture` 、これらの言語と地域の組み合わせでは、mono ベースのローカライズ (たとえば、.resx ファイルを使用した場合) によって期待される結果が得られない場合があります。 このような状況では、必要に応じて、Apple の Api を使用してローカライズします。

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS では、 `NSCurrentLocaleDidChangeNotification` ユーザーがロケールを更新するとが生成されます。 アプリケーションは、実行中にこの通知をリッスンでき、UI に適切な変更を加えることができます。

## <a name="localization-basics-in-ios"></a>IOS でのローカライズの基礎

IOS の次の機能は、Xamarin で簡単に使用して、ユーザーに表示するためのローカライズされたリソースを提供します。 これらのアイデアを実装する方法については、 [TaskyL10n サンプル](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) を参照してください。

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>情報 plist での既定の言語とサポートされる言語の指定

[Technical Q&A QA1828: ios でアプリの言語がどのように決定される](https://developer.apple.com/library/content/qa/qa1828/_index.html)かについては、Apple では、アプリで使用する言語を ios が選択する方法について説明しています。 表示される言語には、次の要因が影響します。

- ユーザーの優先言語 ( **設定** アプリにあります)
- アプリにバンドルされているローカライズ (. lproj フォルダー)
- `CFBundleDevelopmentRegion` (アプリケーションの既定の言語を指定する **情報 plist** 値)
- `CFBundleLocalizations` (サポートされているすべてのローカライズを指定する **情報 plist** 配列)

テクニカル Q&A で示されているように、は `CFBundleDevelopmentRegion` アプリの既定の地域と言語を表します。 アプリがユーザーの優先言語を明示的にサポートしていない場合は、このフィールドで指定された言語が使用されます。

> [!IMPORTANT]
> iOS 11 では、以前のバージョンのオペレーティングシステムよりも厳密にこの言語選択メカニズムが適用されます。 このため、サポートされているローカライズを明示的に宣言していない iOS 11 アプリは、lproj フォルダーを含めるか、値を設定することによって、ios 11 では ios `CFBundleLocalizations` 10 とは異なる言語を表示できます。

`CFBundleDevelopmentRegion`が **情報** ファイルに指定されていない場合、Xamarin のビルドツールは現在、の既定値を使用します `en_US` 。 これは将来のリリースで変更される可能性がありますが、既定の言語が英語であることを意味します。

アプリが期待される言語を選択できるようにするには、次の手順を実行します。

- 既定の言語を指定します。 **情報 plist** を開き、**ソース** ビューを使用してキーの値を設定します。 XML では、 `CFBundleDevelopmentRegion` 次のようになります。

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

この例では、"es" を使用して、ユーザーの優先言語がサポートされていない場合に、既定でスペイン語を指定します。

- サポートされているすべてのローカライズを宣言します。 **情報 plist** では、**ソース** ビューを使用してキーの配列を設定します `CFBundleLocalizations` 。 XML では、次のようになります。

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

.Resx ファイルなどの .NET メカニズムを使用してローカライズされた Xamarin iOS アプリでは、これらの **情報** を提供する必要があります。

**これらの情報の** 詳細については、Apple の [情報プロパティリストキーのリファレンスを参照](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html)してください。

### <a name="getlocalizedstring-method"></a>GetLocalizedString メソッド

メソッドは、 `NSBundle.MainBundle.GetLocalizedString` プロジェクト内の **文字列** ファイルに格納されているローカライズされたテキストを検索します。 これらのファイルは、言語別に、特別に指定さ **れ** たディレクトリ (拡張子は小文字の "L") で構成されます。

#### <a name="strings-file-locations"></a>. 文字列ファイルの場所

- **Base. lproj** は、既定の言語のリソースが格納されているディレクトリです。
  多くの場合、プロジェクトのルートに配置されます (ただし、 **Resources** フォルダーに配置することもできます)。
- **&lt; language &gt; . lproj** ディレクトリは、サポートされている言語ごとに作成されます。通常は **Resources** フォルダーに作成されます。

各言語ディレクトリには、次のような複数の異なる **文字列** ファイルがあります。

- **ローカライズ** 可能な文字列–ローカライズされたテキストのメインリスト。
- **インフォ plist。文字列** –このファイルでは、アプリケーション名などを変換するために特定の特定のキーを使用できます。
- **\<storyboard-name> . 文字列**–ストーリーボード内のユーザーインターフェイス要素の翻訳を含む省略可能なファイルです。

これらのファイルの **ビルドアクション** は、 **バンドルリソース** である必要があります。

#### <a name="strings-file-format"></a>. strings ファイル形式

ローカライズされた文字列値の構文は次のとおりです。

```console
/* comment */
"key"="localized-value";
```

文字列の次の文字をエスケープする必要があります。

- `\"` あらかじめ
- `\\` 逆
- `\n` 改行

これは、 **es/ローカライズ** 可能な文字列 (ie の例です。スペイン語) サンプルのファイルを次に示します。

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

### <a name="images"></a>画像

IOS でイメージをローカライズするには:

1. コードでイメージを参照します。次に例を示します。

    ```csharp
    UIImage.FromBundle("flag");
    ```

2. 既定のイメージファイル **flag.png** を、 **基本の lproj** (ネイティブ開発言語のディレクトリ) に配置します。

3. 必要に応じて、ローカライズされたバージョンのイメージを各言語の **lproj** フォルダーに配置します (例 **es**. lproj, **ja-jp**)。 各言語ディレクトリで同じファイル名 **flag.png** を使用します。

特定の言語のイメージが存在しない場合、iOS は既定のネイティブ言語フォルダーに戻り、そこからイメージを読み込みます。

#### <a name="launch-images"></a>起動イメージ

各言語の **lproj** ディレクトリに配置する場合は、起動イメージ (および iPhone 6 モデルの XIB またはストーリーボード) に標準的な名前付け規則を使用します。

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>アプリの名前

**. Lproj** ディレクトリに **インフォ plist 文字列** ファイルを配置すると、アプリケーション名を含む、アプリの **情報** の一部の値をオーバーライドできます。

```console
"CFBundleDisplayName" = "LeónTodo";
```

[アプリケーション固有の文字列をローカライズ](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21)するために使用できるその他のキーは次のとおりです。

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>日付と時刻

組み込みの .NET の日付と時刻の関数 (現在のを含む) を使用してロケールの日付と時刻の書式を設定することもでき `CultureInfo` ますが、ロケール固有のユーザー設定 (言語とは別に設定可能) は無視されます。

IOS を使用して、 `NSDateFormatter` ユーザーのロケール設定に一致する出力を生成します。 次のサンプルコードは、基本的な日付と時刻の書式設定オプションを示しています。

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

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

スペインでのスペイン語の結果:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

詳細については、Apple [Date フォーマッタ](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) のドキュメントを参照してください。 ロケールを区別する日付と時刻の書式をテストする場合は、 **iPhone の言語** と **地域** の両方の設定を確認します。

<a name="rtl"></a>

### <a name="right-to-left-rtl-layout"></a>右から左 (RTL) のレイアウト

iOS には、RTL 対応アプリの構築に役立つさまざまな機能が用意されています。

- 自動レイアウトのおよび属性を使用して `leading` `trailing` コントロールの配置を調整します (英語の場合は左および右に、RTL 言語では逆に対応します)。
  コントロールは、 [`UIStackView`](~/ios/user-interface/controls/uistackview.md) RTL を認識するようにコントロールをレイアウトする場合に特に便利です。
- `TextAlignment = UITextAlignment.Natural`テキストの配置に使用します (ほとんどの言語に対して残されますが、RTL の場合は右側にあります)。
- `UINavigationController` [戻る] ボタンを自動的に反転し、スワイプの方向を反転させます。

次のスクリーンショットは、アラビア語とヘブライ語のローカライズされた [Tasky サンプル](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) を示しています (ただし、フィールドには英語が入力されています)。

[![アラビア語でのローカライズ](images/rtl-ar-sml.png)](images/rtl-ar.png#lightbox "アラビア語")

[![ヘブライ語でのローカライズ](images/rtl-he-sml.png)](images/rtl-he.png#lightbox "ヘブライ語")

iOS では、が自動的に反転され、 `UINavigationController` その他のコントロールは `UIStackView` 自動レイアウトに沿って配置されます。
RTL テキストは、LTR テキストと同じように、文字列ファイルを使用してローカライズされます **。**

<a name="code"></a>

## <a name="localizing-the-ui-in-code"></a>コードでの UI のローカライズ

[Tasky (コード内でローカライズ)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)サンプルは、(xib またはストーリーボードではなく) コードでユーザーインターフェイスが構築されるアプリケーションをローカライズする方法を示しています。

### <a name="project-structure"></a>プロジェクト構造

![スクリーンショットには、ローカライズ可能な文字列の場所を含むサンプルのリソースツリーが表示されています。](images/solution-code.png)

### <a name="localizablestrings-file"></a>ローカライズ可能な文字列ファイル

前述のように、 **ローカライズ** 可能な文字列ファイル形式は、キーと値のペアで構成されます。 キーは文字列の意図を表し、値はアプリで使用される翻訳されたテキストです。

このサンプルのスペイン語 (**es**) ローカライズ版を以下に示します。

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

### <a name="performing-the-localization"></a>ローカリゼーションの実行

アプリケーションコードで、ユーザーインターフェイスの表示テキストが設定されている場合 (ラベルのテキストであるか、入力のプレースホルダーであるかにかかわらず)、コードは iOS 関数を使用して、 `GetLocalizedString` 表示する正しい翻訳を取得します。

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"></a>

## <a name="localizing-storyboard-uis"></a>ストーリーボード Ui のローカライズ

サンプル [Tasky (ローカライズされたストーリーボード)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) は、ストーリーボードのコントロールのテキストをローカライズする方法を示しています。

### <a name="project-structure"></a>プロジェクト構造

**基本の lproj** ディレクトリにはストーリーボードが含まれており、アプリケーションで使用されるイメージも含まれている必要があります。

他の言語ディレクトリには、コードで参照されている文字列リソースのローカライズ可能な **文字列** ファイルと、ストーリーボードのテキストの翻訳を含む **mainstoryboard.storyboard ファイル** ファイルが含まれています。

![スクリーンショットには、Mainstoryboard.storyboard ファイル文字列の場所を含むサンプルのリソースツリーが表示されています。](images/solution-storyboard.png)

言語ディレクトリには、ローカライズ済みのイメージのコピーが含まれていなければなり **ません。**

### <a name="object-id--localization-id"></a>オブジェクト ID/ローカライズ ID

ストーリーボードでコントロールを作成および編集する場合は、各コントロールを選択し、ローカライズに使用する ID を確認します。

- Visual Studio for Mac では、 **Properties Pad** にあり、 **ローカライズ ID** と呼ばれます。
- Xcode では、これは **オブジェクト ID** と呼ばれます。

この文字列値は、次のスクリーンショットに示すように、"NF3-h8-xmR" のような形式になっていることがよくあります。

![ストーリーボードのローカライズの Xcode ビュー](images/xs-designer-localization-id.png)

この値は、各コントロールに翻訳されたテキストを自動的に割り当てるために、文字列ファイルで使用され **ます。**

### <a name="mainstoryboardstrings"></a>Mainstoryboard.storyboard ファイル

ストーリーボード変換ファイルの形式は、ローカライズ可能な **文字列** ファイルに似ています。ただし、キー (左側の値) はユーザー定義にはできませんが、その代わりに、という形式を使用する必要が `ObjectID.property` あります。

次の **mainstoryboard.storyboard ファイル** の例では、 `UITextField` `placeholder` にローカライズ `UILabel` 可能な text プロパティがあることを確認できます。にはプロパティがあり、 `text` `UIButton` s の既定のテキストはを使用して設定され `normalTitle` ます。

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
> サイズクラスのストーリーボードを使用すると、アプリケーションに表示されない翻訳が発生する可能性があります。 [Apple の Xcode のリリースノート](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) では、ストーリーボードまたは XIB が、サイズクラスを使用していて、基本ローカリゼーションとビルドターゲットが Universal に設定されており、ビルドが iOS 7.0 を対象としている場合は、正しくローカライズされないことを示しています。 この問題を解決するには、次のスクリーンショットに示すように、ストーリーボード文字列ファイルを2つの同じファイル ( **mainstoryboard.storyboard ファイル ~ iphone** と **mainstoryboard.storyboard ファイル ~ ipad. 文字列**) に複製します。
>
> ![文字列ファイル](images/xs-dup-strings.png)

<a name="appstore"></a>

## <a name="app-store-listing"></a>アプリストアの一覧

[アプリストアのローカライズ](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization)に関する APPLE の FAQ に従って、アプリが販売されている各国の翻訳を入力します。 アプリに言語のローカライズされた **lproj** ディレクトリも含まれている場合にのみ、翻訳が表示されることを警告します。

## <a name="summary"></a>まとめ

この記事では、組み込みのリソース処理機能とストーリーボード機能を使用した iOS アプリケーションのローカライズの基本について説明します。

[このクロスプラットフォームガイド](~/cross-platform/app-fundamentals/localization.md)では、IOS、Android、クロスプラットフォームアプリ (Xamarin を含む) の I18n と L10n の詳細について説明します。

## <a name="related-links"></a>関連リンク

- [Tasky (コードでローカライズ) (サンプル)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (ローカライズされたストーリーボード) (サンプル)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple のローカライズガイド](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [クロスプラットフォームのローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms のローカライズ](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android のローカライズ](~/android/app-fundamentals/localization.md)
