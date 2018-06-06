---
title: watchOS Xamarin で複雑な問題
description: このドキュメントでは、Xamarin で watchOS 複雑さの一部を操作する方法について説明します。 コンプリケーション、テンプレートを作成、問題を追加する方法について説明し、サンプル コードを提供します。
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/03/2017
ms.openlocfilehash: 3c69f65091e7d6c83afe34c6d8c06477cc5d133b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791840"
---
# <a name="watchos-complications-in-xamarin"></a>watchOS Xamarin で複雑な問題

_watchOS により、開発者はウォッチ面のカスタム複雑さの一部を記述するには_

このページでは、異なる種類の使用可能な複雑な問題および watchOS 3 アプリに問題を追加する方法について説明します。

各 watchOS アプリケーションがコンプリケーションの 1 つを持てることに注意してください。

参照して[Apple のドキュメント](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html)をアプリは問題に適しているかどうかを判断します。 5 がある`CLKComplicationFamily`種類の表示を選択します。

[![](complications-images/all-complications-sml.png "使用可能な 5 CLKComplicationFamily 型: 循環小さな、小さなモジュール型、モジュール、大規模な実用的小さな実用的大きな")](complications-images/all-complications.png#lightbox)

アプリには、1 つのスタイル、または 5 つすべてを表示するデータに応じてを実装できます。
過去または将来の時刻の値を提供するように、ユーザーがデジタル クラウンをオンに、タイム トラベルをサポートすることもできます。

<a name="adding" />

## <a name="adding-a-complication"></a>問題を追加します。

### <a name="configuration"></a>構成

複雑さの一部は、既存のソリューションに手動で追加または作成中に、watch アプリを追加できます。

### <a name="add-new-project"></a>新しいプロジェクトを追加してください.

**新しいプロジェクトを追加しています.** ウィザードに自動的にコンプリケーション コント ローラー クラスを作成および構成する チェック ボックスが含まれています、 **Info.plist**ファイル。

![](complications-images/file-new-project-sml.png "含めるコンプリケーションのチェック ボックス")

### <a name="existing-projects"></a>既存のプロジェクト

問題を既存のプロジェクトに追加します。

1. 新しい**ComplicationController.cs**クラス ファイルおよび実装`CLKComplicationDataSource`です。
2. アプリの構成**Info.plist**コンプリケーション、およびどのコンプリケーション ファミリがサポートされている id を公開します。

次の手順についてで詳しく説明します。

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource クラス

次の c# テンプレートには、実装する場合は、最低限必要なメソッドが含まれています、`CLKComplicationDataSource`です。

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

以下の[、コンプリケーションの書き込み](#writing)をこのクラスにコードを追加する手順。

### <a name="infoplist"></a>Info.plist

ウォッチ拡張機能の**Info.plist**ファイルの名前を指定する必要があります、`CLKComplicationDataSource`サポートするどのコンプリケーションのファミリとします。

[![](complications-images/complications-config-sml.png "コンプリケーションのファミリ型")](complications-images/complications-config.png#lightbox)

**データ ソース クラス**エントリの一覧はそのサブクラスにクラス名に表示`CLKComplicationDataSource`複雑なロジックを含むサブクラスです。

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

すべてのコンプリケーション機能は実装からメソッドをオーバーライドする 1 つのクラス、`CLKComplicationDataSource`抽象クラス (を実装する、`ICLKComplicationDataSource`インターフェイス)。

### <a name="required-methods"></a>必要なメソッド

実行するコンプリケーションの次のメソッドを実装する必要があります。

- `GetPlaceholderTemplate` 構成時に、または、アプリは、値を指定できないときに使用する静的表示を返します。
- `GetCurrentTimelineEntry` -、コンプリケーションが実行されているときに、適切なディスプレイを計算します。
- `GetSupportedTimeTravelDirections` -からオプションを返します`CLKComplicationTimeTravelDirections`など`None`、 `Forward`、 `Backward`、または`Forward | Backward`です。

### <a name="privacy"></a>プライバシー

個人のデータを表示する複雑な問題

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` または `HideOnLockScreen`

このメソッドが戻る場合`HideOnLockScreen`いずれかのアイコン、アプリケーション名、およびデータは表示されません)、問題が表示されます、ウォッチがロックされている場合。

### <a name="updates"></a>更新

- `GetNextRequestedUpdateDate` -クエリする場合、オペレーティング システムを次にコンプリケーションの更新されたデータを表示するようにアプリケーション時間を返します。

IOS アプリからの更新を強制することもできます。

### <a name="supporting-time-travel"></a>タイム トラベルのサポート

省略可能とによって制御される時間の旅行のサポートを`GetSupportedTimeTravelDirections`メソッドです。 返された場合`Forward`、 `Backward`、または`Forward | Backward`し、次のメソッドを実装する必要があります

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>問題の記述

単純なデータの範囲は複雑さの一部は、複雑なイメージとタイム トラベル サポート データの表示を表示します。 次のコードでは、単純、単一テンプレート コンプリケーションをビルドする方法を示します。

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>サンプル コード

この例のみをサポート、`UtilitarianLarge`テンプレート、コンプリケーションの種類をサポートする特定のウォッチ面でのみ選択できるようにします。 ときに*を選択すると*複雑さの一部が表示されます、ウォッチ上**マイ コンプリケーション**とタイミング*を実行している*、テキストが表示**分_時間_**  (時間の一部) にします。

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

さまざまなテンプレートの数はコンプリケーション スタイルごとに使用できます。
**リング**テンプレートを使用して、進行状況やその他の値をグラフィカルに表示するために使用すると、コンプリケーションの周りのスタイルの進行状況リングを表示できます。

[Apple の CLKComplicationTemplate docs](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>循環小

これらのテンプレート クラス名は、すべて先頭に付きます`CLKComplicationTemplateCircularSmall`:

- **RingImage**の周りの進行状況リングを持つ、単一のイメージを表示します。
- **RingText** -単一の行の周りの進行状況リングでのテキストを表示します。
- **SimpleImage** -1 つの小さな画像を表示するだけです。
- **SimpleText** -テキストの小さなスニペットを表示するだけです。
- **StackImage** -イメージおよびテキストは、上下の行を表示
- **StackText** -2 つの行のテキストを表示します。

### <a name="modular-small"></a>モジュール式の小さな

これらのテンプレート クラス名は、すべて先頭に付きます`CLKComplicationTemplateModularSmall`:

- **ColumnsText** -テキスト値 (2 行と 2 つの列) の小さなグリッドを表示します。
- **RingImage**の周りの進行状況リングを持つ、単一のイメージを表示します。
- **RingText** -単一の行の周りの進行状況リングでのテキストを表示します。
- **SimpleImage** -1 つの小さな画像を表示するだけです。
- **SimpleText** -テキストの小さなスニペットを表示するだけです。
- **StackImage** -イメージおよびテキストは、上下の行を表示
- **StackText** -2 つの行のテキストを表示します。

### <a name="modular-large"></a>大規模なモジュール

これらのテンプレート クラス名は、すべて先頭に付きます`CLKComplicationTemplateModularLarge`:

- **列**-各行の左側にイメージを含めることも、2 つの列を持つ 3 つの行のグリッドを表示します。
- **StandardBody** -プレーン テキストの 2 つの行での太字ヘッダー文字列を表示します。 ヘッダーは、左側のイメージを必要に応じて表示できます。
- **テーブル**の下にあるテキストの 2 × 2 のグリッドでの太字ヘッダー文字列を表示します。 ヘッダーは、左側のイメージを必要に応じて表示できます。
- **TallBody** -大きなフォントの単一行の下にあるテキストでの太字ヘッダー文字列を表示します。

### <a name="utilitarian-small"></a>実用的小

これらのテンプレート クラス名は、すべて先頭に付きます`CLKComplicationTemplateUtilitarianSmall`:

- **フラット**-(テキストは、短い必要があります) は 1 行で、イメージといくつかのテキストが表示されます。
- **RingImage**の周りの進行状況リングを持つ、単一のイメージを表示します。
- **RingText** -単一の行の周りの進行状況リングでのテキストを表示します。
- **正方形**-(40 px または 44px、38 mm または 42 mm Apple Watch それぞれの四角形) 正方形イメージを表示します。

### <a name="utilitarian-large"></a>大規模な実用的

このコンプリケーション スタイルのテンプレートが 1 つだけ:`CLKComplicationTemplateUtilitarianLargeFlat`です。
すべて 1 つの行に単一のイメージといくつかのテキストが表示されます。



## <a name="related-links"></a>関連リンク

- [Apple のドキュメント](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC ビデオ](https://developer.apple.com/videos/play/wwdc2015-209/)
