---
title: MacOS のユーザー補助機能
description: このドキュメントでは、Xamarin. Mac アプリで macOS ユーザー補助機能を使用する方法について説明します。 ここでは、ストーリーボードとコード、カスタムコントロール、およびアクセシビリティのテストの UI 要素について説明します。
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 2162fba1275b66167965e90aeade721e08ea9130
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489324"
---
# <a name="accessibility-on-macos"></a>MacOS のユーザー補助機能

このページでは、[ユーザー補助のチェックリスト](~/cross-platform/app-fundamentals/accessibility.md)に従って、MacOS アクセシビリティ api を使用してアプリを構築する方法について説明します。
他のプラットフォーム Api については、 [Android アクセシビリティ](~/android/app-fundamentals/accessibility.md)ページと[iOS アクセシビリティ](~/ios/app-fundamentals/accessibility.md)ページを参照してください。

MacOS でアクセシビリティ Api がどのように機能するかを理解するには (旧称 OS X)、まず[Os x アクセシビリティモデル](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html)を確認します。

## <a name="describing-ui-elements"></a>UI 要素の記述

AppKit は、`NSAccessibility` プロトコルを使用して、ユーザーインターフェイスにアクセスできるようにする Api を公開します。 これには、ボタンの `AccessibilityLabel`の設定など、アクセシビリティプロパティに意味のある値を設定しようとする既定の動作が含まれます。 ラベルは、通常、コントロールまたはビューを説明する1つの単語または短い語句です。

### <a name="storyboard-files"></a>ストーリーボードファイル

Xcode Interface Builder を使用して、ストーリーボードファイルを編集します。
コントロールがデザインサーフェイスで選択されている場合は、 **id インスペクター**でアクセシビリティ情報を編集できます (次のスクリーンショットを参照)。

[![Xcode の Interface Builder にアクセシビリティを追加する](accessibility-images/xcode.png "Xcode の Interface Builder にアクセシビリティを追加する")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>コード

現在、Xamarin は `AccessibilityLabel` setter として公開されていません。  アクセシビリティラベルを設定するには、次のヘルパーメソッドを追加します。

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

このメソッドは、次のようにコードで使用できます。

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

`AccessibilityHelp` プロパティは、コントロールまたはビューの機能について説明するためのものであり、ラベルが十分な情報を提供できない場合にのみ追加する必要があります。 ヘルプテキストは、"ドキュメントを削除する" など、可能な限り短くしておく必要があります。

ユーザーインターフェイス要素の中には、アクセス可能なアクセスに関係のないものがあります (ユーザー補助ラベルとヘルプを持つ入力の横にあるラベルなど)。
このような場合は、スクリーンリーダーやその他のユーザー補助ツールによってこれらのコントロールやビューがスキップされるように `AccessibilityElement = false` を設定します。

Apple では、アクセシビリティラベルとヘルプテキストのベストプラクティスについて説明する[アクセシビリティのガイドライン](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html)を提供しています。

## <a name="custom-controls"></a>カスタム コントロール

必要な追加手順の詳細については、「[アクセス可能なカスタムコントロールに関する](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html)Apple のガイドライン」を参照してください。

## <a name="testing-accessibility"></a>アクセシビリティのテスト

macOS には、ユーザー補助機能をテストするための**アクセシビリティインスペクター**が用意されています。 インスペクターは Xcode に含まれています。

**ユーザー補助機能インスペクター**を初めて起動するときは、ユーザー補助機能を使用してコンピューターを制御するアクセス許可が必要です。

![実行するためのアクセス許可を要求するアクセシビリティインスペクター](accessibility-images/accessibility-inspector-1.png "実行するためのアクセス許可を要求するアクセシビリティインスペクター")

(必要に応じて左下の) 設定画面のロックを解除し、**アクセシビリティインスペクター**を目盛りします。

![アクセシビリティインスペクターを有効にするための設定画面](accessibility-images/accessibility-inspector-2.png "アクセシビリティインスペクターを有効にするための設定画面")

有効にすると、インスペクターは画面の周りを移動できるフローティングウィンドウとして表示されます。 次のスクリーンショットは、サンプルの Mac アプリの横で実行されているインスペクターを示しています。 カーソルがウィンドウ上を移動すると、インスペクターに各コントロールのユーザー補助プロパティがすべて表示されます。

[![実行中のアクセシビリティインスペクターの例](accessibility-images/accessibility-example.png "実行中のアクセシビリティインスペクターの例")](accessibility-images/accessibility-example-large.png#lightbox)

詳細については、 [OS X 用のユーザー補助のテスト](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html)に関するガイドを参照してください。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのアクセシビリティ](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac アクセシビリティ](https://www.apple.com/accessibility/mac/)
