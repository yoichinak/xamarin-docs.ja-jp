---
title: watchOS で Xamarin には、複雑な問題
description: このドキュメントでは、Xamarin で watchOS コンプリケーションを操作する方法について説明します。 コンプリケーション、テンプレートの記述、厄介に追加する方法について説明し、サンプル コードを提供します。
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/03/2017
ms.openlocfilehash: 85b0c9b0688e9fb310a8f427018a02fe629404bb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61225547"
---
# <a name="watchos-complications-in-xamarin"></a>watchOS で Xamarin には、複雑な問題

_watchOS により、開発者はカスタム コンプリケーションの腕時計型インターフェイスを記述するには_

このページには、さまざまな種類の使用可能な複雑な問題と、コンプリケーションを 3 watchOS アプリを追加する方法について説明します。

WatchOS アプリケーションごとに 1 つコンプリケーションはしかことに注意してください。

まず[Apple のドキュメント](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html)アプリが厄介に適しているかどうかを判断します。 5 は`CLKComplicationFamily`から選択するためにディスプレイの種類。

[![](complications-images/all-complications-sml.png "5 CLKComplicationFamily 可能な型:円形-小、モジュラー (小)、モジュラー (小)、大規模で実利的な大規模な実利")](complications-images/all-complications.png#lightbox)

1 つだけのスタイル、または 5 つすべて、表示されているデータに応じて、アプリを実装できます。
過去または将来の時刻の値を提供するように、ユーザーがデジタル クラウンのタイム トラベルもサポートできます。

<a name="adding" />

## <a name="adding-a-complication"></a>コンプリケーションの追加

### <a name="configuration"></a>構成

複雑な問題の作成時に、watch アプリに追加または既存のソリューションに手動で追加できます。

### <a name="add-new-project"></a>新しいプロジェクトを追加してください.

**新しいプロジェクトを追加しています.** ウィザードには自動的にコンプリケーション コント ローラー クラスを作成および構成する チェック ボックスが含まれています、 **Info.plist**ファイル。

![](complications-images/file-new-project-sml.png "コンプリケーションを含めるチェック ボックス")

### <a name="existing-projects"></a>既存のプロジェクト

コンプリケーションを既存のプロジェクトに追加します。

1. 新規作成**ComplicationController.cs**クラス ファイル、および実装`CLKComplicationDataSource`します。
2. アプリの構成**Info.plist**複雑な作業とどのコンプリケーション ファミリがサポートされている id を公開します。

次の手順については、以下で詳しく説明します。

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource クラス

次C#テンプレートには、実装するために必要な最低限の方法が含まれています、`CLKComplicationDataSource`します。

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

に従って、[記述を合わせる](#writing)手順については、このクラスにコードを追加します。

### <a name="infoplist"></a>Info.plist

ウォッチ拡張機能の**Info.plist**ファイルの名前を指定する必要があります、`CLKComplicationDataSource`コンプリケーションのファミリをサポートするために削除します。

[![](complications-images/complications-config-sml.png "コンプリケーションのファミリの種類")](complications-images/complications-config.png#lightbox)

**データ ソース クラス**エントリの一覧がサブクラスであるにはクラス名の表示は`CLKComplicationDataSource`複雑なロジックを含むサブクラスです。

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

コンプリケーションのすべての機能がからメソッドをオーバーライドする、1 つのクラスで実装されている、`CLKComplicationDataSource`抽象クラス (実装する、`ICLKComplicationDataSource`インターフェイス)。

### <a name="required-methods"></a>必要なメソッド

実行する複雑なは、次のメソッドを実装する必要があります。

- `GetPlaceholderTemplate` -構成のアプリは、値を指定できない場合、または使用する静的な表示を返します。
- `GetCurrentTimelineEntry` -コンプリケーションを実行するときに、適切なディスプレイを計算します。
- `GetSupportedTimeTravelDirections` -からオプションを返します`CLKComplicationTimeTravelDirections`など`None`、 `Forward`、 `Backward`、または`Forward | Backward`します。

### <a name="privacy"></a>プライバシー

個人データを表示する複雑な問題

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` または `HideOnLockScreen`

このメソッドが戻る場合`HideOnLockScreen`コンプリケーションと表示されますか、アイコン、またはアプリケーション名 (データは表示されません)、ウォッチがロックされている場合。

### <a name="updates"></a>更新

- `GetNextRequestedUpdateDate` 場合、オペレーティング システムにする必要がありますコンプリケーションの更新されたデータを表示するようにアプリケーション クエリ次に時刻を返します。

IOS アプリから更新プログラムを強制することもできます。

### <a name="supporting-time-travel"></a>タイム トラベルのサポート

省略可能なとによって制御される時間の移動のサポートを`GetSupportedTimeTravelDirections`メソッド。 返された場合`Forward`、 `Backward`、または`Forward | Backward`し、次のメソッドを実装する必要があります

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>書き込みを合わせる

単純なデータから複雑な問題の範囲は、タイム トラベルのサポートを使用したデータのレンダリング、複雑なイメージを表示します。 次のコードでは、単純な単一テンプレート コンプリケーションを構築する方法を示します。

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>サンプル コード

この例のみをサポート、`UtilitarianLarge`テンプレート、コンプリケーションの種類をサポートする特定の腕時計型インターフェイスにのみ選択できるようにします。 ときに*選択*複雑な問題が表示されます、時計の**マイ コンプリケーション**とタイミング*を実行している*、テキストが表示されます**分_時間_**  (と時間の部分)。

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

## <a name="complication-templates"></a>コンプリケーションのテンプレート

多数のさまざまなテンプレートはコンプリケーション スタイルごとに使用できます。
**リング**テンプレートを使用して、進行状況やその他の値をグラフィカルに表示するために使用すると、コンプリケーションの周りのスタイルの進行状況リングを表示できます。

[Apple の CLKComplicationTemplate docs](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>円形-小

これらのテンプレート クラス名はすべてを先頭`CLKComplicationTemplateCircularSmall`:

- **RingImage** -その周りの進行状況リングを持つ、単一のイメージを表示します。
- **RingText** -単一の周囲の進行状況リングでのテキストを表示します。
- **SimpleImage** -小規模な 1 つのイメージを表示するだけです。
- **SimpleText** -テキストの小さなスニペットを表示するだけです。
- **StackImage** -イメージとテキストは、上、もう 1 つの行を表示
- **StackText** -2 つの行のテキストを表示します。

### <a name="modular-small"></a>モジュラー (小)

これらのテンプレート クラス名はすべてを先頭`CLKComplicationTemplateModularSmall`:

- **ColumnsText** -テキストの値 (2 つの行と 2 つの列) の小さなグリッドを表示します。
- **RingImage** -その周りの進行状況リングを持つ、単一のイメージを表示します。
- **RingText** -単一の周囲の進行状況リングでのテキストを表示します。
- **SimpleImage** -小規模な 1 つのイメージを表示するだけです。
- **SimpleText** -テキストの小さなスニペットを表示するだけです。
- **StackImage** -イメージとテキストは、上、もう 1 つの行を表示
- **StackText** -2 つの行のテキストを表示します。

### <a name="modular-large"></a>モジュラー (大)

これらのテンプレート クラス名はすべてを先頭`CLKComplicationTemplateModularLarge`:

- **列**-必要に応じて各行の左側にイメージを含む 2 つの列を持つ 3 つの行のグリッドを表示します。
- **StandardBody** -太字ヘッダー文字列、プレーン テキストの 2 つの行を表示します。 オプションで、ヘッダーは、左側のイメージを表示できます。
- **テーブル**-太字ヘッダー文字列、2 x 2 のグリッドの下にあるテキストを表示します。 オプションで、ヘッダーは、左側のイメージを表示できます。
- **TallBody** -太字ヘッダー文字列より大きなフォントの 1 行の下のテキストを表示します。

### <a name="utilitarian-small"></a>実利小

これらのテンプレート クラス名はすべてを先頭`CLKComplicationTemplateUtilitarianSmall`:

- **フラット**-イメージとテキスト (テキストは短くする必要があります) を 1 行を表示します。
- **RingImage** -その周りの進行状況リングを持つ、単一のイメージを表示します。
- **RingText** -単一の周囲の進行状況リングでのテキストを表示します。
- **正方形**-(40 px または 44px、38 mm または 42 mm Apple Watch それぞれの正方形)、正方形イメージを表示します。

### <a name="utilitarian-large"></a>大規模な実利

この複合スタイルのテンプレートが 1 つだけ:`CLKComplicationTemplateUtilitarianLargeFlat`します。
1 つの行で、1 つのイメージとテキストの一部を表示します。



## <a name="related-links"></a>関連リンク

- [Apple のドキュメント](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC ビデオ](https://developer.apple.com/videos/play/wwdc2015-209/)
