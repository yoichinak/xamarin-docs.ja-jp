---
title: Xamarin 用 NuGet パッケージを手動で作成します。
description: このドキュメントには、Xamarin プラットフォームを対象とする NuGet パッケージの構築に役立つヒントが含まれています。 NuGet パッケージの Xamarin プロファイル、プラットフォームの依存関係を持つ PCL NuGets について説明し、さまざまなオープン ソースのサンプルへのリンクします。
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 2f66d8a3960741643013a1010162f52d283026d6
ms.sourcegitcommit: afe9d93373d66eb45d82cabefca83b5733969634
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855707"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Xamarin 用 NuGet パッケージを手動で作成します。

_このページには、Xamarin プラットフォームを対象とする NuGet パッケージの構築に役立つヒントいくつかにはが含まれています。_

> [!NOTE]
> Xamarin Studio 6.2 (と Visual Studio for Mac) に、機能が用意されており_自動的に_PCL、.NET Standard、または共有プロジェクトから NuGet パッケージを生成します。 参照してください、[コードを共有するためのマルチプラット フォーム ライブラリ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)詳細についてガイドします。

## <a name="nuget-package-xamarin-profiles"></a>NuGet パッケージの Xamarin のプロファイル

NuGet の web サイトの[複数の .NET Framework バージョンをサポートしているとプロファイル](https://docs.nuget.org/create/enforced-package-conventions)さまざまな Microsoft のフレームワークとプロファイルをサポートする方法について説明しますが、Xamarin で使用されるターゲット フレームワーク名には含まれません。

使用中の主な Xamarin ターゲット フレームワークは今日は。

* **MonoAndroid** -Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [Unified API](~/cross-platform/macios/unified/index.md) (64 ビットをサポートしています)
* **Xamarin.Mac** -Xamarin.Mac のモバイル プロファイルこれは、Xamarin.iOS および Xamarin.Android API サーフェイスに相当します。

古い iOS 用のターゲットも[クラシック API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -iOS クラシック API

A **.nuspec**これらすべてを対象となるファイルはのようになります。

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

上記では、すべてのポータブル クラス ライブラリは無視されます。

ほとんど **.nuspec**ファイル指定のターゲット フレームワークのバージョン番号が、アセンブリは、そのターゲット フレームワークのすべてのバージョンで動作している場合は省略可能です。 ターゲットがあれば**lib\MonoAndroid**つまり Xamarin.Android の任意のバージョンで動作します。

小数点なしの数値のセットでバージョンを指定することができます、または 10 進数のポイントを使用して指定できます。 10 進数のポイントのない NuGet だけにかかる各番号、それを挿入することで、バージョンに変換を '.' 間の各桁。

上記"MonoAndroid10"では、「Android 1.0」を意味します。 これは、プロジェクトのとき、[ターゲット フレームワーク](~/android/app-fundamentals/android-api-levels.md)MonoAndroid 1.0 またはそれ以降のバージョンを指定する必要があります。 バージョンが指定されて、`<TargetFrameworkVersion>`プロジェクト ファイル内の要素。

: を明確にするには

- **MonoAndroid403** Android 4.0.3 と一致して、それ以降 (API レベル 15 ie)
- **Xamarin.iOS10** 1.0 以降の Xamarin.iOS の一致
- **Xamarin.iOS1.0** Xamarin.iOS 1.0 以降にも一致

## <a name="pcl-nugets-with-platform-dependencies"></a>プラットフォームの依存関係を持つ PCL NuGets

PCL プロファイルはどのような .NET framework にアクセスできる Api に制限があり、確かにプラットフォーム固有のコードにアクセスできません。 これらのサード パーティ製のリンクでは、PCL とネイティブ Api を使用して、Xamarin とその他のプラットフォームの互換性を提供する NuGet パッケージを作成するためのさまざまな方法について説明します。

- [ポータブル クラス ライブラリの作業を作成する方法](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [おとりの PCL トリック](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Xamarin.iOS で動作する NuGet PCL を作成します。](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

この外部[、NuGet のターゲットの名前の PCL プロファイル一覧](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY)便利な参照されています。

## <a name="examples"></a>使用例

オープン ソース例をいくつかを参照することができます。

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) – System.Net.Http を使用して、アプリの記述が内にこのライブラリを削除して大幅に高速化が変わります (ビュー[ソース](https://github.com/paulcbetts/ModernHttpClient))。
- [**Splat** ](https://www.nuget.org/packages/Splat/) – 物事をクロスプラット フォーム対応にする必要があるライブラリ (ビュー[ソース](https://github.com/paulcbetts/Splat))。
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -.NET でのベクター グラフィックスをレンダリングするためのクロス プラットフォーム ライブラリ (ビュー[ソース](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec))。

## <a name="related-links"></a>関連リンク

- [Nugetizer 3000 Nuget 作成の自動化](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)       
- [プロジェクトに NuGet を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
