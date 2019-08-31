---
title: Xamarin の watchOS の複雑さ
description: このドキュメントでは、Xamarin で watchOS の複雑さを処理する方法について説明します。 また、複雑な機能を追加し、複雑なテンプレートを作成し、サンプルコードを提供する方法についても説明します。
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/03/2017
ms.openlocfilehash: 7e2b3e93baaeac85267c9db2f414793610521f2e
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200028"
---
# <a name="watchos-complications-in-xamarin"></a>Xamarin の watchOS の複雑さ

_watchOS を使用すると、開発者はウォッチ顔に関するカスタムの複雑さを作成できます。_

このページでは、使用できるさまざまな種類の複雑さと、watchOS 3 アプリに複雑なものを追加する方法について説明します。

各 watchOS アプリケーションでは、1つの複雑なアプリケーションしか使用できないことに注意してください。

まず、 [Apple のドキュメント](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html)を読んで、アプリが複雑に適しているかどうかを判断します。 表示には`CLKComplicationFamily` 、次の5種類から選択できます。

[![](complications-images/all-complications-sml.png "5種類の CLKComplicationFamily を使用できます。小、モジュール式 s、モジュール式 l、Utilitarian s、Utilitarian l")](complications-images/all-complications.png#lightbox)

アプリでは、表示されるデータに応じて、1つまたは5つのスタイルのみを実装できます。
また、時間の移動をサポートし、ユーザーが Digital Crown をオンにしたときの過去または将来の時刻の値を指定することもできます。

<a name="adding" />

## <a name="adding-a-complication"></a>複雑な追加

### <a name="configuration"></a>構成

複雑さは、作成時に watch アプリに追加することも、既存のソリューションに手動で追加することもできます。

### <a name="add-new-project"></a>新しいプロジェクトの追加...

**[新しいプロジェクトの追加]** ウィザードには、複雑なコントローラークラスを自動的に作成し、**情報**ファイルを構成するためのチェックボックスがあります。

![](complications-images/file-new-project-sml.png "[複雑なものを含める] チェックボックス")

### <a name="existing-projects"></a>既存のプロジェクト

既存のプロジェクトに複雑なものを追加するには、次のようにします。

1. 新しい**ComplicationController.cs**クラスファイルを作成し、 `CLKComplicationDataSource`を実装します。
2. アプリの情報を構成**し**て、複雑なものを公開し、どのような複雑なファミリがサポートされているかを識別します。

これらの手順については、以下で詳しく説明します。

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource クラス

次C#のテンプレートには、を`CLKComplicationDataSource`実装するために最低限必要なメソッドが含まれています。

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
  public ComplicationController ()
  {
  }
  public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
  {
  }
  public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
  {
  }
  public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
  {
  }
}
```

「[複雑な命令の記述](#writing)」に従って、このクラスにコードを追加します。

### <a name="infoplist"></a>Info.plist

Watch 拡張機能の**情報 plist**ファイルでは、 `CLKComplicationDataSource`の名前と、サポートする必要のある複雑なファミリを指定する必要があります。

[![](complications-images/complications-config-sml.png "複雑なファミリの種類")](complications-images/complications-config.png#lightbox)

**データソースクラス**のエントリの一覧には、複雑な`CLKComplicationDataSource`ロジックを含むサブクラスをサブクラス化するクラス名が表示されます。

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

すべての複雑な機能は、 `CLKComplicationDataSource` `ICLKComplicationDataSource`インターフェイスを実装する抽象クラスのメソッドをオーバーライドして、1つのクラスに実装されます。

### <a name="required-methods"></a>必須のメソッド

複雑な動作を実行するには、次のメソッドを実装する必要があります。

- `GetPlaceholderTemplate`-構成中、またはアプリが値を指定できないときに使用される静的ディスプレイを返します。
- `GetCurrentTimelineEntry`-複雑な動作が実行されている場合は、適切な表示を計算します。
- `GetSupportedTimeTravelDirections``CLKComplicationTimeTravelDirections` - `None`、 、`Forward`、などのオプションを返します。`Forward | Backward` `Backward`

### <a name="privacy"></a>プライバシー

個人データを表示する複雑さ

- `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` または `HideOnLockScreen`

このメソッドからが`HideOnLockScreen`返された場合は、ウォッチがロックされているときに、アイコンまたはアプリケーション名 (およびデータではありません) のいずれかが表示されます。

### <a name="updates"></a>Updates

- `GetNextRequestedUpdateDate`-オペレーティングシステムが次にアプリに対して、更新された複雑な表示データを照会する必要がある時間を返します。

IOS アプリから強制的に更新することもできます。

### <a name="supporting-time-travel"></a>タイムトラベルのサポート

タイムトラベルのサポートは省略可能であり、 `GetSupportedTimeTravelDirections`メソッドによって制御されます。 、 `Forward` `Forward | Backward` 、またはが返された場合は、次のメソッドを実装する必要があります。 `Backward`

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>複雑な記述

複雑さは、単純なデータ表示から、複雑な画像や、タイムトラベルサポートによるデータレンダリングまでの範囲です。 次のコードは、単純な単一テンプレートの複雑さを構築する方法を示しています。

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>サンプル コード

この例では`UtilitarianLarge`テンプレートのみがサポートされているため、この種の複雑な機能をサポートする特定のウォッチ面でのみ選択できます。 ウォッチで複雑さを*選択*すると、その複雑さが表示され、*実行*時にはテキスト**分**  (時間の部分) が表示されます。

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
    public ComplicationController ()
    {
    }
    public ComplicationController (IntPtr p) : base (p)
    {
    }
    public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
        CLKComplicationTimelineEntry entry = null;
    var complicationDisplay = "MINUTE " + DateTime.Now.Minute.ToString(); // text to display on watch face
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge)
        {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText(complicationDisplay); // dynamic display
            entry = CLKComplicationTimelineEntry.Create(NSDate.Now, textTemplate);
        } else {
            Console.WriteLine("Complication family timeline not supported (" + complication.Family + ")");
        }
        handler (entry);
    }
    public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
        CLKComplicationTemplate template = null;
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge) {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat ();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText ("MY COMPLICATION"); // static display
            template = textTemplate;
        } else {
            Console.WriteLine ("Complication family placeholder not not supported (" + complication.Family + ")");
        }
        handler (template);
    }
    public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
        handler (CLKComplicationTimeTravelDirections.None);
    }
}
```


