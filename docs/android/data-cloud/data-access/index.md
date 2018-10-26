---
title: Xamarin.Android Data Access
description: ほとんどのアプリケーションでは、デバイスにローカルにデータを保存するには、いくつか要件があります。 データの量が普通に小規模でない限り、通常が必要です、データベースとデータベースへのアクセスを管理するアプリケーションでのデータ層。  Android デバイスには組み込みの SQLite データベース エンジンと Xamarin のプラットフォームでデータを格納および取得のアクセスが簡略化されます。 このドキュメントでは、クロスプラット フォーム対応の方法で SQLite データベースにアクセスする方法を示します。
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 19a2842fa7d29ed40052166b880bf4b26dc09e9c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120698"
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android Data Access

_ほとんどのアプリケーションでは、デバイスにローカルにデータを保存するには、いくつか要件があります。データの量が普通に小規模でない限り、通常が必要です、データベースとデータベースへのアクセスを管理するアプリケーションでのデータ層。Android デバイスには組み込みの SQLite データベース エンジンと Xamarin のプラットフォームでデータを格納および取得のアクセスが簡略化されます。このドキュメントでは、クロスプラット フォーム対応の方法で SQLite データベースにアクセスする方法を示します。_

## <a name="data-access-overview"></a>データ アクセスの概要

ほとんどのアプリケーションでは、デバイスにローカルにデータを保存するには、いくつか要件があります。 データの量が普通に小規模でない限り、通常が必要です、データベースとデータベースへのアクセスを管理するアプリケーションでのデータ層。 Android の両方が「組み込み」Sqlite データベース エンジンと、SQLite のデータ プロバイダーに付属する Xamarin のプラットフォームによって、データへのアクセスが簡略化します。

Xamarin.Android などのデータベース アクセス Api をサポートします。

-  ADO.NET フレームワークです。
-  SQLite NET サード パーティのライブラリ。

このセクションでは、コードの大部分は、クロス プラットフォームの完全には、され、変更しなくても iOS または Android で実行されます。 説明した 2 つのサンプル アプリがあります。

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash;単純なデータ操作が、結果をテキスト コントロールの表示を書き込みます

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash;一覧が表示され、単純なデータ構造の編集作業アプリケーションを小さなデータ操作に統合します。

両方のサンプル ソリューションには、iOS と Android のサンプル アプリケーションのプロジェクトが含まれます。

Xamarin.Forms アプリケーションでは、読み取る[データベースでの作業](~/xamarin-forms/app-fundamentals/databases.md)PCL ライブラリを Xamarin.Forms での SQLite を使用する方法を説明しています。

このセクションのトピックでは、データベース エンジンに SQLite を使用して Xamarin.Android でのデータ アクセスについて説明します。 ADO.NET の構文を使用して「直接」、データベースにアクセスしたり、SQLite.NET ORM を追加し、c# でのデータの操作を実行できます。

2 つのサンプルを確認します。 更新と削除機能をテキスト フィールドには、単純なアプリケーションを含む出力の作成、読み取り、非常に単純なデータ アクセス コードを含む 1 つ。 スレッド処理、および事前設定された SQLite データベースを使用してアプリケーションをシードする方法も説明しました。

クロス プラットフォームのデータ アクセスの他の例を参照してください、 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)ケース スタディ。


## <a name="related-links"></a>関連リンク

- [DataAccess Basic (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [データ アクセスの詳細 (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android のデータのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms のデータ アクセス](~/xamarin-forms/app-fundamentals/databases.md)
