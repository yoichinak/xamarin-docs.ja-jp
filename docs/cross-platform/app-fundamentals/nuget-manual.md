---
title: Xamarin の NuGet パッケージを手動で作成します。
description: このドキュメントには、Xamarin プラットフォームを対象とする NuGet パッケージの構築を支援するためのヒントが含まれています。 NuGet パッケージの Xamarin プロファイル、プラットフォームの依存関係を持つ PCL NuGets をについて説明し、さまざまなオープン ソースのサンプルにリンクします。
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 2adfe70e1d37c7f6e6fc825de1d86513de4a985c
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34845968"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Xamarin の NuGet パッケージを手動で作成します。

_このページには、Xamarin プラットフォームを対象とする NuGet パッケージの構築を支援するいくつかのヒントが含まれています。_

> [!NOTE]
> Xamarin Studio 6.2 (および Visual Studio for Mac) にする機能が含まれています_自動的に_PCL、.NET Standard、または共有プロジェクトから NuGet パッケージを生成します。 参照してください、[コードを共有するためのマルチプラット フォーム ライブラリ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)詳細についてガイドします。

## <a name="nuget-package-xamarin-profiles"></a>NuGet パッケージの Xamarin のプロファイル

NuGet の web サイトの[.NET Framework の複数のバージョンのサポートとプロファイル](https://docs.nuget.org/create/enforced-package-conventions)さまざまな Microsoft フレームワークとプロファイルをサポートする方法について説明しますが、Xamarin で使用されるターゲット フレームワーク名には含まれません。

使用中では、メインの Xamarin ターゲット フレームワークは現在は。

* **MonoAndroid** -Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [Unified API](~/cross-platform/macios/unified/index.md) (64 ビットをサポートします)
* **Xamarin.Mac** -Xamarin.Mac のモバイル プロファイルは、Xamarin.iOS および Xamarin.Android API サーフェイスに相当します。

ターゲットがありますまた、古い iOS 用[Classic API](~/cross-platform/macios/unified/index.md):。

* **MonoTouch** -iOS Classic API

A **.nuspec**これらすべてを対象となるファイルのように表示します。

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

上記のすべてのポータブル クラス ライブラリを無視します。

ほとんど **.nuspec**ファイルがターゲット フレームワークのバージョン番号を指定が省略可能な場合、アセンブリは、そのターゲット フレームワークのすべてのバージョンで動作します。 ターゲットがあれば**lib\MonoAndroid**つまり Xamarin.Android の任意のバージョンでの動作です。

小数点を持たない数値のセットとバージョンを指定できますか、10 進数のポイントを使用することを指定できます。 10 進数のポイントがない NuGet はだけの個々 の数を取得し、電源をバージョンに挿入することで、'.' 各桁の間です。

上記"MonoAndroid10"では、「Android 1.0」を意味します。 これは、プロジェクトのとき、[ターゲット フレームワーク](~/android/app-fundamentals/android-api-levels.md)MonoAndroid 1.0 またはそれ以降のバージョンを指定する必要があります。 バージョンが指定されて、`<TargetFrameworkVersion>`プロジェクト ファイル内の要素。

: を明確にするには

- **MonoAndroid403** Android 4.0.3 以降と一致して新しい (ie は API レベル 15)
- **Xamarin.iOS10** Xamarin.iOS 1.0 以降と一致します。
- **Xamarin.iOS1.0** Xamarin.iOS 1.0 以降にも一致

## <a name="pcl-nugets-with-platform-dependencies"></a>プラットフォーム依存関係を持つ PCL NuGets

PCL プロファイルはどのような .NET framework にアクセスする Api に制限があり、確かにプラットフォーム固有のコードにアクセスできません。 これらのサード パーティ製のリンクでは、Xamarin とその他のプラットフォームの互換性を提供するために PCL をおよびネイティブ Api を使用する NuGet パッケージを作成するための方法について説明します。

- [ポータブル クラス ライブラリの作業を作成する方法](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [おとり PCL トリック](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Xamarin.iOS で動作するために PCL を NuGet を作成します。](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

この外部[PCL プロファイルとその NuGet ターゲット名の一覧](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY)便利参照されています。

## <a name="examples"></a>使用例

オープン ソース例をいくつかを参照することができます。

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) – System.Net.Http を使用してアプリを記述が内にこのライブラリを削除しては大幅に高速移動 (ビュー[ソース](https://github.com/paulcbetts/ModernHttpClient))。
- [**Splat** ](https://www.nuget.org/packages/Splat/) – を取ってもクロスプラット フォーム ライブラリでありする必要があります (ビュー[ソース](https://github.com/paulcbetts/Splat))。
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -.NET のベクター グラフィックスを表示するため、クロス プラットフォーム ライブラリ (ビュー[ソース](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec))。

## <a name="related-links"></a>関連リンク

- [Nugetizer 3000 自動 Nuget の作成](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [IOS 64 ビットの NuGets の更新](http://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [プロジェクトで、NuGet を含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
