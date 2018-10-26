---
title: Contentprovider の使用
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: d1ec628de3481820f320a5a8e6ef88fcbaab75a6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122115"
---
# <a name="using-a-contentprovider"></a>Contentprovider の使用

CursorAdapters が、ContentProvider のデータを表示することもできます。
ContentProviders では、他のアプリケーションによって公開されているデータにアクセスできます (連絡先のように Android のシステム データを含むメディアと予定表の情報)。

ContentProvider にアクセスすることをお勧めは、LoaderManager を使用する CursorLoader とは。 LoaderManager はメインのスレッドをブロックしているタスクを移動する Android 3.0 (API レベル 11、Honeycomb) で導入され、CursorLoader を使う前に、表示するための ListView にバインドされているスレッドに読み込まれるデータ。

参照してください[ContentProviders 入門](~/android/platform/content-providers/index.md)詳細についてはします。

