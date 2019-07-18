---
title: Eclipse ライブラリ プロジェクトのバインド
description: このチュートリアルでは、Xamarin.Android プロジェクト テンプレートを使用して、Eclipse Android ライブラリ プロジェクトをバインドする方法について説明します。
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a5887748eda8af89b9215e3e121db592dfa73935
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60957657"
---
# <a name="binding-an-eclipse-library-project"></a>Eclipse ライブラリ プロジェクトのバインド

_このチュートリアルでは、Xamarin.Android プロジェクト テンプレートを使用して、Eclipse Android ライブラリ プロジェクトをバインドする方法について説明します。_


## <a name="overview"></a>概要

ですが。AAR ファイルには、Android ライブラリの配布の norm がますますのバインドを作成する必要は場合によっては、 *Android ライブラリ プロジェクト*します。 Android ライブラリ プロジェクトでは、共有可能なコードが含まれている特別な Android プロジェクトと Android アプリケーション プロジェクトで参照可能なリソースが。 通常にバインドする Android ライブラリ プロジェクトを Eclipse IDE で、ライブラリが作成されたとき。
このチュートリアルでは、Android ライブラリ プロジェクトを作成する方法の例を示します。Eclipse のプロジェクトのディレクトリ構造から zip 圧縮します。

Android ライブラリ プロジェクトは、APK にコンパイルされないおよび自身では、通常の Android プロジェクトとさまざまなデバイスに展開します。 代わりに、Android ライブラリ プロジェクトを Android アプリケーション プロジェクトで参照されるものでは。 Android アプリケーション プロジェクトをビルドするときは、まず Android ライブラリ プロジェクトがコンパイルされます。 Android アプリケーション プロジェクトは、コンパイル済みの Android ライブラリ プロジェクトに吸収され、コードとリソースを配布用 APK に含めます。 この違いにより、Android ライブラリ プロジェクトのバインドの作成は Java のバインドを作成するよりも若干異なります。JAR またはします。AAR ファイルです。



## <a name="walkthrough"></a>チュートリアル

Java バインドの Xamarin.Android プロジェクトで、Android ライブラリ プロジェクトを使用するには、Eclipse での Android ライブラリ プロジェクトをビルドする最初の必要になります。 次のスクリーン ショットは、コンパイル後に、Android ライブラリ プロジェクトを 1 つの例を示します。 

[![Eclipse でのライブラリ プロジェクトの例](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

一時的に、Android ライブラリ プロジェクトからソース コードがコンパイルされたことに注意してください。という名前の JAR ファイル**android mapviewballoons.jar**、リソースにコピーされていると、 **bin/res/crunch**フォルダー。 

Eclipse で、Android ライブラリ プロジェクトをコンパイルすると、Xamarin.Android Java バインド プロジェクトを使用し、バインドできます。 最初、します。含む ZIP ファイルを作成する必要があります、 **bin**と**res** Android ライブラリ プロジェクトのフォルダー。 介在するを削除することが重要**crunch**サブディレクトリ内のリソースがあるように**bin/res**。次のスクリーン ショットの内容など。ZIP ファイル: 

[![Android ライブラリ プロジェクトの .zip の内容](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

これ。次のスクリーン ショットに示すように、ZIP ファイルがプロジェクトの Xamarin.Android Java バインドに追加されます。

[![Java バインド プロジェクトに追加する zip](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

注意のビルド アクション、します。ZIP ファイルは自動的に設定されている**LibraryProjectZip**します。

存在する場合。Android ライブラリ プロジェクトで必要な JAR ファイルを追加する場所を**Jars** Java バインド ライブラリ プロジェクトのフォルダーと**ビルド アクション**に設定**ReferenceJar**. この例は、次のスクリーン ショットで確認できます。 

[![ビルド アクション ReferenceJar に設定](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

次の手順が完了したら、このドキュメントで前述の説明に従って Xamarin.Android Java バインド プロジェクトが使用できます。

> [!NOTE]
> この時点では、他の Ide で Android ライブラリ プロジェクトをコンパイルすることはできません。 他の Ide では、同じディレクトリ構造や内のファイルは作成可能性があります、 **bin** Eclipse とフォルダー。 


## <a name="summary"></a>まとめ

この記事には、Android ライブラリ プロジェクトをバインドするプロセスを説明しました。 Eclipse で、Android ライブラリ プロジェクトを構築しから zip ファイルを作成した、 **bin**と**res** Android ライブラリ プロジェクトのフォルダー。 次に、この zip を使用して Xamarin.Android Java バインド プロジェクトを作成しました。 

