---
title: タブレットおよびデスクトップ アプリのレイアウト
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: bcd277145de13a95a0b19aa4945b02078af52978
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="layout-for-tablet-and-desktop-apps"></a>タブレットおよびデスクトップ アプリのレイアウト

Xamarin.Forms は、携帯電話、に加えてアプリも実行のプラットフォームでは、サポートされている、使用可能なすべてのデバイスの種類をサポートします。

* Ipad、
* Android タブレットは、
* Windows タブレットやデスクトップ コンピューターが (Windows 10 を実行している)。

このページを簡単に説明します。

* サポートされている[デバイスの種類](#Device_Types)、および
* ハウツー[最適化](#optimize)携帯電話とタブレットのレイアウトです。

<a name="Device_Types" />

## <a name="device-types"></a>デバイスの種類

大きな画面デバイス Xamarin.Forms によってサポートされるプラットフォームのすべての利用できます。

### <a name="ipads-ios"></a>Ipad (iOS)

Xamarin.Forms テンプレート iPad サポートを構成することによって自動的に含まれます、 **Info.plist > デバイス**設定**ユニバーサル**(つまり、iPhone と iPad の両方がサポートされている)。

快適になりスタートアップ エクスペリエンスを提供し、全画面解像度がすべてのデバイスで使用を図ることを確認してください、 [iPad に固有の起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)(ストーリー ボードを使用) が指定されています。 これにより、iPad ミニ、iPad、および iPad Pro のデバイスでアプリが正しくレンダリングされます。

IOS 9 の前に、すべてのアプリがデバイスで、全画面表示をしたが、一部 Ipad を実行できるようになりました[画面マルチタスクを分割](~/ios/platform/multitasking.md)です。
つまり、アプリが画面では、50% の幅 画面で、または全画面の片側スリム列だけをかかる場合があります。

[![](tablet-images/ipad-sml.png "iPad 分割画面例")](tablet-images/ipad.png#lightbox "iPad 分割画面の例")

分割画面機能により、幅、または限り 1366 ピクセルをわずか 320 ピクセルでうまく機能するアプリを設計する必要があります。

### <a name="android-tablets"></a>Android タブレット

Android のエコシステムでは、さまざまな大規模なタブレットまでの小規模の携帯電話からのサポートされている画面のサイズがあります。 Xamarin.Forms は、すべての画面サイズをサポートできますが、として他のプラットフォームと大規模なデバイスの場合、ユーザー インターフェイスを調整します。

多くのさまざまな画面解像度をサポートしている場合は、ユーザー エクスペリエンスを最適化するためにさまざまなサイズで、ネイティブ イメージ リソースを指定できます。
確認、 [Android リソース](~/android/app-fundamentals/resources-in-android/index.md)ドキュメント (特に[さまざまな画面サイズのリソースを作成する](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) フォルダーとファイル名に Android アプリを構成する方法の詳細についてアプリで最適化されたイメージ リソースをインクルードするプロジェクトです。

### <a name="windows-tablets-and-desktops"></a>Windows タブレットやデスクトップ

タブレットや Windows を実行しているデスクトップ コンピューターをサポートするために使用する必要があります[Windows UWP サポート](~/xamarin-forms/platform/windows/installation/index.md)、どの Windows 10 で動作するユニバーサル アプリをビルドします。

Windows タブレットやデスクトップで実行されているアプリ サイズを変更できる任意のディメンションをさらに実行されている全画面表示にします。

[![](tablet-images/splitscreen-sml.png "Windows は、画面の例を分割")](tablet-images/splitscreen.png#lightbox "Windows 分割画面の例")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>タブレットおよびデスクトップ用の最適化

スマート フォンかどうかに応じて、Xamarin.Forms のユーザー インターフェイスを調整することができます、またはタブレット/デスクトップ デバイスが使用されています。 これは、タブレットやデスクトップ コンピューターなどの大きな画面デバイスのユーザー エクスペリエンスを最適化することを意味します。


### <a name="deviceidiom"></a>Device.Idiom

使用することができます、 [ `Device` ](~/xamarin-forms/platform/device.md)アプリやユーザー インターフェイスの動作を変更するクラス。 使用して、`Device.Idiom`列挙することができます

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

この方法は、個々 のページ レイアウトに大幅な変更を加えるかより大きな画面にまったく別のページを表示するためにも拡張できます。

### <a name="leveraging-masterdetailpage"></a>MasterDetailPage を活用します。

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)画面は、使用して、iPad で特に大きくなりますに最適ですが、 [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/)ネイティブの iOS エクスペリエンスを提供します。

レビュー [Xamarin ブログの投稿](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)画面は大きくなりますは別で使用できるし、電話が 1 つのレイアウトを使用できるように、ユーザー インターフェイスを適用する方法を表示する (で、 `MasterDetailPage`)。



## <a name="related-links"></a>関連リンク

- [Xamarin のブログ](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe サンプル](https://github.com/jamesmontemagno/myshoppe)
