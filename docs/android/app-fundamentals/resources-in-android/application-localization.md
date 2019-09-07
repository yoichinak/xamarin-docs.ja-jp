---
title: アプリケーションのローカライズと文字列リソース
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 45fe5c783e737fb913730082841e0dfafc555684
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755132"
---
# <a name="application-localization-and-string-resources"></a>アプリケーションのローカライズと文字列リソース

アプリケーションのローカライズは、特定の地域またはロケールをターゲットとする代替リソースを提供する機能です。 たとえば、さまざまな国にローカライズされた言語の文字列を指定したり、特定のカルチャに合わせて色やレイアウトを変更したりすることができます。 Android では、ソースコードを変更せずに、実行時にデバイスのロケールに適したリソースが読み込まれ、使用されます。

たとえば、次の図は、3つの異なるデバイスロケールで実行されている同じアプリケーションを示していますが、各ボタンに表示されるテキストは、各デバイスが設定されているロケールに固有のものです。

[![3つの異なるロケールの例](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

この例では、レイアウトファイルのメインの内容は次のように**なります**。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent"
   >
<Button  
   android:id="@+id/myButton"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
android:text="@string/hello"
   />
</LinearLayout>
```

上の例では、ボタンの文字列は、文字列のリソース ID を指定することによって、リソースから読み込まれました。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![3つの言語のリソース文字列](application-localization-images/02-resource-strings-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![3つの言語のリソース文字列](application-localization-images/02-resource-strings-xs.png)

-----

## <a name="localizing-android-apps"></a>Android アプリのローカライズ

詳細については、「[ローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)」を参照してください。

「 [Android アプリのローカライズ](~/android/app-fundamentals/localization.md)」ガイドでは、Xamarin android を使用して文字列を翻訳し、イメージをローカライズする方法について、より具体的な例を紹介しています。

## <a name="related-links"></a>関連リンク

- [Android アプリのローカライズ](~/android/app-fundamentals/localization.md)
- [クロスプラットフォームのローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)
