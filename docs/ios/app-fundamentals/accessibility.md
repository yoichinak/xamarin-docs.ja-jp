---
title: IOS でユーザー補助
description: このドキュメントでは、さまざまなプロパティと使用できるアプリケーションを使用できるようにする多くのユーザーによって可能な機能について説明する、iOS のユーザー補助機能について説明します。
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/18/2016
ms.openlocfilehash: aa3e15797ae1dac621ea8a78345044be1387ebaa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61179607"
---
# <a name="accessibility-on-ios"></a>IOS でユーザー補助

このページは、iOS ユーザー補助の Api を使用して、に従ってアプリを構築する方法をについて説明します、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)します。
参照してください、 [Android アクセシビリティ](~/android/app-fundamentals/accessibility.md)と[OS X アクセシビリティ](~/mac/app-fundamentals/accessibility.md)他のプラットフォーム Api のページ。

## <a name="describing-ui-elements"></a>UI 要素を記述します。

iOS の提供、`AccessibilityLabel`と`AccessibilityHint`VoiceOver で使用できるわかりやすいテキストを追加する開発者向けプロパティ画面、コントロールをよりアクセスできるようにするリーダー。 コントロールは、アクセス可能なモードで追加のコンテキストを提供する 1 つまたは複数の特徴でもタグ付けすることができます。

一部のコントロールは、(例、入力テキストまたは純粋に装飾されるイメージのラベル) – にアクセスできるようにする必要はありません、`IsAccessibilityElement`そのような場合のユーザー補助機能を無効にすることはできます。

**UI デザイナー**

**Properties Pad**これらの設定を iOS Designer の UI のコントロールが選択されたときに編集できるユーザー補助セクションが含まれています。

![](accessibility-images/ios-designer-sml.png "ユーザー補助の設定")

**C#**

これらのプロパティは、コード内で直接設定することもできます。

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>AccessibilityIdentifier とは何ですか。

`AccessibilityIdentifier` UIAutomation API を使用してユーザー インターフェイス要素を指すために使用できる一意のキーを設定するために使用します。

値`AccessibilityIdentifier`話されるか、ユーザーに表示されることはありませんが。

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

`UIAccessibility.PostNotification`メソッドにより、外部 (たとえば、特定のコントロールとやり取りしている場合) の直接の対話でユーザーに発生するイベントです。

### <a name="announcement"></a>アナウンス

ユーザー (バック グラウンド操作が完了すると)、いくつかの状態が変更されたことを通知するためにコードからアナウンスを送信できます。 ユーザー インターフェイスを視覚これと共に使用する可能性があります。

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

`LayoutChanged`お知らせが使用されるときに画面のレイアウト。

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>アクセシビリティとローカライズ

ユーザー インターフェイスの他のテキストのようなアクセシビリティのプロパティなど、ラベルとヒントを単にローカライズすることができます。

**MainStoryboard.strings**

ユーザー インターフェイスは、ストーリー ボードのレイアウトは、その他のプロパティと同じ方法でアクセシビリティのプロパティの翻訳を行うことができます。 次の例で、`UITextField`が、**ローカリゼーション ID**の`Pqa-aa-ury`とスペイン語に設定されている 2 つのアクセシビリティのプロパティ。

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

このファイルを配置すると、 **es.lproj**スペイン語のコンテンツのディレクトリ。

**Localizable.strings**

または、翻訳に追加できる、 **Localizable.strings**ローカライズされたコンテンツ ディレクトリ (例: ファイル **es.lproj**スペイン語用)。

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

これらの変換で使用できるC#を使用して、`LocalizedString`メソッド。

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

参照してください、 [iOS ローカリゼーション ガイド](~/ios/app-fundamentals/localization/index.md)コンテンツ ローカライズの詳細についてはします。

<a name="testing" />

## <a name="testing-accessibility"></a>アクセシビリティのテスト

ボイスが有効になっている、**設定**アプリに移動して**全般 > アクセシビリティ > VoiceOver**:

![](accessibility-images/settings-sml.png "読み上げ速度の設定")

**アクセシビリティ**画面には、ズーム、テキストのサイズ、およびコントラストの色のオプション、音声認識の設定、およびその他の構成オプションの設定も用意されています。

手順に従います。 [VoiceOver 指示](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html)iOS デバイスでのアクセシビリティをテストします。


## <a name="simulator-testing"></a>シミュレーター テスト

シミュレーターでテストするときに、**アクセシビリティ インスペクター**はアクセシビリティのプロパティとイベントが正しく構成されているかを確認するために使用できます。 インスペクターで有効にする、**設定**アプリに移動して**全般 > アクセシビリティ > アクセシビリティ インスペクター**:

![](accessibility-images/settings-inspector-sml.png "ユーザー補助 Inspector を有効にします。")

有効にすると、続いてインスペクター ウィンドウを常に、iOS 画面経由で配置します。
テーブル ビューの行を選択すると、出力の例を次に示します – に注意してください、**ラベル**行の「完了」コンテンツは、文が含まれています (つまり。 目盛りが表示される)。

![](accessibility-images/tableview-a11y-sml.png "ユーザー補助 Inspector を使用します。")

インスペクターが表示されているときに、左上に"X"アイコンを使用して、一時的に表示し、オーバーレイを非表示、およびユーザー補助の設定を有効または無効にします。



## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのアクセシビリティ](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS (Apple) のユーザー補助機能](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
