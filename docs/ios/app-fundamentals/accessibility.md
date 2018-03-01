---
title: "IOS でのユーザー補助機能"
ms.topic: article
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/18/2016
ms.openlocfilehash: 28701daca18852be93724ddfe444e1e620788619
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="accessibility-on-ios"></a>IOS でのユーザー補助機能

このページによるとアプリをビルドする iOS ユーザー補助 Api を使用する方法を説明する、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)です。
参照してください、 [Android アクセシビリティ](~/android/app-fundamentals/accessibility.md)と[OS X ユーザー補助](~/mac/app-fundamentals/accessibility.md)Api の他のプラットフォーム用のページです。

## <a name="describing-ui-elements"></a>UI 要素について説明します。

iOS の提供、`AccessibilityLabel`と`AccessibilityHint`開発者は、ボイス オーバーで使用できるわかりやすいテキストを追加のプロパティを画面のコントロールを使いやすくリーダー。 コントロールは、アクセス可能なモードで追加のコンテキストを提供する 1 つまたは複数の特性を持つもタグ付けすることができます。

一部のコントロールが (たとえば、テキストの入力または純粋な装飾的なであるイメージのラベル) – 用アクセスできるようにする必要はありません、`IsAccessibilityElement`はそのような場合のユーザー補助機能を無効にするために提供します。

**デザイナーの UI**

**プロパティ パッド**により、これらの設定を iOS UI デザイナーでコントロールが選択されている場合に編集するユーザー補助セクションが含まれています。

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

`AccessibilityIdentifier` UIAutomation API を使用してユーザー インターフェイス要素を参照するために使用する一意のキーを設定するために使用します。

値`AccessibilityIdentifier`は決して話されるかをユーザーに表示されます。

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

`UIAccessibility.PostNotification`メソッドにより、外部 (たとえば、特定のコントロールを操作する場合) は直接やり取りでユーザーに発生するイベントです。

### <a name="announcement"></a>アナウンス

アナウンスは、コードから (バック グラウンド操作が完了すると) など、いくつかの状態が変更されたことをユーザーに通知を送信できます。 これは、ユーザー インターフェイスに表示が存在する可能性があります。

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

`LayoutChanged`アナウンスが使用されるときに画面のレイアウト。

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>ユーザー補助機能とローカライズ

ユーザー インターフェイスでは、その他のテキストのようなユーザー補助プロパティなど、ラベルとヒントを単にローカライズすることができます。

**MainStoryboard.strings**

ユーザー インターフェイスは、ストーリー ボードのレイアウトは、その他のプロパティと同じ方法でアクセシビリティのプロパティの翻訳を入力できます。 次の例で、`UITextField`が、**ローカリゼーション ID**の`Pqa-aa-ury`とスペイン語に設定されている 2 つのユーザー補助プロパティ。

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

このファイルを配置すると、 **es.lproj**スペイン語のコンテンツのディレクトリ。

**Localizable.strings**

代わりに、翻訳を追加できる、 **Localizable.strings**ディレクトリ内のファイル、ローカライズされたコンテンツ (例です。 **es.lproj**スペイン語用)。

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

(C#) を使用してこれらの変換を使用できます、`LocalizedString`メソッド。

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

参照してください、 [iOS ローカリゼーション ガイド](~/ios/app-fundamentals/localization/index.md)コンテンツ ローカライズの詳細についてはします。

<a name="testing" />

## <a name="testing-accessibility"></a>ユーザー補助のテスト

ボイス オーバーが有効になっている、**設定**に移動してアプリ**全般 > アクセシビリティ > ボイス オーバー**:

![](accessibility-images/settings-sml.png "話し速度の設定")

**アクセシビリティ**画面には、ズーム、テキストのサイズ、およびコントラストの色のオプション、音声認識の設定、およびその他の構成オプションの設定も用意されています。

手順に従います。[ボイス オーバー指示](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html)を iOS デバイスでユーザー補助機能をテストします。


## <a name="simulator-testing"></a>シミュレーターのテスト

シミュレーターでテストするときに、**アクセシビリティ インスペクター**はアクセシビリティのプロパティとイベントが正しく構成されているかを確認するために使用できます。 ときにインスペクターを有効に、**設定**アプリに移動して**全般 > アクセシビリティ > アクセシビリティ インスペクター**:

![](accessibility-images/settings-inspector-sml.png "ユーザー補助インスペクターを有効にします。")

有効にすると、インスペクター ウィンドウを常に iOS の画面上で配置します。
テーブル ビューの行を選択すると、出力の例を次に示します: に注意してください、**ラベル**行の「完了」のコンテンツを提供する文が含まれています (ie。 目盛りは表示)。

![](accessibility-images/tableview-a11y-sml.png "ユーザー補助の設定を使用してください。")

インスペクターが表示されているときに、上部左にある"X"アイコンを使用して、一時的に表示し、オーバーレイを非表示、およびユーザー補助の設定を有効または無効にします。



## <a name="related-links"></a>関連リンク

- [クロスプラット フォームのユーザー補助機能](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS (Apple) ユーザー補助機能](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS ボイス オーバー](http://www.apple.com/accessibility/ios/voiceover/)
