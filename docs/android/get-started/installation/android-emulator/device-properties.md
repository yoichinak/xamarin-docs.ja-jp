---
title: Android 仮想デバイス プロパティの編集
description: この記事では、Android Device Manager を使用して Android 仮想デバイスのプロファイル プロパティを編集する方法について説明します。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 3E33C136-8042-4184-A40C-3200D8CD99CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2018
ms.openlocfilehash: a1906d8c5c001c2328a0dbdce58f92092454e2c0
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69525086"
---
# <a name="editing-android-virtual-device-properties"></a>Android 仮想デバイス プロパティの編集

_この記事では、Android Device Manager を使用して Android 仮想デバイスのプロファイル プロパティを編集する方法について説明します。_

::: zone pivot="windows"

## <a name="android-device-manager-on-windows"></a>Windows 上の Android Device Manager

**Android Device Manager** では、個々の Android 仮想デバイスのプロファイル プロパティの編集がサポートされます。 **[New Device]\(新しいデバイス\)** 画面と **[Device Edit]\(デバイスの編集\)** 画面では、最初の列に仮想デバイスのプロパティがリストされ、2 番目の列に各プロパティの対応する値が示されます (以下の例を参照)。 

[![[New Device]\(新しいデバイス\) 画面の例](device-properties-images/win/01-new-device-editor-sml.png)](device-properties-images/win/01-new-device-editor.png#lightbox)

プロパティを選ぶと、そのプロパティの詳しい説明が右側に表示されます。 *ハードウェア プロファイルのプロパティ*と *AVD のプロパティ* を変更することができます。 ハードウェア プロファイルのプロパティ (`hw.ramSize` や `hw.accelerometer` など) では、エミュレートされたデバイスの物理的な特性が記述されています。 このような特性としては、画面のサイズ、使用可能な RAM の量、加速度計が存在するかどうか、などがあります。 AVD のプロパティでは、実行時の AVD の動作が指定されています。 たとえば、AVD のプロパティを構成して、AVD がレンダリングに開発用コンピューターのグラフィックス カードを使う方法を指定することができます。

次のようにしてプロパティを変更できます。

- ブール型のプロパティを変更するには、プロパティの右側にあるチェック マークをクリックします。

    ![ブール型プロパティの変更](device-properties-images/win/02-boolean-value.png)

- *enum* (列挙) 型のプロパティを変更するには、プロパティの右側にある下向き矢印をクリックして、新しい値を選びます。

    ![列挙型プロパティの変更](device-properties-images/win/04-enum-value.png)

- 文字列型または整数型のプロパティを変更するには、値の列で現在の文字列または整数の設定をダブルクリックし、新しい値を入力します。

    ![整数型プロパティの変更](device-properties-images/win/03-integer-value.png)

::: zone-end
::: zone pivot="macos"

## <a name="android-device-manager-on-macos"></a>macOS 上の Android Device Manager

**Android Device Manager** では、個々の Android 仮想デバイスのプロファイル プロパティの編集がサポートされます。 **[New Device]\(新しいデバイス\)** 画面と **[Device Edit]\(デバイスの編集\)** 画面では、最初の列に仮想デバイスのプロパティがリストされ、2 番目の列に各プロパティの対応する値が示されます (以下の例を参照)。 

[![[New Device]\(新しいデバイス\) 画面の例](device-properties-images/mac/01-new-device-editor-sml.png)](device-properties-images/mac/01-new-device-editor.png#lightbox)

プロパティを選ぶと、そのプロパティの詳しい説明が右側に表示されます。 *ハードウェア プロファイルのプロパティ*と *AVD のプロパティ* を変更することができます。 ハードウェア プロファイルのプロパティ (`hw.ramSize` や `hw.accelerometer` など) では、エミュレートされたデバイスの物理的な特性が記述されています。 このような特性としては、画面のサイズ、使用可能な RAM の量、加速度計が存在するかどうか、などがあります。 AVD のプロパティでは、実行時の AVD の動作が指定されています。 たとえば、AVD のプロパティを構成して、AVD がレンダリングに開発用コンピューターのグラフィックス カードを使う方法を指定することができます。

次のようにしてプロパティを変更できます。

- ブール型のプロパティを変更するには、プロパティの右側にあるチェック マークをクリックします。

    ![ブール型プロパティの変更](device-properties-images/mac/02-boolean-value.png)

- *enum* (列挙) 型のプロパティを変更するには、プロパティの右側にあるプルダウン メニューをクリックして、新しい値を選びます。

    ![列挙型プロパティの変更](device-properties-images/mac/04-enum-value.png)

- 文字列型または整数型のプロパティを変更するには、値の列で現在の文字列または整数の設定をダブルクリックし、新しい値を入力します。

    ![整数型プロパティの変更](device-properties-images/mac/03-integer-value.png)

::: zone-end

次の表では、**[New Device]\(新しいデバイス\)** 画面および**デバイス エディター**画面に一覧表示されるプロパティについて詳しく説明します。

[!include[](~/android/includes/emulator-properties.md)]

これらのプロパティについて詳しくは、「[Hardware Profile Properties](https://developer.android.com/studio/run/managing-avds.html#hpproperties)」(ハードウェア プロファイルのプロパティ) をご覧ください。

