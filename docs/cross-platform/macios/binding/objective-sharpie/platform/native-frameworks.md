---
title: ネイティブ フレームワークのバインド
description: このドキュメントでは、目標マジックペンのフレームワークオプションを使用して、フレームワークとして配布されるライブラリへのバインドを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: conceptdev
ms.author: crdun
ms.date: 01/15/2016
ms.openlocfilehash: 560db570e915cc9bf261f482f03d972973968585
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279038"
---
# <a name="binding-native-frameworks"></a>ネイティブ フレームワークのバインド

ネイティブライブラリが[フレームワーク](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html)として配布される場合があります。 目標マジックペンは、 `-framework`オプションを使用して適切に定義されたフレームワークをバインドするための便利な機能を提供します。

たとえば、 [Adobe CREATIVE SDK Framework](https://creativesdk.adobe.com/downloads.html) for iOS をバインドするのは簡単です。

```
$ sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1
```

場合によっては、フレームワークがどの SDK をコンパイルする必要があるかを示す**情報**が、フレームワークによって指定されます。 この情報が存在し、明示`-sdk`的なオプションが指定されていない場合、目標マジックペンは、フレームワークの**情報** `DTSDKName` ( `DTPlatformVersion`キーまたはキーの`DTPlatformName`組み合わせ) からそれを推論します。

オプション`-framework`では、明示的なヘッダーファイルを渡すことはできません。 包括的なヘッダーファイルは、フレームワーク名に基づいて規則によって選択されます。 包括的なヘッダーが見つからない場合、目標マジックペンはフレームワークのバインドを試行しません。また、解析するための正しい包括ヘッダーファイルと、clang のフレームワーク引数 (例に示す`-F`ように)を指定して、バインドを手動で実行する必要があります。フレームワークの検索パスオプション)。

内部的には、 `-framework`を指定するだけでショートカットができます。 次のバインド引数は、上記の`-framework`短縮形と同じです。
特に重要なの`-F .`は、clang に提供されるフレームワーク検索パスです (コマンドの一部として必要なスペースとピリオドに注意してください)。

```
$ sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .
```
