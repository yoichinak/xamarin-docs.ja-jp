---
title: Eclipse ライブラリ プロジェクトのバインド
description: このチュートリアルでは、Xamarin.Android プロジェクト テンプレートを使用して Eclipse Android ライブラリ プロジェクトをバインドする方法について説明します。
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 2ac402bf423c9f3fe136d1ba31622d915d2e2eef
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027738"
---
# <a name="binding-an-eclipse-library-project"></a>Eclipse ライブラリ プロジェクトのバインド

_このチュートリアルでは、Xamarin.Android プロジェクト テンプレートを使用して Eclipse Android ライブラリ プロジェクトをバインドする方法について説明します。_

## <a name="overview"></a>概要

Android ライブラリの配布では .AAR ファイルがますます標準となっていますが、場合によっては、*Android ライブラリ プロジェクト*のバインドを作成する必要があります。 Android ライブラリ プロジェクトは、Android アプリケーション プロジェクトで参照できる共有可能なコードとリソースを含む特別な Android プロジェクトです。 通常、Eclipse IDE でライブラリを作成するときに、Android ライブラリ プロジェクトにバインドします。
このチュートリアルでは、Eclipse プロジェクトのディレクトリ構造から Android ライブラリ プロジェクトの .ZIP を作成する方法について説明します。

Android ライブラリ プロジェクトは、APK にコンパイルされず、それだけではデバイスにデプロイできないという点で、通常の Android プロジェクトとは異なります。 代わりに、Android ライブラリ プロジェクトは、Android アプリケーション プロジェクトによって参照されることが意図されています。 Android アプリケーション プロジェクトをビルドすると、Android ライブラリ プロジェクトが最初にコンパイルされます。 その後、Android アプリケーション プロジェクトがコンパイル済みの Android ライブラリ プロジェクトに追加され、配布用の APK にコードとリソースが含まれます。 この違いにより、Android ライブラリ プロジェクトのバインドの作成は、Java の .JAR または .AAR ファイル用のバインディングの作成とは少し異なります。

## <a name="walkthrough"></a>チュートリアル

Xamarin.Android Java バインド プロジェクトで Android ライブラリ プロジェクトを使用するには、まず、Eclipse で Android ライブラリ プロジェクトをビルドする必要があります。 次のスクリーンショットは、コンパイル後の 1 つの Android ライブラリ プロジェクトの例を示しています。 

[![Eclipse のライブラリ プロジェクトの例](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Android ライブラリ プロジェクトのソース コードが **android-mapviewballoons.jar** という名前の一時的な .JAR ファイルにコンパイルされ、リソースが **bin/res/crunch** フォルダーにコピーされていることに注意してください。 

Android ライブラリ プロジェクトが Eclipse でコンパイルされると、Xamarin.Android Java バインド プロジェクトを使用してバインドできます。 最初に、Android ライブラリ プロジェクトの **bin** と **res** フォルダーを含む ZIP ファイルを作成する必要があります。 **bin/res** にリソースが存在するように、邪魔となる **crunch** サブディレクトリを削除することが重要です。次のスクリーンショットは、そのような ZIP ファイルの内容を示しています。 

[![Android ライブラリ プロジェクトの .zip の内容](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

この .ZIP ファイルはその後、次のスクリーンショットに示すように、Xamarin.Android Java バインド プロジェクトに追加されます。

[![Java バインド プロジェクトに追加された Zip](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

.ZIP ファイルの [ビルド アクション] が自動的に **LibraryProjectZip** に設定されていることに注意してください。

Android ライブラ リプロジェクトに必要な .JAR ファイルが存在する場合は、Java バインド ライブラリ プロジェクトの **Jars** フォルダーに追加し、 **[ビルド アクション]** を **ReferenceJar** に設定する必要があります。 この例については、次のスクリーンショットを参照してください。 

[![ReferenceJar に設定されたビルド アクション](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

これらの手順が完了したら、このドキュメントで既に説明したように、Xamarin.Android Java バインド プロジェクトを使用できます。

> [!NOTE]
> 現時点では、他の IDE での Android ライブラリ プロジェクトのコンパイルはサポートされていません。 他の IDE では、**bin** フォルダー内に Eclipse 同じディレクトリ構造またはファイルが作成されない場合があります。 

## <a name="summary"></a>まとめ

この記事では、Android ライブラリ プロジェクトをバインドするプロセスについて説明しました。 Eclipse で Android ライブラリ プロジェクトをビルドした後、Android ライブラリ プロジェクトの **bin** および **res** フォルダーから zip ファイルを作成しました。 次に、この zip を使用して、Xamarin.Android Java バインド プロジェクトを作成しました。 
