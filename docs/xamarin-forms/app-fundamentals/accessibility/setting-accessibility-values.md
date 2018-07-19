---
title: ユーザー インターフェイス要素のアクセシビリティ値の設定
description: この記事では、スクリーン リーダーは、ページの要素について発言できるように、AutomationProperties クラスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 576fe34b0536c83825aa39bdd7111143d9474225
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998915"
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>ユーザー インターフェイス要素のアクセシビリティ値の設定

_Xamarin.Forms では、ネイティブのアクセシビリティ値を設定する、AutomationProperties クラスから添付プロパティを使用してユーザー インターフェイス要素に設定するユーザー補助の値を許可します。この記事では、スクリーン リーダーは、ページの要素について発言できるように、AutomationProperties クラスを使用する方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Forms は、次の添付プロパティを使用してユーザー インターフェイス要素に設定する値をユーザー補助機能を使用できます。

- `AutomationProperties.IsInAccessibleTree` – 要素がアクセスできるアプリケーションで使用できるかどうかを示します。 詳細については、次を参照してください。 [AutomationProperties.IsInAccessibleTree](#isinaccessibletree)します。
- `AutomationProperties.Name` – 要素の speakable 識別子として機能する要素の簡単な説明。 詳細については、次を参照してください。 [AutomationProperties.Name](#name)します。
- `AutomationProperties.HelpText` – 要素に関連付けられているツールヒント テキストとして考えることができる要素の詳しい説明。 詳細については、次を参照してください。 [AutomationProperties.HelpText](#helptext)します。
- `AutomationProperties.LabeledBy` – 現在の要素のアクセシビリティに関する情報を定義するもう 1 つの要素を許可します。 詳細については、次を参照してください。 [AutomationProperties.LabeledBy](#labeledby)します。

ネイティブのアクセシビリティ値の設定のプロパティは、スクリーン リーダーが要素について発言できるようにこれらアタッチ。 添付プロパティの詳細については、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)を参照してください。

> [!IMPORTANT]
> 使用して、`AutomationProperties`添付プロパティの Android の UI テストの実行に影響を与える可能性があります。 [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId)、`AutomationProperties.Name`と`AutomationProperties.HelpText`プロパティはどちらも設定ネイティブ`ContentDescription`プロパティで、`AutomationProperties.Name`と`AutomationProperties.HelpText`プロパティ値の優先順位を引き継ぎ、 `AutomationId`値 (両方`AutomationProperties.Name`と`AutomationProperties.HelpText`設定、値が連結されます)。 つまり、すべてのテストを探して`AutomationId`場合は失敗`AutomationProperties.Name`または`AutomationProperties.HelpText`要素に設定されます。 このシナリオでは、値を検索する UI テストを変更する`AutomationProperties.Name`または`AutomationProperties.HelpText`、または両方を連結します。

各プラットフォームでは、アクセシビリティ値のナレーションをさまざまな画面リーダーがあります。

- iOS では、ボイス オーバーがあります。 詳細については、次を参照してください。 [VoiceOver のデバイスでのテスト ユーザー補助](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html)developer.apple.com にします。
- Android では、TalkBack が。 詳細については、次を参照してください。[テスト アプリケーションのユーザー補助](https://developer.android.com/training/accessibility/testing.html#talkback)developer.android.com にします。
- Windows では、ナレーターがあります。 詳細については、次を参照してください。[ナレーターを使用して、メイン アプリケーションのシナリオを確認](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/)します。

ただし、スクリーン リーダーの正確な動作は、ソフトウェアおよびそのユーザーの構成によって異なります。 たとえば、ほとんどのスクリーン リーダー読み取りフォーカスを受け取ったときに、コントロールに関連付けられたテキスト自体、ページ上のコントロール間で移動するためにユーザーを有効にします。 スクリーン リーダーによってアプリケーション全体のユーザー インターフェイスを読み取ることも、ページが表示されたら、それを移動しようとする前にすべてのページの使用可能な情報のコンテンツを受信するユーザーを有効します。

また、スクリーン リーダーは、異なるアクセシビリティ値を読み取る。 サンプル アプリケーション。

- VoiceOver は読み取り、`Placeholder`の値、 `Entry`、その後に、コントロールの使用方法について説明します。
- TalkBack は読み取り、`Placeholder`の値、 `Entry`、その後に、`AutomationProperties.HelpText`コントロールを使用するための手順に続く値。
- ナレーターが読み上げる、`AutomationProperties.LabeledBy`の値、`Entry`コントロールを使用する手順、その後にします。

さらに、ナレーターは優先順位を付ける`AutomationProperties.Name`、 `AutomationProperties.LabeledBy`、し`AutomationProperties.HelpText`します。 Android では、TalkBack を組み合わせることも、`AutomationProperties.Name`と`AutomationProperties.HelpText`値。 そのため、ことを最適なエクスペリエンスを確認するには、各プラットフォームで実施徹底的なアクセシビリティのテストをお勧めします。

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree`添付プロパティは、`boolean`要素にはアクセス可能で、スクリーン リーダーに、そのため表示される場合を決定します。 設定する必要がある`true`添付プロパティの他のユーザー補助機能を使用します。 これで実行できます XAML には、次のようにします。

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

または、設定できます (C#) には、次のように。

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> なお、 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object))メソッドが設定することもでき、`AutomationProperties.IsInAccessibleTree`添付プロパティ。 `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name`添付プロパティの値は、スクリーン リーダーを使用して要素を発表する簡潔でわかりやすいテキスト文字列である必要があります。 このプロパティは、内容を理解、またはユーザー インターフェイスとの対話の重要な意味を持つ要素を設定する必要があります。 これで実行できます XAML には、次のようにします。

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

または、設定できます (C#) には、次のように。

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> なお、 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object))メソッドが設定することもでき、`AutomationProperties.Name`添付プロパティ。 `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText`接続のプロパティは、ユーザー インターフェイス要素を説明するテキストに設定する必要があり、要素に関連付けられたツールヒントのテキストとしての検討することができます。 これで実行できます XAML には、次のようにします。

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

または、設定できます (C#) には、次のように。

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> なお、 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object))メソッドが設定することもでき、`AutomationProperties.HelpText`添付プロパティ。 `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

一部のプラットフォームでの編集などのコントロール、 [ `Entry` ](xref:Xamarin.Forms.Entry)、`HelpText`プロパティはことがありますを省略すると、プレース ホルダー テキストに置き換えられます。 候補は、たとえば、「ここで、名前を入力」、 [ `Entry.Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder)プロパティの前に、ユーザーの実際の入力コントロールにテキストを配置します。

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy`添付プロパティが、現在の要素のアクセシビリティに関する情報を定義するもう 1 つの要素を許可します。 たとえば、 [ `Label` ](xref:Xamarin.Forms.Label) の横に、 [ `Entry` ](xref:Xamarin.Forms.Entry)新機能の記述に使用できる、`Entry`を表します。 これで実行できます XAML には、次のようにします。

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

または、設定できます (C#) には、次のように。

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> なお、 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object))メソッドが設定することもでき、`AutomationProperties.IsInAccessibleTree`添付プロパティ。 `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>まとめ

この記事では、アクセシビリティ値の設定でユーザー インターフェイス要素で Xamarin.Forms アプリケーションでから添付プロパティを使用して、方法を説明した、`AutomationProperties`クラス。 ネイティブのアクセシビリティ値の設定のプロパティは、スクリーン リーダーは、ページの要素について発言できるようにこれらアタッチ。


## <a name="related-links"></a>関連リンク

- [添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)
- [ユーザー補助 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
