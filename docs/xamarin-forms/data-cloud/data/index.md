---
title: Xamarin.Forms のローカル データ ストレージ
description: SQLite.Net を使用して、ローカルの SQLite データベースにデータを読み書きする方法とファイル共有の Xamarin.Forms コードから処理を実行する方法について説明します。
ms.prod: xamarin
ms.assetid: A324C247-7DA8-4B14-A813-25F85525E32B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 585b55b79046af07e466fb25b33f4c22c2ec5e5b
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659069"
---
# <a name="xamarinforms-local-data-storage"></a>Xamarin.Forms のローカル データ ストレージ

## <a name="filesfilesmd"></a>[ファイル](files.md)

Xamarin.Forms を使ったファイルの処理は、.NET Standard ライブラリにあるコードを使うか、埋め込みリソースを使うことで実現できます。 この記事では、ファイル、Xamarin.Forms アプリケーションで共有されるコードから処理を実行する方法について説明します。

## <a name="local-databasesdatabasesmd"></a>[ローカル データベース](databases.md)

Xamarin.Forms では、SQLite データベース エンジンを使ったデータベース駆動型アプリケーションがサポートされています。これにより、共有コードでのオブジェクトの読み込みと保存が可能になります。 この記事では、Xamarin.Forms アプリケーションで SQLite.Net を使用して、ローカルの SQLite データベースに対してデータの読み取りと書き込みを行う方法について説明します。
