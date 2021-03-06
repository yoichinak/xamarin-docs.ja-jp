---
title: Android リソース
description: この記事では、Xamarin Android の Android リソースの概念を紹介し、それらの使用方法について説明します。 Android アプリケーションのリソースを使用してアプリケーションのローカリゼーションをサポートする方法と、さまざまな画面サイズや密度を含む複数のデバイスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/01/2018
ms.openlocfilehash: 1ff4d9d896aaa5f290402a49aa4b4bd1f1e00aaf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025053"
---
# <a name="android-resources"></a>Android リソース

_この記事では、Xamarin Android の Android リソースの概念を紹介し、それらの使用方法について説明します。Android アプリケーションのリソースを使用してアプリケーションのローカリゼーションをサポートする方法と、さまざまな画面サイズや密度を含む複数のデバイスを使用する方法について説明します。_

## <a name="overview"></a>概要

Android アプリケーションは、ほとんどの場合、ソースコードではありません。 多くの場合、アプリケーションを構成するファイルは他にも多数あります。ビデオ、画像、フォント、オーディオファイルなどがあります。 これらの非ソースコードファイルは、まとめてリソースと呼ばれ、ビルド処理中に (ソースコードと共に) コンパイルされ、デバイスへの配布とインストールのために APK としてパッケージ化されます。

![パッケージ図](images/packaging-diagram.png)

リソースには、Android アプリケーションにいくつかの利点があります。

- **コード分離**&ndash; は、イメージ、文字列、メニュー、アニメーション、色などからソースコードを分離します。このようなリソースは、をローカライズするときに非常に役立ちます。

- **複数のデバイスを対象**とする &ndash; コードを変更することなく、さまざまなデバイス構成をより簡単にサポートできます。

- **コンパイル時のチェック**&ndash; リソースが静的で、アプリケーションにコンパイルされます。 これにより、コンパイル時にリソースの使用を確認することができます。これは、検索が困難で、修正に時間がかかる場合ではなく、実行時と比較して、リソースの使用を簡単に検出して修正することができます。

新しい Xamarin Android プロジェクトが開始されると、リソースと呼ばれる特別なディレクトリが、いくつかのサブディレクトリと共に作成されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![リソースのフォルダーと内容](images/resources-folder-vs.png)

上の図では、アプリケーションのリソースは、その種類に応じて次のサブディレクトリに編成されています。イメージは、構成済み**のディレクトリに**あります。ビューは、**レイアウト**サブディレクトリなどに表示されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![リソースのフォルダーと内容](images/resources-folder-xs.png)

上の図では、アプリケーションリソースは、種類に応じて次のサブディレクトリに編成されています。イメージは**mipmap**ディレクトリに配置されます。ビューは、**レイアウト**サブディレクトリなどに表示されます。

-----

Xamarin Android アプリケーションでこれらのリソースにアクセスするには、次の2つの方法があります。*プログラムによっ*てコード内で、または特殊な xml 構文を使用して xml で*宣言*します。

これらのリソースは*既定のリソース*と呼ばれ、より具体的な一致が指定されていない限り、すべてのデバイスで使用されます。 また、すべての種類のリソースには、Android が特定のデバイスを対象にするために使用できる*代替リソース*が必要な場合もあります。 たとえば、ユーザーのロケール、画面サイズ、またはデバイスが縦から横に90°回転した場合、リソースを提供することができます。これらの各ケースでは、Android はアプリケーションで使用するためのリソースを読み込みます。開発者による追加のコーディング作業は必要ありません。

代替リソースを指定するには、特定の種類のリソースを保持しているディレクトリの末尾に、*修飾子*と呼ばれる短い文字列を追加します。

たとえば、**リソース/** 描画機能では、ドイツ語のロケールに設定されているデバイスのイメージを指定します。一方、**リソース/ドロウ**コードは、デバイスのイメージをフランス語のロケールに設定します。 別のリソースを提供する例として、次の図に示すように、デバイスのロケールだけを変更して同じアプリケーションを実行しています。

![さまざまなロケールの画面の例](images/localized-screenshots.png)

この記事では、リソースの使用方法について詳しく説明し、次のトピックについて説明します。

- **Android リソースの基礎**&ndash; 既定のリソースをプログラムで使用し、宣言によってイメージやフォントなどのリソースの種類をアプリケーションに追加します。

- アプリケーションのさまざまな画面解像度と密度をサポート &ndash;**デバイス固有の構成**。

- **ローカライズ**&ndash;、アプリケーションを使用できるさまざまなリージョンをサポートするためにリソースを使用します。

## <a name="related-links"></a>関連リンク

- [Android アセットの使用](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [アプリケーションの基礎](https://developer.android.com/guide/topics/fundamentals.html)
- [アプリケーション リソース](https://developer.android.com/guide/topics/resources/index.html)
- [複数の画面のサポート](https://developer.android.com/guide/practices/screens_support.html)
