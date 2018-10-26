---
title: アプリケーションのローカライズと文字列リソース
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: d9d90e371199c8587d61199240523cf0a23f5efd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116525"
---
# <a name="application-localization-and-string-resources"></a>アプリケーションのローカライズと文字列リソース

アプリケーションのローカライズは、特定のリージョンまたはロケールを対象とする別のリソースを提供するのです。 たとえば、さまざまな国で、ローカライズされた言語の文字列を提供する可能性があります。 または色または特定のカルチャに合わせてレイアウトを変更する可能性があります。 Android は読み込むし、ランタイム時、ソース コードに何も変更せずに、デバイスのロケールの適切なリソースを使用します。

たとえば、次の図は次の 3 つの別のデバイスのロケールで実行されている同じアプリケーションが、各ボタンに表示されるテキストは各デバイスに設定されているロケールに固有。

[![次の 3 つの異なるロケールの例](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

この例では、レイアウト ファイルの内容で**Main.axml**ようになります。

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

上記の例では、ボタンの文字列は、文字列のリソース ID を提供することで、リソースから読み込まれました。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![3 つの言語のリソース文字列](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![3 つの言語のリソース文字列](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Android アプリをローカライズします。

読み取り、[ローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)のヒントとモバイル アプリのローカライズについて説明します。

[Android アプリのローカライズ](~/android/app-fundamentals/localization.md)ガイドには、文字列の変換し、Xamarin.Android を使用してイメージをローカライズする方法の具体的な例が含まれています。



## <a name="related-links"></a>関連リンク

- [Android アプリをローカライズします。](~/android/app-fundamentals/localization.md)
- [クロスプラット フォームのローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)
