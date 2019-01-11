---
title: Android でのエントリの入力方式エディター オプション
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Android プラットフォームに固有のエントリのソフト キーボードのエディター オプションの入力方法を設定するを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7909C738-04B2-4476-9A3B-A6D79BC3B9B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 930614b2479d0edcdba74ed3f98a169491252c63
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54208917"
---
# <a name="entry-input-method-editor-options-on-android"></a>Android でのエントリの入力方式エディター オプション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

この Android のプラットフォーム固有設定入力方式エディター (IME) オプションのソフト キーボードの[ `Entry`](xref:Xamarin.Forms.Entry)します。 これは、ソフト キーボード、およびとの対話の下隅にあるユーザーのアクション ボタンの設定が含まれています、`Entry`します。 これは、 XAML で[`Entry.ImeOptions`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty)添付プロパティを[`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)列挙型の値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)のソフト キーボードの入力メソッドのアクションのオプションを設定するため、名前空間、 [ `Entry` ](xref:Xamarin.Forms.Entry)、[ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)次の値を提供する列挙体。

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – 特定のアクションのキーは必要ありませんし、可能な基になるコントロールが、独自の場合に生成されることを示します。 これになりますか`Next`または`Done`します。
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – アクション キーがない使用できることを示します。
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – アクション キーは、"go"操作を実行は、入力テキストのターゲットにしてユーザーを導くことを示します。
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – [検索] 操作を実行するアクション キー、入力したテキストの検索の結果にユーザーを導くことを示します。
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – アクション キーがターゲットにテキストを提供する「送信」操作を実行することを示します。
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – アクション キーがテキストを受け入れる次のフィールドにユーザーを作成、[次へ] の操作を実行することを示します。
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – アクション キーがソフト キーボードを閉じる、"done"操作を実行することを示します。
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – アクション キーがテキストを受け取る前のフィールドに、ユーザーがかかっています「前」の操作を実行することを示します。
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – アクションのオプションを選択するマスク。
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – こと、スペル チェックによっては、ユーザーから学ぶでも、ユーザーが以前入力した内容に基づいて修正の提案を示します。
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – UI が全画面表示にならないことを示します。
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – 抽出されたテキストの UI が表示されませんを示します。
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – カスタム アクションの UI が表示しないことを示します。

結果は、指定されたを[ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)のソフト キーボードを適用する値、 [ `Entry` ](xref:Xamarin.Forms.Entry)、入力方式エディターのオプションを設定します。

[![エントリは、エディターのプラットフォームに固有のメソッドを入力](entry-ime-options-images/entry-imeoptions.png "エントリ エディターのプラットフォームに固有のメソッドの入力")](entry-ime-options-images/entry-imeoptions-large.png#lightbox "エントリ エディターのプラットフォームに固有のメソッドの入力")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
