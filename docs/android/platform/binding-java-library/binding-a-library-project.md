---
title: Eclipse ライブラリ プロジェクトのバインド
description: このチュートリアルでは、Xamarin のプロジェクトテンプレートを使用して Eclipse Android ライブラリプロジェクトをバインドする方法について説明します。
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 2ac402bf423c9f3fe136d1ba31622d915d2e2eef
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027738"
---
# <a name="binding-an-eclipse-library-project"></a>Eclipse ライブラリ プロジェクトのバインド

_このチュートリアルでは、Xamarin のプロジェクトテンプレートを使用して Eclipse Android ライブラリプロジェクトをバインドする方法について説明します。_

## <a name="overview"></a>概要

が.Android ライブラリの配布で AAR ファイルが標準になりつつあります。場合によっては、 *android ライブラリプロジェクト*のバインドを作成する必要があります。 Android ライブラリプロジェクトは、共有可能なコードと、Android アプリケーションプロジェクトで参照できるリソースを含む特別な Android プロジェクトです。 通常、Eclipse IDE でライブラリを作成するときに、Android ライブラリプロジェクトにバインドします。
このチュートリアルでは、Android ライブラリプロジェクトを作成する方法の例を示します。Eclipse プロジェクトのディレクトリ構造からの ZIP。

Android ライブラリプロジェクトは、APK にコンパイルされず、独自にデバイスに展開できないということで、通常の Android プロジェクトとは異なります。 代わりに、android ライブラリプロジェクトは、Android アプリケーションプロジェクトによって参照されることを意図しています。 Android アプリケーションプロジェクトをビルドすると、Android ライブラリプロジェクトが最初にコンパイルされます。 その後、Android アプリケーションプロジェクトがコンパイル済みの Android ライブラリプロジェクトに追加され、配布用の APK にコードとリソースが含まれます。 この違いにより、Android ライブラリプロジェクトのバインドの作成は、Java 用のバインディングの作成とは少し異なります。JAR または。AAR ファイル。

## <a name="walkthrough"></a>チュートリアル

Xamarin. Android Java バインドプロジェクトで Android ライブラリプロジェクトを使用するには、まず、Eclipse で Android ライブラリプロジェクトをビルドする必要があります。 次のスクリーンショットは、コンパイル後の1つの Android ライブラリプロジェクトの例を示しています。 

[![Eclipse でのライブラリプロジェクトの例](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Android ライブラリプロジェクトのソースコードが一時にコンパイルされていることに注意してください。**Android-mapviewballoons**という名前の jar ファイル。 **bin/res/高速処理**フォルダーにリソースがコピーされています。 

Android ライブラリプロジェクトが Eclipse でコンパイルされると、Xamarin Android Java Binding プロジェクトを使用してバインドできます。 最初の a。Android ライブラリプロジェクトの**bin**フォルダーと**res**フォルダーを含む ZIP ファイルを作成する必要があります。 中間の**高速処理**サブディレクトリを削除して、リソースが**bin/res**に存在するようにすることが重要です。次のスクリーンショットは、そのような内容を示しています。ZIP ファイル: 

[Android ライブラリプロジェクト .zip の![の内容](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

これ。次のスクリーンショットに示すように、ZIP ファイルが Xamarin Android Java バインドプロジェクトに追加されます。

[Java バインドプロジェクトに追加された![Zip](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

のビルドアクションに注意してください。ZIP ファイルは自動的に**Libraryprojectzip**に設定されています。

存在する場合はです。Android ライブラリプロジェクトに必要な JAR ファイルは、Java バインドライブラリプロジェクトの**jar**フォルダーに追加し、**ビルドアクション**を**referencejar**に設定する必要があります。 この例については、次のスクリーンショットを参照してください。 

[![ビルドアクションが ReferenceJar に設定される](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

これらの手順が完了したら、このドキュメントで既に説明したように、Xamarin Java Binding プロジェクトを使用できます。

> [!NOTE]
> 現時点では、他の Ide での Android ライブラリプロジェクトのコンパイルはサポートされていません。 他の Ide では、 **bin**フォルダー内に Eclipse と同じディレクトリ構造またはファイルが作成されない場合があります。 

## <a name="summary"></a>まとめ

この記事では、Android ライブラリプロジェクトをバインドするプロセスについて説明します。 Eclipse で Android ライブラリプロジェクトをビルドした後、Android ライブラリプロジェクトの**bin**フォルダーと**res**フォルダーから zip ファイルを作成しました。 次に、この zip を使用して、Xamarin Android Java バインドプロジェクトを作成しています。 
