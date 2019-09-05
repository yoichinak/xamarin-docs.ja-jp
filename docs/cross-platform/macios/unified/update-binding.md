---
title: バインドの Unified API への移行
description: この記事では、Xamarin および Xamarin アプリケーション用の統合 Api をサポートするために、既存の Xamarin バインドプロジェクトを更新するために必要な手順について説明します。
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: dcee5c6d5324be11f424739a20ba673817553e36
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287376"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>バインドの Unified API への移行

_この記事では、Xamarin および Xamarin アプリケーション用の統合 Api をサポートするために、既存の Xamarin バインドプロジェクトを更新するために必要な手順について説明します。_

## <a name="overview"></a>概要

2月1日以降、2015 Apple では、iTunes および Mac App Store への新規送信がすべて64ビットアプリケーションである必要があります。 そのため、新しい Xamarin. iOS または monotouch.dialog アプリケーションでは、64ビットをサポートするために、既存のクラシックおよびモノ Mac Api ではなく、新しい Unified API を使用する必要があります。

さらに、Xamarin バインドプロジェクトは、64ビットの Xamarin. iOS または Xamarin. Mac プロジェクトに含まれる新しい統合 Api もサポートする必要があります。 この記事では、Unified API を使用するように既存のバインドプロジェクトを更新するために必要な手順について説明します。

## <a name="requirements"></a>必要条件

この記事に記載されている手順を完了するには、次のものが必要です。

- **Visual Studio for Mac** -開発用コンピューターにインストールされ、構成されている Visual Studio for Mac の最新バージョンです。
- **Apple mac** -IOS および Mac 用のバインドプロジェクトをビルドするには、apple mac が必要です。

Windows コンピューターの Visual studio では、プロジェクトのバインドはサポートされていません。

## <a name="modify-the-using-statements"></a>Using ステートメントを変更する

統合された Api を使用すると、Mac と iOS の間でコードを共有できるだけでなく、同じバイナリで32および64ビットアプリケーションをサポートすることができます。 名前空間から_モノ Mac_と_monotouch.dialog_プレフィックスを削除することにより、xamarin と xamarin の両方のアプリケーションプロジェクトでより簡単な共有が実現されます。

そのため`.cs` 、microsoft の`using`ステートメントから、モノの_mac_と_monotouch.dialog_のプレフィックスを削除するには、バインドコントラクト (およびバインドプロジェクト内のその他のファイル) を変更する必要があります。

たとえば、バインドコントラクトで次の using ステートメントを使用したとします。

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

プレフィックスを`MonoTouch`削除すると、次のような結果になります。

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

ここでも、バインドプロジェクト内の任意`.cs`のファイルに対してこれを行う必要があります。 この変更が行われたら、次の手順では、新しいネイティブデータ型を使用するようにバインドプロジェクトを更新します。

Unified API の詳細については、 [Unified API](~/cross-platform/macios/unified/index.md)のドキュメントを参照してください。 32および64ビットアプリケーションのサポートとフレームワークに関する情報の背景については、 [32 および64ビットプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md)に関するドキュメントを参照してください。

## <a name="update-to-native-data-types"></a>ネイティブデータ型への更新

目標 C は、 `NSInteger`データ`int32_t`型を`int64_t` 32 ビットシステム上のと64ビットシステムにマップします。 この動作を一致させるために、新しい Unified API は、 `int`の以前の使用 (.net では、 `System.Int32`常にとして定義さ`System.nint`れている) を新しいデータ型に置き換えます。

新しい`nint`データ型と共に、型および型へ`nuint` `NSUInteger` `CGFloat`の`nfloat`マッピングのために、Unified API には型と型が導入されています。

前に説明したように、API を確認し、のすべての`NSInteger`インスタンス`NSUInteger`と`CGFloat` 、以前に`uint` `int` `float`マップしたのインスタンスと、新しい`nint`に更新する必要があります。`nuint`型と`nfloat`型。

たとえば、という目的の C メソッド定義を指定した場合、次のようになります。

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

前のバインディングコントラクトが次のように定義されている場合:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

