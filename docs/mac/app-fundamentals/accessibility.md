---
title: MacOS でユーザー補助
description: このガイドでは、アクセス可能な Xamarin.Mac アプリケーションを構築するための機能について説明します。
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: ad04e0276c046f133a6f71abb38912343d2d86b6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility-on-macos"></a>MacOS でユーザー補助

このページによるとアプリをビルドする macOS ユーザー補助 Api を使用する方法を説明する、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)です。
参照してください、 [Android アクセシビリティ](~/android/app-fundamentals/accessibility.md)と[iOS アクセシビリティ](~/ios/app-fundamentals/accessibility.md)Api の他のプラットフォーム用のページです。

動作を理解するアクセシビリティ Api (旧称 OS X)、macOS で最初のレビュー、 [OS X ユーザー補助モデル](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html)です。

## <a name="describing-ui-elements"></a>UI 要素について説明します。

AppKit を使用して、`NSAccessibility`のに役立つ、ユーザー インターフェイスにアクセスできるように Api を公開するプロトコル。 これにより、ボタンの設定などのユーザー補助機能のプロパティの意味のある値を設定しようとする既定の動作が含まれます。`AccessibilityLabel`です。 ラベルは、通常、1 つの単語またはコントロールまたはビューを説明する短い語句。

### <a name="storyboard-files"></a>ストーリー ボード ファイル

Xamarin.Mac では、Xcode インターフェイスのビルダーを使用して、ストーリー ボード ファイルを編集します。
ユーザー補助情報で編集できる、 **Identity インスペクター**コントロールを選択すると、デザイン サーフェイスに示すように次のスクリーン ショット)。

[![Xcode のインターフェイスのビルダーでのユーザー補助機能の追加](accessibility-images/xcode.png "Xcode のインターフェイスのビルダーでのユーザー補助機能の追加")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>コード

として現在公開されていない Xamarin.Mac `AccessibilityLabel` set アクセス操作子。  ユーザー補助のラベルを設定する次のヘルパー メソッドを追加します。

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

ように、このメソッドは、コードで使用できます。

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

`AccessibilityHelp`プロパティは、コントロールまたはビューの詳細については、ラベルが十分な情報が提供されない場合にのみ追加するとします。 ヘルプ テキストおく必要があるまだできるだけ短く、ドキュメントを例「削除」にします。

ユーザー インターフェイス要素の一部は、アクセス可能 (など、独自のユーザー補助のラベルとヘルプを持つ入力の横にあるラベル) の関係ではありません。
このような場合は、次のように設定します。`AccessibilityElement = false`できるように、これらのコントロールまたはビューはスクリーン リーダーやその他のユーザー補助ツールによってスキップされます。

Apple 提供[ユーザ補助ガイドライン](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html)アクセシビリティのラベルとヘルプ テキストのベスト プラクティスを説明します。

## <a name="custom-controls"></a>カスタム コントロール

Apple を参照してください[アクセス可能なカスタム コントロールのガイドライン](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html)の詳細については、追加の手順が必要です。

## <a name="testing-accessibility"></a>ユーザー補助のテスト

macOS 提供、**アクセシビリティ インスペクター**できますがユーザー補助機能をテストします。 インスペクターは、Xcode に含まれています。

開始されてから、最初に、**アクセシビリティ インスペクター**ユーザー補助機能を使用してコンピューターを制御するアクセス許可が必要になります。

![実行するアクセス許可を要求するユーザー補助インスペクター](accessibility-images/accessibility-inspector-1.png "アクセシビリティ インスペクターを実行するアクセス許可を要求します。")

(必要な場合、左下に) [設定] 画面および目盛りのロックを解除、**アクセシビリティ インスペクター**:

![ユーザー補助インスペクターを有効にする設定 画面](accessibility-images/accessibility-inspector-2.png "アクセシビリティ インスペクターを有効にする設定 画面")

有効にすると、インスペクターが画面上を移動できるフローティング ウィンドウとして表示されます。 次のスクリーン ショットは、サンプルの Mac アプリケーションの横にある実行されているインスペクターを示しています。 カーソルがウィンドウの上で移動するときに、インスペクターには、各コントロールのすべてのアクセス可能なプロパティが表示されます。

[![例を実行中のユーザー補助インスペクターの](accessibility-images/accessibility-example.png "アクセシビリティ インスペクターの例の実行")](accessibility-images/accessibility-example-large.png#lightbox)

詳細については、読み取り、 [OS X のガイドのユーザー補助機能をテスト](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html)です。



## <a name="related-links"></a>関連リンク

- [クロスプラット フォームのユーザー補助機能](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac のユーザー補助機能](https://www.apple.com/accessibility/mac/)
