---
ms.openlocfilehash: 22a8542c0e829db8b889abc2c7fe5f5ab9d19ed4
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527648"
---

Visual Studio や Mac Build エージェントで iOS アプリをビルドするとき、.app バンドルは Windows マシンにコピーされません。 Visual Studio 7.4 用の Xamarin ツールでは新たに `CopyAppBundle` プロパティが加わりました。このプロパティによって CI ビルドは .app バンドルを Windows にコピーできます。

この機能を利用するには、この機能を適用するプロパティ グループの下で .csproj に `CopyAppBundle` プロパティを追加します。 たとえば、次の例では、**iPhoneSimulator** をターゲットにする**デバッグ** ビルドのために .app バンドルを Windows コンピューターにコピーする方法を確認できます。

```xml
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
    <CopyAppBundle>true</CopyAppBundle>
</PropertyGroup>
```