新しいバインドを次のように更新します。

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```

最初にリンクしたものより新しいバージョンのサードパーティライブラリにマッピングする場合は、 `.h`ライブラリのヘッダーファイルを確認し、 `int`、 `int32_t` `unsigned int`、、 `uint32_t`またはの呼び出しが終了しているかどうかを確認する必要があります。は、 、`NSUInteger` また`CGFloat`はにアップグレードされました。 `NSInteger` `float` その場合は、 `nint`、、 `nuint`および`nfloat`型に対する同じ変更を、マッピングにも加える必要があります。

これらのデータ型の変更の詳細については、[ネイティブ型](~/cross-platform/macios/nativetypes.md)のドキュメントを参照してください。

## <a name="update-the-coregraphics-types"></a>CoreGraphics 型を更新する

で`CoreGraphics`使用されるポイント、サイズ、および四角形のデータ型は、実行されているデバイスに応じて32または64ビットを使用します。 Xamarin が iOS と Mac の api を最初にバインドしたときに、の`System.Drawing`データ型と一致するようになった既存のデータ構造を使用しました (`RectangleF`など)。

64ビットと新しいネイティブデータ型をサポートするための要件があるため、メソッドを呼び出す`CoreGraphic`ときには、既存のコードに対して次の調整を行う必要があります。

- **Cgrect** -浮動`CGRect`小数点`RectangleF`四角形領域を定義するときに、の代わりにを使用します。
- **Cgsize** -浮動`CGSize`小数点`SizeF`サイズ (幅と高さ) を定義するときに、の代わりにを使用します。
- **Cgpoint** -浮動`CGPoint`小数点`PointF`位置 (X 座標と Y 座標) を定義するときに、の代わりにを使用します。

上記のように、API を確認`CGRect`し、以前に`CGSize` `RectangleF` `SizeF` `PointF`バインドされていた、 `CGPoint`またはネイティブ型`CGRect`に変更されたのインスタンスがあることを確認する必要があります。または`CGPoint`直接。 `CGSize`

たとえば、の目的の C 初期化子を指定した場合、次のようになります。

```csharp
- (id)initWithFrame:(CGRect)frame;
```

前のバインディングに次のコードが含まれている場合:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

そのコードを次のように更新します。

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

すべてのコード変更が適用されたので、バインドプロジェクトを変更するか、統合 Api にバインドするようにファイルを作成する必要があります。

## <a name="modify-the-binding-project"></a>バインドプロジェクトを変更する

統合された api を使用するようにバインドプロジェクトを更新する最後の手順として`MakeFile` 、プロジェクトのビルドに使用するを変更するか、Xamarin プロジェクトの種類 (Visual Studio for Mac 内からバインドする場合) を変更して、 _btouch_にバインドするように指示する必要があります。従来の Api ではなく、統合された Api に対して。


### <a name="updating-a-makefile"></a>メイクファイルの更新

メイクファイルを使用して、バインドプロジェクトを Xamarin にビルドする場合。DLL では、 `--new-style`コマンドラインオプションを指定し、の`btouch`代わり`btouch-native`にを呼び出す必要があります。

次`MakeFile`のように指定します。

<!--markdownlint-disable MD010 -->
```makefile
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch


all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```
<!--markdownlint-enable MD010 -->

の呼び出し`btouch`からに切り替える必要が`btouch-native`あるため、次のようにマクロ定義を調整します。

```makefile
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

の呼び出しを更新し、 `btouch`次の`--new-style`ようにオプションを追加します。

<!--markdownlint-disable MD010 -->
```makefile
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```
<!--markdownlint-enable MD010 -->

これで、を通常`MakeFile`どおりに実行して、API の新しい64ビットバージョンをビルドできるようになりました。

### <a name="updating-a-binding-project-type"></a>バインドプロジェクトの種類の更新

Visual Studio for Mac バインドプロジェクトテンプレートを使用して API を構築している場合は、新しい Unified API バージョンのバインドプロジェクトテンプレートに更新する必要があります。 これを行う最も簡単な方法は、新しい Unified API バインドプロジェクトを開始し、既存のすべてのコードと設定をコピーすることです。

次の手順で行います。

1. Visual Studio for Mac を開始します。
2. **ファイル** > の新しいソリューション > の選択.. **.**
3. [新しいソリューション] ダイアログボックスで、[ **ios** > **Unified API** > **ios バインドプロジェクト**] を選択します。 

    [![](update-binding-images/image01new.png "[新しいソリューション] ダイアログボックスで、[iOS/Unified API/iOS バインドプロジェクト] を選択します。")](update-binding-images/image01new.png#lightbox)
4. [新しいプロジェクトの構成] ダイアログで、新しいバインドプロジェクトの**名前**を入力し、 **[OK]** をクリックします。
5. バインドを作成する対象となる、64ビットバージョンの目標 C ライブラリを含めます。
6. 既存の32ビット Classic API バインドプロジェクト ( `ApiDefinition.cs`や`StructsAndEnums.cs`ファイルなど) からソースコードをコピーします。
7. 上記の変更をソースコードファイルに加えます。

これらの変更をすべて適用したら、32ビットバージョンの場合と同様に、API の新しい64ビットバージョンをビルドできます。

## <a name="summary"></a>Summary

この記事では、新しい統合 Api と64ビットデバイスをサポートするために、既存の Xamarin バインドプロジェクトに加える必要がある変更と、API の新しい64ビット互換バージョンを構築するために必要な手順について説明しました。



## <a name="related-links"></a>関連リンク

- [Mac と iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64 ビットプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md)
- [既存の iOS アプリのアップグレード](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://docs.microsoft.com/samples/xamarin/ios-samples/bindingsample/)
