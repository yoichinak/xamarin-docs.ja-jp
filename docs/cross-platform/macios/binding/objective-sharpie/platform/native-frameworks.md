---
title: バインドのネイティブ フレームワーク
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 52b845d9e062eea6292528c5a40a74aa67d8e1b7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
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

