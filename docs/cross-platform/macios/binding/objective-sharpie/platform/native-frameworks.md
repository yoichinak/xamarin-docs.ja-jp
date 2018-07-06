---
title: ネイティブ フレームワークのバインド
description: このドキュメントは、目的の油性を使用する方法を説明のフレームワークとして配布されるライブラリへのバインドを作成するフレームワーク オプション。
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: ca103ee027597813be51e126aaa05f9aa969af35
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855098"
---
# <a name="binding-native-frameworks"></a>ネイティブ フレームワークのバインド

としてネイティブ ライブラリを配布することがあります、 [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html)します。 目標油性を通じてフレームワークが定義されているバインドを適切に便利な機能を提供します、`-framework`オプション。

たとえば、バインド、 [Adobe Creative SDK フレームワーク](https://creativesdk.adobe.com/downloads.html)iOS は簡単です。

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

場合によっては、フレームワークを指定します、 **Info.plist**を示すに対してどの SDK フレームワークをコンパイルする必要があります。 この情報が存在する場合と明示的な`-sdk`オプションが渡されると、目的の油性を使用すると、フレームワークから推測されます**Info.plist** (いずれか、`DTSDKName`キーまたはの組み合わせ、`DTPlatformName`と`DTPlatformVersion`キー)。

`-framework`オプションでは、渡される明示的なヘッダー ファイルは許可されていません。 包括的なヘッダー ファイルは、フレームワーク名に基づく規則によって選択されます。 包括的なヘッダーが見つからない場合は、目標油性はフレームワークのバインドを試みません clang の framework 引数があると共に、解析する適切な包括的なヘッダー ファイルを提供することで、バインドを手動で実行する必要があります (など、 `-F`フレームワークの検索パスのオプション)。

内部的には、指定する`-framework`ショートカットだけです。 次のバインド引数と同じですが、`-framework`上記の短縮形。
特別な重要なは、 `-F .` clang に提供するフレームワークの検索パス (コマンドの一部として必要な領域と期間に注意してください)。

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

## <a name="related-links"></a>関連リンク

- [Xamarin University のコース: OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース: 目標油性、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)

