---
title: バインドの Unified API への移行
description: この記事では、Xamarin.IOS および Xamarin.Mac アプリケーションの Unified Api をサポートするために既存の Xamarin バインド プロジェクトの更新に必要な手順について説明します。
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: f081ccda507fe3fe65af0e2fb50841aecd7b3c23
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830466"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>バインドの Unified API への移行

_この記事では、Xamarin.IOS および Xamarin.Mac アプリケーションの Unified Api をサポートするために既存の Xamarin バインド プロジェクトの更新に必要な手順について説明します。_

## <a name="overview"></a>概要

2015 年 2 月 1 日の開始 Apple では、iTunes、Mac App Store への新しい送信は 64 ビット アプリケーションである必要がある必要があります。 その結果、新しい Xamarin.iOS または Xamarin.Mac アプリケーションでは、64 ビットをサポートするために、既存のクラシック MonoTouch と MonoMac Api ではなく新しい Unified API を使用する必要があります。

さらに、Xamarin バインド プロジェクトは、64 ビット Xamarin.iOS、Xamarin.Mac プロジェクトに含まれる新しい Unified Api もサポートする必要があります。 この記事では、Unified API を使用して既存のバインド プロジェクトの更新に必要な手順を説明します。

## <a name="requirements"></a>必要条件

この記事で紹介する手順を完了する、次が必要。

- **Visual Studio for Mac** -Visual Studio for Mac の最新バージョンがインストールされており、開発用コンピューターで構成されています。
- **Apple Mac** - Apple の iOS 用のバインドのプロジェクトをビルドするには mac が必要とファルダ

バインド プロジェクトは Windows マシン上の Visual studio ではサポートされていません。

## <a name="modify-the-using-statements"></a>使用してステートメントを変更します。

Unified Api によりを簡単にこれまでよりも Mac および iOS だけでなく、同じの 32 ビットおよび 64 ビット アプリケーションをサポートするバイナリことができますの間でコードを共有します。 削除して、 _MonoMac_と_MonoTouch_プレフィックス、名前空間から簡単に共有 Xamarin.Mac、Xamarin.iOS アプリケーションのプロジェクト間で実現されます。

バインディング、コントラクトのいずれかを変更する必要がありますが、結果として (およびその他の`.cs`バインド プロジェクト内のファイル) を削除する、 _MonoMac_と_MonoTouch_からプレフィックス、 `using`ステートメント。

たとえば、次を指定されたバインドのコントラクトでステートメントを使用します。

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

取り除くことは、`MonoTouch`プレフィックスは次のようになります。

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

このいずれかに対して行う必要がありますが、もう一度`.cs`バインド プロジェクト ファイル。 この変更は、次の手順は、新しいネイティブ データ型を使用するには、このバインド プロジェクトを更新するは。

Unified API の詳細についてを参照してください、 [Unified API](~/cross-platform/macios/unified/index.md)ドキュメント。 32 ビットおよび 64 ビット アプリケーションとフレームワークに関する情報をサポートしている詳しい背景情報についてを参照してください。、 [32 ビットおよび 64 ビット プラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md)ドキュメント。

## <a name="update-to-native-data-types"></a>ネイティブ データ型への更新

Objective C のマップ、`NSInteger`のデータ型`int32_t`32 ビット システムでと`int64_t`64 ビット システムでします。 この動作が一致する新しい Unified API には、前に使用が置き換えられます`int`(常として定義されている .net `System.Int32`) に新しいデータ型:`System.nint`します。

新しいと共に`nint`データ型の場合は、Unified API が導入されています、`nuint`と`nfloat`へのマッピングの種類、`NSUInteger`と`CGFloat`型もします。

当社の API を確認し、任意のインスタンスのことを確認する必要があります、 `NSInteger`、`NSUInteger`と`CGFloat`以前にマップされている`int`、`uint`と`float`新しいに更新される`nint`、 `nuint`と`nfloat`型。

たとえばの OBJECTIVE-C メソッド定義を考えてみます。

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

以前のバインドのコントラクトには、次の定義が必要がある。 場合

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

新しいバインドを更新します。

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
私たちはマッピングする場合、新しいバージョン サード パーティ製のライブラリと私たちが最初にリンクされているよりも、確認する必要があります、`.h`あればライブラリとヘッダー ファイルに既存の明示的な呼び出し`int`、 `int32_t`、 `unsigned int`、`uint32_t`または`float`するアップグレードされた、 `NSInteger`、`NSUInteger`または`CGFloat`します。 そうである場合、同じ変更を`nint`、`nuint`と`nfloat`型にも、そのマッピングにする必要があります。

これらのデータ型の変更の詳細については、次を参照してください。、[ネイティブ型](~/cross-platform/macios/nativetypes.md)ドキュメント。

