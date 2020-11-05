---
title: Android の入力方法エディターオプション
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、エントリのソフトキーボードの Input Method Editor オプションを設定する Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7909C738-04B2-4476-9A3B-A6D79BC3B9B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 79ceca6f028ec5d0399251e178d82d1b2fff81ef
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373342"
---
# <a name="entry-input-method-editor-options-on-android"></a>Android の入力方法エディターオプション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、のソフトキーボードの Input Method Editor (IME) オプションを設定し [`Entry`](xref:Xamarin.Forms.Entry) ます。 これには、ソフトキーボードの下隅にあるユーザー操作ボタンの設定と、との対話が含まれ `Entry` ます。 これは、 [`Entry.ImeOptions`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) 添付プロパティを列挙体の値に設定することによって XAML で使用され [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) ます。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

メソッドは、 `Entry.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 [ `Entry.SetImeOptions` ] (Xref: Xamarin.FormsPlatformConfiguration. AndroidSpecific の. Entry. SetImeOptions ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration. Android、 Xamarin.Forms 。Entry Xamarin.Forms }、PlatformConfiguration. AndroidSpecific) メソッドを名前空間で使用して、の [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) ソフトキーボードの [入力方式] アクションオプションを設定し [`Entry`](xref:Xamarin.Forms.Entry) [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) ます。列挙体には次の値を指定します。

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) –特定のアクションキーが必要ないこと、および基になるコントロールが可能な場合はそれ自体を生成することを示します。 これは、またはのいずれかになり `Next` `Done` ます。
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) –アクションキーが使用できないことを示します。
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) –アクションキーが "移動" 操作を実行し、ユーザーが入力したテキストのターゲットを取得することを示します。
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) –アクションキーが "検索" 操作を実行し、ユーザーが入力したテキストを検索した結果を取得することを示します。
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) –アクションキーが "send" 操作を実行し、テキストをターゲットに配信することを示します。
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) –アクションキーが "next" 操作を実行し、ユーザーがテキストを受け入れる次のフィールドに到達することを示します。
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) –アクションキーがソフトキーボードを終了する "done" 操作を実行することを示します。
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) –アクションキーが "前の" 操作を実行し、テキストを受け取る前のフィールドにユーザーを取得することを示します。
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) –アクションオプションを選択するマスク。
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) –スペルチェックをユーザーから学習することも、ユーザーが以前に入力した内容に基づいて修正候補を表示しないことを示します。
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – UI が全画面表示されないように指定します。
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) –抽出されたテキストに対して UI が表示されないことを示します。
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) –カスタムアクションの UI が表示されないことを示します。

結果として、指定された [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 値がのソフトキーボードに適用され [`Entry`](xref:Xamarin.Forms.Entry) 、Input Method Editor オプションが設定されます。

[![入力 Input Method Editor プラットフォーム固有](entry-ime-options-images/entry-imeoptions.png "入力 Input Method Editor プラットフォーム固有")](entry-ime-options-images/entry-imeoptions-large.png#lightbox "入力 Input Method Editor プラットフォーム固有")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)