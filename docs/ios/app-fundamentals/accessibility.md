---
title: IOS でのユーザー補助
description: このドキュメントでは、iOS でのユーザー補助について説明し、さまざまなプロパティと機能について説明します。この機能は、アプリケーションをできるだけ多くのユーザーが使用できるようにするために使用できます。
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/18/2016
ms.openlocfilehash: 943cdfaee07bc4fd4ed3273840036055ad40b89a
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70766750"
---
# <a name="accessibility-on-ios"></a>IOS でのユーザー補助

このページでは、[ユーザー補助機能のチェックリスト](~/cross-platform/app-fundamentals/accessibility.md)に従って、IOS アクセシビリティ api を使用してアプリを構築する方法について説明します。
他のプラットフォーム Api については、 [Android のアクセシビリティ](~/android/app-fundamentals/accessibility.md)に[関するページを参照して](~/mac/app-fundamentals/accessibility.md)ください。

## <a name="describing-ui-elements"></a>UI 要素の記述

iOS には、VoiceOver スクリーンリーダーがコントロールをより使いやすくするために使用できる説明文を追加するために、開発者が `AccessibilityLabel` と `AccessibilityHint` のプロパティが用意されています。 コントロールには、アクセス可能なモードで追加のコンテキストを提供する1つ以上の特徴をタグ付けすることもできます。

一部のコントロールは、アクセスする必要がない場合があります (たとえば、テキスト入力のラベルや、純粋に装飾されているイメージの場合など)。このような場合、ユーザー補助を無効にするために `IsAccessibilityElement` が用意されています。

**UI デザイナー**

**Properties Pad**には、IOS UI デザイナーでコントロールを選択したときにこれらの設定を編集できるようにするアクセシビリティのセクションが含まれています。

![](accessibility-images/ios-designer-sml.png "Accessibility Settings")

**C#**

これらのプロパティは、コードで直接設定することもできます。

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>AccessibilityIdentifier とは

@No__t_0 は、UIAutomation API を使用してユーザーインターフェイス要素を参照するために使用できる一意のキーを設定するために使用されます。

@No__t_0 の値は、ユーザーに話されることも表示されることもありません。

<a name="postnotification" />

## <a name="postnotification"></a>事後通知

@No__t_0 メソッドを使用すると、ユーザーが直接やり取りすることなくユーザーにイベントを発生させることができます (たとえば、特定のコントロールと対話する場合など)。

### <a name="announcement"></a>アナウンス

通知は、何らかの状態が変更されたことをユーザーに通知するために、コードから送信できます (バックグラウンド操作の完了など)。 これには、ユーザーインターフェイスでの視覚的な表示が伴います。

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

@No__t_0 アナウンスは、画面レイアウトの場合に使用されます。

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```

## <a name="accessibility-and-localization"></a>アクセシビリティとローカライズ

ラベルやヒントなどのアクセシビリティプロパティは、ユーザーインターフェイス内の他のテキストと同様にローカライズできます。

**Mainstoryboard.storyboard ファイル**

ユーザーインターフェイスがストーリーボードにレイアウトされている場合は、他のプロパティと同じ方法でアクセシビリティプロパティの翻訳を提供できます。 次の例では、`UITextField` に `Pqa-aa-ury` の**ローカライズ ID**と、スペイン語で設定されているアクセシビリティプロパティが2つあります。

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

このファイルは、スペイン語のコンテンツの**es. lproj**ディレクトリに配置されます。

**ローカライズ可能な文字列**

また、ローカライズされたコンテンツディレクトリ内のローカライズ可能な**文字列**ファイルに翻訳を追加することもできます ( ドイツ語の**lproj** ):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

これらの変換は、`LocalizedString` C#メソッドを使用してで使用できます。

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

コンテンツのローカライズの詳細については、 [iOS のローカリゼーションガイド](~/ios/app-fundamentals/localization/index.md)を参照してください。

<a name="testing" />

## <a name="testing-accessibility"></a>アクセシビリティのテスト

VoiceOver**は、** **全般 > アクセシビリティ > VoiceOver**:

![](accessibility-images/settings-sml.png "Setting the speaking rate")

また、 **[ユーザー補助]** 画面では、ズーム、テキストサイズ、色 & コントラストオプション、音声設定、およびその他の構成オプションの設定も提供されています。

IOS デバイスでアクセシビリティをテストするには、次の[VoiceOver の手順](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html)に従います。

## <a name="simulator-testing"></a>シミュレーターテスト

シミュレーターでテストする場合、アクセシビリティ**インスペクター**を使用して、ユーザー補助のプロパティとイベントが正しく構成されていることを確認できます。 [全般] > [アクセシビリティ] **> [アクセシビリティ**] [インスペクター] の順に移動して、**設定**アプリのインスペクターをオンにします。

![](accessibility-images/settings-inspector-sml.png "Enable Accessibility Inspector")

有効にすると、[インスペクター] ウィンドウが常に [iOS] 画面に表示されます。
テーブルビューの行が選択されたときの出力の例を次に示します。**ラベル**には、行の内容を示す文と、それが "done" であることも示されています (つまり、ティックは表示されています)。

![](accessibility-images/tableview-a11y-sml.png "Using Accessibility Inspector")

インスペクターが表示されている間、左上にある "X" アイコンを使用して、オーバーレイを一時的に表示および非表示にしたり、ユーザー補助の設定を有効または無効にしたりします。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのアクセシビリティ](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS アクセシビリティ (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