## <a name="update-the-coregraphics-types"></a>CoreGraphics 型を更新します。

使用されるポイント、サイズ、および四角形のデータ型`CoreGraphics`で実行されているデバイスに応じて 32 ビットまたは 64 ビットを使用します。 発生すると、データ型と一致する既存のデータ構造を使用して Xamarin は iOS と Mac の Api は、最初にバインドされると`System.Drawing`(`RectangleF`など)。

呼び出すときに、既存のコードにする必要があります、次の調整の 64 ビットと新しいネイティブ データ型をサポートするために必要であるため`CoreGraphic`メソッド。

- **CGRect** -使用`CGRect`の代わりに`RectangleF`四角形の領域にポイント浮動を定義する場合。
- **CGSize** -使用`CGSize`の代わりに`SizeF`浮動小数点のポイント サイズ (幅と高さ) を定義するときにします。
- **CGPoint** -使用`CGPoint`の代わりに`PointF`(X と Y 座標) の場所にポイント浮動を定義する場合。

API を確認し、任意のインスタンスのことを確認する必要が`CGRect`、`CGSize`または`CGPoint`以前にバインドするされた`RectangleF`、`SizeF`または`PointF`ネイティブな型に変更する`CGRect`、`CGSize`または`CGPoint`直接します。

たとえば、OBJECTIVE-C の初期化子を考えてみます。

```csharp
- (id)initWithFrame:(CGRect)frame;
```

場合は、前のバインディングには、次のコードが含まれています。

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

そのコードを更新します。

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

すべてのコードの変更の準備には、バインド プロジェクトを変更または Unified Api に対してバインド ファイルを作成する必要があります。

## <a name="modify-the-binding-project"></a>バインド プロジェクトを変更します。

Unified Api を使用して、バインド プロジェクトを更新する最後の手順として変更するか必要があります、 `MakeFile` (バインドしているから Visual studio for Mac) の場合、プロジェクトまたは Xamarin プロジェクトの種類を作成し、指示するために使用_btouch_クラシックものではなく Unified Api をバインドします。


### <a name="updating-a-makefile"></a>メイクファイルの更新

場合は、Xamarin に、バインド プロジェクトをビルドするメイクファイルを使用しています。含める必要が、DLL、`--new-style`コマンド ライン オプションと呼び出し`btouch-native`の代わりに`btouch`します。

したがって、次を指定`MakeFile`:

```csharp
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

呼び出し元からに切り替える必要があります`btouch`に`btouch-native`ので、マクロ定義を次のように調整しますが。

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

呼び出しを更新`btouch`を追加し、`--new-style`オプションを次のようにします。

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

実行できるようになりました、 `MakeFile` API の新しい 64 ビット バージョンをビルドするには通常どおりです。

### <a name="updating-a-binding-project-type"></a>バインド プロジェクトの種類を更新しています

場合は当社の API を構築する、Visual Studio for Mac プロジェクト テンプレートのバインドを使用している、バインド プロジェクト テンプレートの新しい Unified API バージョンに更新する必要になります。 これを行う最も簡単な方法では、すべての既存のコードと設定を新しい Unified API のバインド プロジェクトの追加とコピーを開始します。

次の手順で行います。

1. Visual Studio for mac の起動
2. 選択**ファイル** > **新しい** > **ソリューション.**
3. 新しいソリューション ダイアログ ボックスで選択**iOS** > **Unified API** > **iOS プロジェクトのバインド**: 

    [![](update-binding-images/image01new.png "新しいソリューション ダイアログ ボックスで、iOS を選択します/Unified API/iOS バインド プロジェクト。")](update-binding-images/image01new.png#lightbox)
4. 新しいプロジェクトの構成 ダイアログ ボックスで次のように入力します。、**名前**新しいバインド プロジェクトをクリックします、 **OK**ボタン。
5. バインドを作成する予定は OBJECTIVE-C ライブラリの 64 ビット バージョンが含まれます。
6. 既存の 32 ビット クラシック API バインド プロジェクトからソース コードをコピー (など、`ApiDefinition.cs`と`StructsAndEnums.cs`ファイル)。
7. ソース コード ファイルに上記説明したように変更を行います。

すべての変更を行うには、32 ビット バージョンと同様に、新しい 64 ビット バージョンの API を構築できます。

## <a name="summary"></a>まとめ

この記事では、新しい Unified Api と 64 ビットのデバイスをサポートするために既存の Xamarin バインド プロジェクトに対して行う必要がある変更を示しましたし、API の新しい 64 ビットの互換性のあるバージョンをビルドするために必要な手順を実行します。



## <a name="related-links"></a>関連リンク

- [Mac および iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64 ビットのプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md)
- [既存の iOS アプリのアップグレード](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
