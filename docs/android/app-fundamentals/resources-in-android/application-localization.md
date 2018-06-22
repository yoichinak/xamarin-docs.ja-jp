---
title: アプリケーションのローカリゼーションおよび文字列のリソース
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: cfb127500f919b61788087465700dfed213d5eb2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765855"
---
# <a name="application-localization-and-string-resources"></a>アプリケーションのローカリゼーションおよび文字列のリソース

アプリケーションのローカライズとは、特定の地域またはターゲット ロケールに代替のリソースを提供するのです。 たとえば、さまざまな国では、ローカライズされた言語識別文字列を指定する場合があります。 または色または特定のカルチャに合わせてレイアウトを変更する可能性があります。 Android では、読み込まれて、ソース コードを変更することがなくランタイム時に、デバイスのロケールに対応するリソースを使用します。

たとえば、次の図は、次の 3 つの異なるデバイス ロケールで実行されている同じアプリケーションを示していますが、各ボタンに表示されるテキストは、各デバイスに設定されているロケール固有。

[![次の 3 つの異なるロケールの例](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

この例では、レイアウト ファイルの内容で**Main.axml**は次のようになります。

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

上記の例では、ボタンの文字列から読み込まれたリソース文字列のリソース ID を提供することで。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![3 つの言語のリソース文字列](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![3 つの言語のリソース文字列](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Android アプリをローカライズします。

読み取り、[ローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)のヒントとモバイル アプリのローカライズに関するガイダンス。

[に Android アプリをローカライズ](~/android/app-fundamentals/localization.md)ガイドには、文字列を翻訳および Xamarin.Android を使用してイメージをローカライズする方法のより具体的な例が含まれています。



## <a name="related-links"></a>関連リンク

- [Android アプリをローカライズします。](~/android/app-fundamentals/localization.md)
- [クロスプラット フォームのローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)
