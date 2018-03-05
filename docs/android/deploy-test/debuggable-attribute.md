---
title: "デバッグ可能な属性"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: db09ebe29b6c404bac892fd76389faf0b9e03d5b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="debuggable-attribute"></a>デバッグ可能な属性

<a name="Overview" />


デバッグできるようにするために、Android では Java Debug Wire Protocol (JDWP) がサポートされます。 これは、JVM と通信する ADB などのツールを許可するテクノロジです。 JDWP は開発時には重要ですが、アプリケーションを公開する前に無効にする必要があります。

Android アプリケーションで JDWP を `android:debuggable` 属性の値にすることができます。 Xamarin.Android では、この属性を設定する次の方法が提供されます。

1.  `AndroidManifext.xml` ファイルを作成し、そこに `android:debuggable` 属性を設定する。
1.  `[assembly: Application(Debuggable=false)]` のように、`.CS` ファイルに `ApplicationAttribute` を含める。


`AndroidManifest.xml` と `ApplicationAttribute` の両方が存在する場合は、`AndroidManifest.xml` の内容が、`ApplicationAttribute` で指定されたものより優先されます。

`AndroidManifest.xml` と `ApplicationAttribute` のいずれも存在しない場合、`android:debuggable` 属性の既定値は、デバッグ シンボルが生成されているかどうかによって異なります。 デバッグ シンボルが存在する場合は、Xamarin.Android で `android:debuggable` 属性が `true` に設定されます。

`android:debuggable` 属性の値は、必ずしもビルド構成によって異なるわけではないことに注意してください。 リリース ビルドでは、`android:debuggable` 属性を true に設定することができます。


## <a name="related-links"></a>関連リンク

- [Android マーケットにおけるデバッグ可能なアプリ](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
