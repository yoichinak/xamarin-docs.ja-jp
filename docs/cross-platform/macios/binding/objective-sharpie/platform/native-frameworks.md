---
title: ネイティブ フレームワークのバインド
description: このドキュメントでは、目標マジックペンのフレームワークオプションを使用して、フレームワークとして配布されるライブラリへのバインドを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: davidortinau
ms.author: daortin
ms.date: 01/15/2016
ms.openlocfilehash: 0733e2b406f5032a2e2717df66c96dcb28301f09
ms.sourcegitcommit: 24883be72e485e5311dd0eb91f9a22f78eeec11a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2020
ms.locfileid: "77374129"
---
# <a name="binding-native-frameworks"></a>ネイティブ フレームワークのバインド

ネイティブライブラリが[フレームワーク](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html)として配布される場合があります。 目標マジックペンは、適切に定義されたフレームワークを `-framework` オプションを使用してバインドするための便利な機能を提供します。

たとえば、 [Adobe CREATIVE SDK Framework](https://creativesdk.adobe.com/downloads.html) for iOS をバインドするのは簡単です。

```
$ sharpie bind \
    -framework ./AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1
```

場合によっては、フレームワークがどの SDK をコンパイルする必要があるかを示す**情報**が、フレームワークによって指定されます。 この情報が存在し、明示的な `-sdk` オプションが渡されなかった場合、目標マジックペンは、フレームワークの**情報**(`DTSDKName` キー、または `DTPlatformName` と `DTPlatformVersion` キーの組み合わせ) からそれを推論します。

`-framework` オプションでは、明示的なヘッダーファイルを渡すことはできません。 包括的なヘッダーファイルは、フレームワーク名に基づいて規則によって選択されます。 包括的なヘッダーが見つからない場合、目標マジックペンはフレームワークのバインドを試行せず、解析するための正しい包括ヘッダーファイルと、clang のフレームワーク引数 (`-F` framework 検索パスオプションなど) を指定して、バインドを手動で実行する必要があります。

内部的には、`-framework` を指定するのはショートカットだけです。 次のバインド引数は、上記の `-framework` の短縮形と同じです。
特に重要なのは、clang に提供されている `-F .` framework の検索パスです (コマンドの一部として必要なスペースとピリオドに注意してください)。

```
$ sharpie bind \
    -sdk iphoneos8.1 \
    ./AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .
```
