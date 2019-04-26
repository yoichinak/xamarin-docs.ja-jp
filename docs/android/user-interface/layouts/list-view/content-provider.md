---
title: ContentProvider の使用
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: d1ec628de3481820f320a5a8e6ef88fcbaab75a6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61189738"
---
# <a name="using-a-contentprovider"></a>ContentProvider の使用

CursorAdapters が、ContentProvider のデータを表示することもできます。
ContentProviders では、他のアプリケーションによって公開されているデータにアクセスできます (連絡先のように Android のシステム データを含むメディアと予定表の情報)。

ContentProvider にアクセスすることをお勧めは、LoaderManager を使用する CursorLoader とは。 LoaderManager はメインのスレッドをブロックしているタスクを移動する Android 3.0 (API レベル 11、Honeycomb) で導入され、CursorLoader を使う前に、表示するための ListView にバインドされているスレッドに読み込まれるデータ。

参照してください[ContentProviders 入門](~/android/platform/content-providers/index.md)詳細についてはします。

