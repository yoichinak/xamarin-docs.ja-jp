---
title: "Android のリソース"
description: "この記事では、Xamarin.Android Android のリソースの概念を紹介し、その使用方法を文書化されます。 アプリケーションのローカライズ、およびさまざまな画面サイズと密度を含む複数のデバイスをサポートするために Android アプリのリソースを使用する方法を説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 6546870d85f7b77e60dff0cb9e6075f982c9cb8e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="android-resources"></a>Android のリソース

_この記事では、Xamarin.Android Android のリソースの概念を紹介し、その使用方法を文書化されます。アプリケーションのローカライズ、およびさまざまな画面サイズと密度を含む複数のデバイスをサポートするために Android アプリのリソースを使用する方法を説明します。_


## <a name="overview"></a>概要

Android アプリケーションは、ソース コードだけではほとんどありません。 アプリケーションを構成するその他の多くのファイルは多くの場合があります: ビデオ、イメージ、フォント、およびオーディオ ファイルがいくつかの名前を付けるだけです。 集合的に、これら以外のソース コード ファイルのリソースと呼びます (、ソース コードと共に)、ビルド処理中にコンパイルし、は、APK の配布とインストールをデバイスとしてパッケージ化します。

![パッケージのダイアグラム](images/packaging-diagram.png)

リソースは、Android アプリケーションをいくつかの利点を提供します。

-  **コード分離付き**&ndash;イメージ、文字列、メニューのアニメーション、色などのソース コードを分離します。そのためをローカライズするときにかなりリソースに役立ちます。

-  **複数のデバイスを対象に**&ndash;コード変更することがなく別のデバイス構成の簡単なサポートを提供します。

-  **コンパイル時チェック**&ndash;リソースは、静的で、アプリケーションにコンパイルします。 これにより、簡単にキャッチして見つけるが難しく、コストの解決時にランタイムではなく、誤りを修正する場合、コンパイル時にチェックするリソースの使用量。

新しい Xamarin.Android プロジェクトが開始されたときに、いくつかのサブディレクトリと共に、リソースと呼ばれる特別なディレクトリが作成します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![リソースのフォルダーとその内容](images/resources-folder-vs.png)

これらのサブディレクトリに、型に従って、上記の図で、アプリケーションのリソースが構成されています。 イメージは、予定、**ドロウアブル**directory; ビュー移動、**レイアウト**サブディレクトリなどです。
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![リソースのフォルダーとその内容](images/resources-folder-xs.png)

これらのサブディレクトリに、型に従って、上記の図で、アプリケーションのリソースが構成されています。 イメージは、予定、 **mipmap** directory; ビュー移動、**レイアウト**サブディレクトリなどです。
 
-----

Xamarin.Android アプリケーションでこれらのリソースにアクセスする 2 つの方法:*プログラムで*コード内と*宣言によって*特殊な XML 構文を使用して XML にします。

これらのリソースを呼び出す*既定のリソースの*されより具体的な一致が指定されていない限り、すべてのデバイスによって使用されます。 さらに、リソースのすべての種類、必要に応じてできます*代替リソース*Android は特定のデバイスを対象に使えます。 リソースをユーザーのロケール、画面のサイズをターゲットに提供される可能性など、またはデバイスが縦から横に 90 度回転などです。このような場合の各で Android は、アプリケーション開発者によって余分なコード作成作業なしで、使用するためのリソースを読み込みます。

代替のリソースと呼ばれる、短い文字列を追加することによって指定された、*修飾子*、特定の種類のリソースを保持しているディレクトリの末尾にします。

たとえば、**リソース/ドロウアブル de**ドイツ語のロケールに設定されているデバイスのイメージを指定中に**リソース/ドロウアブル fr**を保持するイメージのデバイスがフランス語のロケールに設定します。 デバイスの変更のロケールだけに、同じアプリケーションが実行されている次の図の代替のリソースを提供する例を表示できます。

![別のロケール用の例の画面](images/localized-screenshots.png)

この記事は見て包括的なリソースを使用し、次の内容します。

-  **Android のリソース基礎**&ndash;画像やフォントなどのリソースの種類をアプリケーションに追加するプログラムと宣言、既定のリソースを使用します。

-  **デバイスの特定の構成**&ndash;アプリケーションで、さまざまな画面解像度と密度をサポートします。

-  **ローカリゼーション**&ndash;それぞれの地域にアプリケーションを使用することがありますをサポートするためにリソースを使用します。


## <a name="related-links"></a>関連リンク

- [Android アセットの使用](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [アプリケーションの基礎](http://developer.android.com/guide/topics/fundamentals.html)
- [アプリケーション リソース](http://developer.android.com/guide/topics/resources/index.html)
- [複数の画面をサポートします。](http://developer.android.com/guide/practices/screens_support.html)
