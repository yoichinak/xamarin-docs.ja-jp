---
title: バインドのネイティブ フレームワーク
description: このドキュメントの目標ペンを使わずに使用する方法について説明のフレームワークとして配布されるライブラリへのバインドを作成するフレームワーク オプション。
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 02ee21ce58ecf945893f7e4f94763731abe92018
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781446"
---
# <a name="binding-native-frameworks"></a>バインドのネイティブ フレームワーク

としてネイティブ ライブラリを配布することがあります、 [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html)です。 目標ペンを使わずは、便利な機能を介してフレームワークに正しくバインド定義されている、`-framework`オプション。

たとえば、バインディング、 [Adobe クリエイティブな SDK Framework](https://creativesdk.adobe.com/downloads.html) iOS が簡単では。

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

場合によっては、フレームワークを指定します、 **Info.plist**フレームワークどの SDK に対して示しますをコンパイルする必要があります。 この情報が存在する場合と明示的な`-sdk`オプションが渡される、目標ペンを使わずを使用すると、フレームワークから推測されます**Info.plist** (どちらか、`DTSDKName`キーまたはの組み合わせ、`DTPlatformName`と`DTPlatformVersion`キー)。

`-framework`オプションでは、渡される明示的なヘッダー ファイルは許可されていません。 フレームワーク名に基づく規則により、包括的なヘッダー ファイルが選択されます。 包括的なヘッダーが見つからない場合、目標ペンを使わずはフレームワークのバインドを試行しません clang のフレームワーク引数があると共に、解析を正しい包括的なヘッダー ファイルを提供することによって、バインドを手動で実行する必要があります (など、 `-F`フレームワーク検索パスのオプション)。

内部で指定する`-framework`だけのショートカットです。 次のバインド引数と同じ、`-framework`上記の短縮形です。
特別な重要なは、 `-F .` clang に指定されたフレームワーク検索パス (コマンドの一部として必要な領域と期間に注意してください)。

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

