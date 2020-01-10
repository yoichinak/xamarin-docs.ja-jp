---
title: NuGet パッケージの更新後にパッケージが見つからないというエラーが発生する
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: a1ed4a2be63973e9e26dcb06163a3f9fd7eae649
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728097"
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>NuGet パッケージの更新後にパッケージが見つからないというエラーが発生する

この問題は、主に Xamarin. Forms サンプルアプリソリューションで報告されていますが、この問題の可能性は、NuGet パッケージを使用するプロジェクトで発生する可能性があります。

プロジェクトまたはソリューションで NuGet パッケージを更新した後、古いパッケージのバージョン番号を参照するエラーが表示されます。次に例を示します。

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)
```

この例では、 *1.3.1.6296*は NuGet パッケージの更新で削除された古いバージョン番号です。

これは、古いパッケージのバージョン番号を参照する .csproj ファイル内の XML 要素が手動で追加または編集された場合に発生する可能性があります。手動で追加または編集されている場合、NuGet は削除または更新されないため、プロジェクトは削除されたパッケージを探しています。

この問題を解決するには、.csproj ファイルを手動で編集し、以前のバージョン番号を参照するすべての要素を削除します。

削除するサンプル要素 (パッケージのバージョン番号が古い場合):

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />
```
