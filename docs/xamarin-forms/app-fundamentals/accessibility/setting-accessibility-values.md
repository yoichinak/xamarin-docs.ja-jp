---
title: ユーザー インターフェイス要素のアクセシビリティの値の設定
description: この記事では、スクリーン リーダーが要素に関する ページで会話できますように AutomationProperties クラスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: ad7b1c41f34c14a81910d5be30fd6484919e8d39
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241885"
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>ユーザー インターフェイス要素のアクセシビリティの値の設定

_Xamarin.Forms では、ネイティブのユーザー補助の値を設定する AutomationProperties クラスからの接続のプロパティを使用してユーザー インターフェイス要素に設定するユーザー補助機能の値を許可します。この記事では、スクリーン リーダーが要素に関する ページで会話できますように AutomationProperties クラスを使用する方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Forms は、次の添付プロパティを使用してユーザー インターフェイス要素に設定するユーザー補助機能の値を指定できます。

- `AutomationProperties.IsInAccessibleTree` – 要素がアクセス可能なアプリケーションで使用できるかどうかを示します。 詳細については、次を参照してください。 [AutomationProperties.IsInAccessibleTree](#isinaccessibletree)です。
- `AutomationProperties.Name` – 要素の speakable 識別子として機能する要素の簡単な説明。 詳細については、次を参照してください。 [AutomationProperties.Name](#name)です。
- `AutomationProperties.HelpText` – 要素に関連付けられているツールヒント テキストとしての考えられると、要素の長い説明。 詳細については、次を参照してください。 [AutomationProperties.HelpText](#helptext)です。
- `AutomationProperties.LabeledBy` – 現在の要素のアクセシビリティに関する情報を定義する別の要素を許可します。 詳細については、次を参照してください。 [AutomationProperties.LabeledBy](#labeledby)です。

これらは接続、スクリーン リーダーが要素に関する話すことができるようにセット ネイティブ アクセシビリティのプロパティの値です。 添付プロパティの詳細については、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)を参照してください。

> [!IMPORTANT]
> 使用して、`AutomationProperties`添付プロパティ Android で UI テストの実行に影響を与える可能性があります。 [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/)、`AutomationProperties.Name`と`AutomationProperties.HelpText`プロパティはどちらも設定ネイティブ`ContentDescription`プロパティで、`AutomationProperties.Name`と`AutomationProperties.HelpText`プロパティ値をよりも優先`AutomationId`値 (両方`AutomationProperties.Name`と`AutomationProperties.HelpText`が設定された場合、値が連結されます)。 つまり、他のテストを探して`AutomationId`場合は失敗`AutomationProperties.Name`または`AutomationProperties.HelpText`要素に設定されます。 このシナリオでは、値の検索に UI テストを変更する`AutomationProperties.Name`または`AutomationProperties.HelpText`、または両方を連結します。

各プラットフォームには、ユーザー補助機能の値のナレーションをさまざまな画面リーダーがあります。

- iOS では、ボイス オーバーがします。 詳細については、次を参照してください。[ボイス オーバーのデバイスでテストのユーザー補助](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html)developer.apple.com にします。
- Android では、応答があります。 詳細については、次を参照してください。[テスト アプリケーションのユーザー補助](https://developer.android.com/training/accessibility/testing.html#talkback)developer.android.com にします。
- Windows には、ナレーターがあります。 詳細については、次を参照してください。[ナレーターを使用して、メイン アプリケーション シナリオを確認](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/)です。

ただし、スクリーン リーダーの正確な動作は、ソフトウェアおよびそのユーザーの構成によって異なります。 たとえば、ほとんどのスクリーン リーダーを読み取る、フォーカスを受け取るときに、コントロールに関連付けられたテキスト自体の向き、ページ上のコントロール間で移動するためにユーザーを有効にします。 スクリーン リーダーによっても読み取り、アプリケーション全体のユーザー インターフェイス ページが表示されたら、それを移動する前にすべてのページの利用可能な情報のコンテンツを受信するユーザーを有効にします。

また、スクリーン リーダーは、さまざまなユーザー補助の値を読み取る。 サンプル アプリケーション。

- ボイス オーバーは読み取り、`Placeholder`の値、`Entry`コントロールを使用するための手順と、その後です。
- 応答の読み取りは、`Placeholder`の値、 `Entry`」と入力し、`AutomationProperties.HelpText`値、コントロールを使用する方法を説明します。
- ナレーターは読み取り、`AutomationProperties.LabeledBy`の値、`Entry`については、コントロールを使用すると、その後です。

さらに、ナレーターは優先順位を設定`AutomationProperties.Name`、 `AutomationProperties.LabeledBy`、し`AutomationProperties.HelpText`です。 Android で応答を組み合わせることも、`AutomationProperties.Name`と`AutomationProperties.HelpText`値。 したがって、ある完全なアクセシビリティのテストは実行が最適なエクスペリエンスを確認するには、各プラットフォームでをお勧めします。

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree`添付プロパティは、`boolean`要素にはアクセス可能で、スクリーン リーダーに、そのため表示される場合を決定します。 設定する必要があります`true`アタッチされるプロパティを他のユーザー補助機能を使用します。 これには、XAML で次のようにします。

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

また、そのことができますよう設定できます (C#)。

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> なお、 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/)メソッドを使用して設定することもできます、`AutomationProperties.IsInAccessibleTree`添付プロパティ。 `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name`添付プロパティの値は、スクリーン リーダーを使用して要素をアナウンスする簡潔でわかりやすいテキスト文字列である必要があります。 このプロパティは、内容を理解するか、またはユーザー インターフェイスと対話にとって重要な意味を持つ要素を設定する必要があります。 これには、XAML で次のようにします。

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

また、そのことができますよう設定できます (C#)。

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> なお、 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/)メソッドを使用して設定することもできます、`AutomationProperties.Name`添付プロパティ。 `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText`接続のプロパティは、ユーザー インターフェイス要素を説明するテキストに設定する必要があり、要素に関連付けられたツールヒントのテキストとしての検討することができます。 これには、XAML で次のようにします。

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

また、そのことができますよう設定できます (C#)。

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> なお、 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/)メソッドを使用して設定することもできます、`AutomationProperties.HelpText`添付プロパティ。 `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

一部のプラットフォームでの編集などのコントロール、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)、`HelpText`プロパティはことがありますを省略すると、プレース ホルダー テキストに置き換えられます。 有力候補は、たとえば、「ここアドレスを入力」、 [ `Entry.Placeholder` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/)ユーザーの実際の入力する前に、コントロール内のテキストの配置プロパティです。

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy`添付プロパティが、現在の要素のアクセシビリティに関する情報を定義する別の要素を許可します。 たとえば、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) の横に、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)新機能の記述に使用できる、`Entry`を表します。 これには、XAML で次のようにします。

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

また、そのことができますよう設定できます (C#)。

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> なお、 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/)メソッドを使用して設定することもできます、`AutomationProperties.IsInAccessibleTree`添付プロパティ。 `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>まとめ

この記事は、ユーザー補助機能の値を設定しているユーザーに対するインターフェイス要素 Xamarin.Forms アプリケーションから添付プロパティを使用して、方法を説明した、`AutomationProperties`クラスです。 これらはアタッチできるように、スクリーン リーダーが要素に関する ページで会話できますセット ネイティブ アクセシビリティのプロパティの値です。


## <a name="related-links"></a>関連リンク

- [添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)
- [ユーザー補助 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
