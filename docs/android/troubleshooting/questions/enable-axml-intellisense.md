---
title: Android .axml ファイルでは、Intellisense を有効にする方法は?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 8278576355299894eab1c7c6d3ceaadb607fe915
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774884"
---
# <a name="how-do-i-enable-intellisense-in-android-axml-files"></a>Android .axml ファイルでは、Intellisense を有効にする方法は?

Visual Studio での android の Intellisense では、2 つの XML スキーマ ファイルを適切に作業が必要です。 これらのファイルを検索するには、このフォルダーに移動します。

`C:\Program Files (x86)\Xamarin Studio\AddIns\MonoDevelop.MonoDroid\schemas`*

* (以前: `C:\Program Files (x86)\MSBuild\Xamarin\Android`)

内部 2 つの .xsd ファイルが表示されます。

1. android-layout-xml.xsd
2. schemas.android.com.apk.res.android.xsd

Visual Studio では、それぞれのフォルダー内の XML スキーマをすべて保存します。

`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Xml\Schemas`

これら 2 つの .xsd ファイルを上記の場所に移動または単に Visual Studio 内でこれらのスキーマを追加することができます。 「を追加できます」を使用して Visual Studio 内のこれらのスキーマ、 **XML > スキーマ > 追加**ダイアログ。






