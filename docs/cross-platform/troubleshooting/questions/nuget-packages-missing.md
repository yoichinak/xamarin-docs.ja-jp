---
title: Nuget パッケージを更新した後の不足しているパッケージのエラー
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 7cb802dd60d4e4879a260ff56d4f94ea5acb2965
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674848"
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Nuget パッケージを更新した後の不足しているパッケージのエラー

この問題は、Xamarin.Forms サンプル アプリのソリューションで主に報告されていますが、この問題が発生する可能性が NuGet パッケージを使用するプロジェクトで発生することができます。 

プロジェクトまたはソリューションの Nuget パッケージを更新した場合は、エラーなど、古いパッケージのバージョン番号の参照を参照してください。

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)
```

この例では*Xamarin.Forms.1.3.1.6296* Nuget パッケージの更新で削除された古いバージョン番号です。

これは、古いパッケージのバージョン番号を参照する .csproj ファイルの XML 要素が手動で追加した場合に発生することができますか、編集、Nuget はいない削除または更新していた場合を手動で追加/編集、プロジェクトを今すぐパッケージを探して、削除されます。 

この問題を解決するには、手動で .csproj ファイルを編集し、すべての以前のバージョン番号を参照する要素を削除します。 

(古いパッケージのバージョン番号がある) 場合、削除する要素をサンプル:

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />
```