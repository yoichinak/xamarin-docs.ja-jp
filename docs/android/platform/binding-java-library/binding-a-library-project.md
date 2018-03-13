---
title: "Eclipse ライブラリ プロジェクトのバインド"
description: "このチュートリアルでは、Eclipse Android ライブラリ プロジェクトをバインドする Xamarin.Android プロジェクト テンプレートを使用する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 7b1314c12bf97a2fa21911c747e3066858116a5f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="binding-an-eclipse-library-project"></a>Eclipse ライブラリ プロジェクトのバインド

_このチュートリアルでは、Eclipse Android ライブラリ プロジェクトをバインドする Xamarin.Android プロジェクト テンプレートを使用する方法について説明します。_


## <a name="overview"></a>概要

します。AAR ファイルには、標準の Android ライブラリ配布がますます、場合によってはのバインドを作成するために必要な*Android ライブラリ プロジェクト*です。 Android ライブラリ プロジェクトとは、共有可能なコードが含まれる特殊な Android プロジェクトと Android アプリケーション プロジェクトで参照可能なリソースです。 通常、バインドする Android ライブラリ プロジェクトに Eclipse IDE で、ライブラリの作成時にします。
このチュートリアルでは、Android ライブラリ プロジェクトを作成する方法の例を示します。Eclipse のプロジェクトのディレクトリ構造から zip 圧縮します。

Android ライブラリ プロジェクトとは、APK にコンパイルはされないおよび自身で行われていない、正規の Android プロジェクトを異なるデバイスに展開します。 代わりに、Android ライブラリ プロジェクトは、Android アプリケーション プロジェクトが参照するものです。 Android アプリケーション プロジェクトをビルドするときに、Android ライブラリ プロジェクトが最初にコンパイルされます。 Android アプリケーション プロジェクトをコンパイル済みの Android ライブラリ プロジェクトに吸収するし、配布用 APK にコードおよびリソースを追加します。 この違いにより、Android ライブラリ プロジェクトのバインドの作成は Java のバインドを作成するよりも若干異なります。JAR またはします。[Aar] ファイルです。



## <a name="walkthrough"></a>チュートリアル

Xamarin.Android Java バインドのプロジェクトでの Android ライブラリ プロジェクトを使用するには、Eclipse での Android ライブラリ プロジェクトをビルドする最初の必要を勧めします。 次のスクリーン ショットは、コンパイル後に 1 つの Android ライブラリ プロジェクトの例を示します。 

[![Eclipse での使用例ライブラリ プロジェクト](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

一時的なに Android ライブラリ プロジェクトからソース コードがコンパイルされたことに注意してください。という名前の JAR ファイル**android mapviewballoons.jar**、リソースにコピーされていることと、 **bin/res/破棄**フォルダーです。 

Eclipse の Android ライブラリ プロジェクトをコンパイルした後、Xamarin.Android Java バインド プロジェクトを使用し、バインドできます。 最初、します。含む ZIP ファイルを作成する必要があります、 **bin**と**res** Android ライブラリ プロジェクトのフォルダーです。 介在するを削除することが重要**破棄**サブディレクトリ内のリソースがあるように**bin/res**です。次のスクリーン ショットを示していますの内容は、このようなです。ZIP ファイル: 

[![Android ライブラリ プロジェクト .zip の内容](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

これ。次のスクリーン ショットに示すように、ZIP ファイルは Xamarin.Android Java バインドのプロジェクトに追加されます。

[![Zip Java バインド プロジェクトに追加](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

注意して、ビルド アクションが、します。ZIP ファイルを自動的に設定されている**LibraryProjectZip**です。

存在する場合。Android ライブラリ プロジェクトに必要な JAR ファイルを追加するのには**Jar** Java バインド ライブラリ プロジェクトのフォルダーと**ビルド アクション**に設定**ReferenceJar**. この例は、次のスクリーン ショットに表示できます。 

[![ビルド アクション ReferenceJar に設定](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

これらの手順が完了した後は、このドキュメントで前述の説明に従って Xamarin.Android Java バインド プロジェクトが使用できます。

> [!NOTE]
> この時点では、他の Ide での Android ライブラリ プロジェクトのコンパイルはサポートされていません。 他の Ide では、同じディレクトリ構造や内のファイルは作成可能性があります、 **bin** Eclipse とフォルダー。 


## <a name="summary"></a>まとめ

この記事ではとおし、Android ライブラリ プロジェクトをバインドするプロセスです。 Eclipse での Android ライブラリ プロジェクトを作成したからの zip ファイルを作成した、 **bin**と**res** Android ライブラリ プロジェクトのフォルダーです。 次に、この zip を使用して Xamarin.Android Java バインド プロジェクトを作成しました。 

