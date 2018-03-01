---
title: "Nuget パッケージを更新した後にエラーをパッケージ化がありません。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f330590132e4881484eeae3efafe570d991a509a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Nuget パッケージを更新した後にエラーをパッケージ化がありません。

この問題は主に、Xamarin.Forms サンプル アプリのソリューションで報告されてが NuGet パッケージを使用するプロジェクトでこの問題が発生する可能性が起こります。 

プロジェクトまたはソリューションの Nuget パッケージを更新した後は、エラーなど、古いパッケージのバージョン番号を参照する参照してください。

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)

```

この例では*Xamarin.Forms.1.3.1.6296* Nuget パッケージの更新で削除された古いバージョン番号です。

これは、古いパッケージのバージョン番号を参照する XML 要素の .csproj ファイルに手動で追加された場合に発生することができますか、編集、Nuget はいない削除または更新していた場合を手動で追加/編集プロジェクトがされているパッケージを探してようになりましたので削除します。 

この問題を解決するには、手動で .csproj ファイルを編集し、すべての以前のバージョン番号を参照する要素を削除します。 

(場合がある、古いパッケージのバージョン番号) を削除するサンプル要素:

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />

```

