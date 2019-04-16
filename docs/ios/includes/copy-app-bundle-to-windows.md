---
ms.openlocfilehash: 5bcf548c0ff907df0913e77a1def2fe27f3eecfd
ms.sourcegitcommit: e7f27ba75cae5099ef053b819b84132a77d4f9e7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2019
ms.locfileid: "52295055"
---

Visual Studio や Mac Build エージェントで iOS アプリをビルドするとき、.app バンドルは Windows マシンにコピーされません。 Visual Studio 7.4 用の Xamarin ツールでは新たに `CopyAppBundle` プロパティが加わりました。このプロパティによって CI ビルドは .app バンドルを Windows にコピーできます。

この機能を利用するには、この機能を適用するプロパティ グループの下で .csproj に `CopyAppBundle` プロパティを追加します。 たとえば、次の例では、**iPhoneSimulator** をターゲットにする**デバッグ** ビルドのために .app バンドルを Windows コンピューターにコピーする方法を確認できます。

    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
        <CopyAppBundle>true</CopyAppBundle>
    </PropertyGroup>