<a name="templates" />

## <a name="complication-templates"></a>複雑になるテンプレート

各複雑なスタイルにはさまざまなテンプレートが用意されています。
**リング**テンプレートを使用すると、進行状況やその他の値をグラフィカルに表示するために使用できる、複雑な進行状況を示す進行状況のリングを表示できます。

[Apple の CLKComplicationTemplate ドキュメント](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>円 (小)

これらのテンプレートクラス名の先頭に`CLKComplicationTemplateCircularSmall`は、次の名前が付けられます。

- **RingImage** -1 つのイメージを表示します。その周囲には、進行状況のリングが表示されます。
- **RingText** -1 行のテキストを表示し、その周囲に進行状況のリングを表示します。
- **Simpleimage** -小さな1つのイメージを表示するだけです。
- **Simpletext** -小さなテキストスニペットを表示するだけです。
- **Stackimage** -画像とテキスト行を1つ上に表示します。
- **Stacktext** -2 行のテキストを表示します。

### <a name="modular-small"></a>モジュール小

これらのテンプレートクラス名の先頭に`CLKComplicationTemplateModularSmall`は、次の名前が付けられます。

- **ColumnsText** -テキスト値 (2 行と2列) の小さなグリッドを表示します。
- **RingImage** -1 つのイメージを表示します。その周囲には、進行状況のリングが表示されます。
- **RingText** -1 行のテキストを表示し、その周囲に進行状況のリングを表示します。
- **Simpleimage** -小さな1つのイメージを表示するだけです。
- **Simpletext** -小さなテキストスニペットを表示するだけです。
- **Stackimage** -画像とテキスト行を1つ上に表示します。
- **Stacktext** -2 行のテキストを表示します。

### <a name="modular-large"></a>モジュール l

これらのテンプレートクラス名の先頭に`CLKComplicationTemplateModularLarge`は、次の名前が付けられます。

- **[列]** -2 つの列を含む3行のグリッドを表示します。必要に応じて、各行の左側に画像を含めることもできます。
- **Standardbody** -2 行のプレーンテキストを含む太字のヘッダー文字列を表示します。 ヘッダーでは、必要に応じて、左側に画像を表示できます。
- **テーブル**-太字のヘッダー文字列を表示し、その下に2×2のテキストを表示します。 ヘッダーでは、必要に応じて、左側に画像を表示できます。
- **TallBody** -太字のヘッダー文字列を表示します。テキストのテキストは、大きい方になります。

### <a name="utilitarian-small"></a>Utilitarian s

これらのテンプレートクラス名の先頭に`CLKComplicationTemplateUtilitarianSmall`は、次の名前が付けられます。

- **Flat** -1 行に画像といくつかのテキストを表示します (テキストは短くする必要があります)。
- **RingImage** -1 つのイメージを表示します。その周囲には、進行状況のリングが表示されます。
- **RingText** -1 行のテキストを表示し、その周囲に進行状況のリングを表示します。
- **正方形**: 正方形の画像を表示します (それぞれ38mm または 42 mm Apple Watch には40px または 44 px の正方形)。

### <a name="utilitarian-large"></a>Utilitarian Large

この複雑なスタイル`CLKComplicationTemplateUtilitarianLargeFlat`には、テンプレートが1つだけあります。
1つの画像といくつかのテキストが1行に表示されます。



## <a name="related-links"></a>関連リンク

- [Apple のドキュメント](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC ビデオ](https://developer.apple.com/videos/play/wwdc2015-209/)
