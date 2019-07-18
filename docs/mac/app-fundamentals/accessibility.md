---
title: MacOS でのアクセシビリティ
description: このドキュメントでは、Xamarin.Mac アプリでの macOS ユーザー補助機能を使用する方法について説明します。 これには、ストーリー ボードとコード、カスタム コントロール、およびアクセシビリティのテストの記述の UI 要素について説明します。
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: fdda52309ffdb0d32cc42a4dff052cd9050b1e4f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378479"
---
# <a name="accessibility-on-macos"></a>MacOS でのアクセシビリティ

このページが macOS ユーザー補助 Api を使用して、に従ってアプリを構築する方法について説明します、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)します。
参照してください、 [Android アクセシビリティ](~/android/app-fundamentals/accessibility.md)と[iOS アクセシビリティ](~/ios/app-fundamentals/accessibility.md)他のプラットフォーム Api のページ。

動作を理解するアクセシビリティ Api macOS (OS X と呼ばれる以前) の最初のレビュー、 [OS X のユーザー補助モデル](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html)します。

## <a name="describing-ui-elements"></a>UI 要素を記述します。

AppKit を使用して、`NSAccessibility`ヘルプのユーザー インターフェイスにアクセスできるように Api を公開するプロトコル。 これにより、ボタンの設定などのアクセシビリティのプロパティの意味のある値を設定しようとする既定の動作が含まれます。`AccessibilityLabel`します。 ラベルは、通常、1 つの単語または短い語句が、コントロールまたはビューを記述します。

### <a name="storyboard-files"></a>ストーリー ボード ファイル

Xamarin.Mac では、Xcode の Interface Builder を使用して、ストーリー ボード ファイルを編集します。
編集できるユーザー補助情報、 **Identity inspector** (次のスクリーン ショットで示す) ように、デザイン サーフェイスでコントロールが選択された場合。

[![Xcode の Interface Builder でのユーザー補助機能の追加](accessibility-images/xcode.png "Xcode の Interface Builder でのユーザー補助機能の追加")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>コード

として現在公開されていない Xamarin.Mac `AccessibilityLabel` set アクセス操作子。  アクセシビリティのラベルを設定するのには、次のヘルパー メソッドを追加します。

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

`AccessibilityHelp`プロパティは、コントロールまたはビューの詳細については、ラベルは十分な情報を提供しない場合にのみ追加します。 ヘルプ テキストおく必要があるが、できるだけ短くのドキュメントを例「削除」。

ユーザー インターフェイス要素の一部では、アクセス可能 (など、独自のアクセシビリティのラベルとヘルプを持つ入力の横にあるラベル) に関係ありません。
このような場合は、次のように設定します。`AccessibilityElement = false`できるように、これらのコントロールまたはビューはスクリーン リーダーやその他のユーザー補助ツールによってスキップされます。

Apple から提供されて[ユーザ補助ガイドライン](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html)アクセシビリティのラベルとヘルプ テキストのベスト プラクティスを説明します。

## <a name="custom-controls"></a>カスタム コントロール

Apple を参照してください[アクセス可能なカスタム コントロールのガイドライン](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html)のために必要な追加の手順の詳細について。

## <a name="testing-accessibility"></a>ユーザー補助のテスト

macOS の提供、**アクセシビリティ インスペクター**できますが、ユーザー補助機能をテストします。 インスペクターは、Xcode に含まれています。

これが起動され、最初に、**アクセシビリティ インスペクター**ユーザー補助機能を使用してコンピューターを管理するためのアクセス許可が必要になります。

![実行するアクセス許可を要求するユーザー補助インスペクター](accessibility-images/accessibility-inspector-1.png "アクセシビリティ インスペクターを実行するアクセス許可を要求します。")

ティックと (必要な場合、左下に) [設定] 画面のロックを解除、**アクセシビリティ インスペクター**:

![ユーザー補助 Inspector を有効にする設定画面](accessibility-images/accessibility-inspector-2.png "アクセシビリティ Inspector を有効にする設定画面")

有効にすると、インスペクターは画面内を移動できるフローティング ウィンドウとして表示されます。 次のスクリーン ショットは、サンプルの Mac アプリの横にある実行されているインスペクターを示しています。 ウィンドウの上でカーソルを移動すると、インスペクターの各コントロールのアクセス可能なすべてのプロパティが表示されます。

[![実行中のユーザー補助インスペクターの使用例](accessibility-images/accessibility-example.png "アクセシビリティ インスペクターの例の実行")](accessibility-images/accessibility-example-large.png#lightbox)

詳細については、読み取り、 [OS X のガイドのユーザー補助機能をテスト](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html)します。



## <a name="related-links"></a>関連リンク

- [クロス プラットフォームのユーザー補助機能](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac のユーザー補助機能](https://www.apple.com/accessibility/mac/)
