---
title: Xamarin.Formsローカルデータストレージ
description: 共有コードからファイル処理を実行する方法 Xamarin.Forms 、および SQLite.Net を使用してローカルの SQLite データベースに対するデータの読み取りと書き込みを行う方法について説明します。
ms.prod: xamarin
ms.assetid: A324C247-7DA8-4B14-A813-25F85525E32B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 997a659dd01e410f791af28d1b657055296081c8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127115"
---
# <a name="xamarinforms-local-data-storage"></a>Xamarin.Formsローカルデータストレージ

## <a name="files"></a>[[ファイル]](files.md)

を使用したファイル処理は Xamarin.Forms 、.NET Standard ライブラリのコードを使用するか、埋め込みリソースを使用して実現できます。 この記事では、アプリケーションの共有コードからファイル処理を実行する方法について説明 Xamarin.Forms します。

## <a name="local-databases"></a>[ローカル データベース](databases.md)

Xamarin.Formsは、SQLite データベースエンジンを使用したデータベース駆動型アプリケーションをサポートします。これにより、共有コードでオブジェクトを読み込んで保存できるようになります。 この記事で Xamarin.Forms は、SQLite.Net を使用して、アプリケーションがローカルの SQLite データベースに対してデータの読み取りと書き込みを行う方法について説明します。
