---
title: Xamarin の NuGet パッケージを手動で作成する
description: このドキュメントには、Xamarin プラットフォームを対象とする NuGet パッケージの構築に役立つヒントが含まれています。 NuGet パッケージ Xamarin プロファイル、プラットフォーム依存関係を備えた PCL Nuget、さまざまなオープンソースサンプルへのリンクについて説明します。
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 16b8f303555bc2f45516c3c060c0d2482f9c4954
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728227"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Xamarin の NuGet パッケージを手動で作成する

_このページには、Xamarin プラットフォームを対象とする NuGet パッケージのビルドに役立つヒントがいくつか含まれています。_

> [!NOTE]
> Xamarin Studio 6.2 (および Visual Studio for Mac) には、PCL、.NET Standard、または共有プロジェクトから NuGet パッケージを_自動的に_生成する機能が含まれています。 詳細については、「[コード共有用のマルチプラットフォームライブラリ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)」ガイドを参照してください。

## <a name="nuget-package-xamarin-profiles"></a>NuGet パッケージ Xamarin プロファイル

[複数の .NET Framework バージョンおよびプロファイル](https://docs.nuget.org/create/enforced-package-conventions)をサポートする NuGet web サイトでは、さまざまな Microsoft フレームワークとプロファイルをサポートする方法について説明していますが、Xamarin で使用されるターゲットフレームワーク名は含まれていません。

現在使用されている Xamarin ターゲットフレームワークは、次のとおりです。

- **モノ android** -Xamarin android
- **Xamarin ios** -xamarin [Unified API](~/cross-platform/macios/unified/index.md) (64 ビットをサポート)
- Xamarin. **mac** -xamarin のモバイルプロファイル。これは、Xamarin と XAMARIN の API サーフェスに相当します。

以前の iOS [Classic API](~/cross-platform/macios/unified/index.md)のターゲットもあります。

- **Monotouch.dialog** -iOS Classic API

これらのすべてを対象とする**nuspec**ファイルは次のようになります。

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

上の例では、ポータブルクラスライブラリはすべて無視されます。

**Nuspec**ファイルのほとんどはターゲットフレームワークのバージョン番号を指定しますが、アセンブリがそのターゲットフレームワークのすべてのバージョンで動作する場合は省略可能です。 このため、ターゲットが**lib/モノ android**の場合、これはすべてのバージョンの Xamarin android で動作することを意味します。

小数点のない一連の数値を使用してバージョンを指定することも、小数点を使用して指定することもできます。 小数点がない場合、NuGet は各数字の間に '. ' を挿入することによって、各数値を取得し、それを1つのバージョンに変換します。

上記の "MonoAndroid10" は "Android 1.0" を意味します。 これは、プロジェクトの[ターゲットフレームワーク](~/android/app-fundamentals/android-api-levels.md)がモノ android バージョン1.0 以降である必要があることを意味します。 バージョンは、プロジェクトファイルの `<TargetFrameworkVersion>` 要素で指定されます。

解説:

- **MonoAndroid403**は Android 4.0.3 以降 (ie API レベル 15) と一致します
- **IOS10**は、Xamarin. iOS 1.0 以降に一致します。
- **Xamarin. ios 1.0**も Xamarin. ios 1.0 以降に一致します。

## <a name="pcl-nugets-with-platform-dependencies"></a>プラットフォームの依存関係を持つ PCL Nuget

PCL プロファイルは、アクセスできる .NET framework Api に限定されており、プラットフォーム固有のコードにはアクセスできません。 これらのサードパーティのリンクは、PCL とネイティブ Api を使用して Xamarin およびその他のプラットフォームとの互換性を提供する NuGet パッケージを作成するためのさまざまなアプローチについて説明します。

- [ポータブルクラスライブラリを機能させる方法](https://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Bait とスイッチの PCL トリック](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Xamarin で動作する NuGet PCL を作成する](https://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

この[PCL プロファイルの外部リストと NuGet ターゲット名](https://portablelibraryprofiles.stephencleary.com)は、参考資料としても役に立ちます。

## <a name="examples"></a>使用例

参照可能なオープンソースの例を次に示します。

- [**ModernHttpClient**](https://www.nuget.org/packages/modernhttpclient/) – System .Net. Http を使用してアプリを作成しますが、でこのライブラリを削除すると、大幅に高速になります ([ソース](https://github.com/paulcbetts/ModernHttpClient)の表示)。
- [**記号**](https://www.nuget.org/packages/Splat/)–プラットフォームをクロスプラットフォームにするためのライブラリです ([ソース](https://github.com/paulcbetts/Splat)の表示)。
- [**Ngraphics**](https://www.nuget.org/packages/NGraphics/) -.net でベクターグラフィックスをレンダリングするためのクロスプラットフォームライブラリ ([ソース](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)の表示)。

## <a name="related-links"></a>関連リンク

- [Nugetizer-3000 自動 NuGet 作成](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)       
- [プロジェクトに NuGet を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
