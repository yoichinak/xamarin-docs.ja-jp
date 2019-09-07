---
title: Android サポートパッケージとの下位互換性の提供
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/12/2017
ms.openlocfilehash: 838f8bdcf3bd82a31bf0d033eee628bd19ad1c30
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757548"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Android サポートパッケージとの下位互換性の提供

フラグメントの有用性は、Android 3.0 (API レベル 11) デバイスとの下位互換性がない限り制限されます。 この機能を提供するために、Google は[サポートライブラリ](https://developer.android.com/sdk/compatibility-library.html)(もともとは*android 互換ライブラリ*がリリースされた時点) を導入しました。これにより、新しいバージョンの android から以前のバージョンの android に api の一部をバックします。 Android 1.6 (API レベル 4) を実行しているデバイスを Android 2.3.3 に対して有効にする Android サポートパッケージです。 (API レベル 10)。

> [!NOTE]
> `ListFragment` とは、Androidサポートパッケージを通じて`DialogFragment`のみ使用できます。 などの他のフラグメントサブクラス`PreferenceFragment,`は、Android サポートパッケージではサポートされていません。 Android 3.0 より前のアプリケーションでは機能しません。 

## <a name="adding-the-support-package"></a>サポートパッケージの追加

Android サポートパッケージは、Xamarin Android アプリケーションに自動的に追加されません。 Xamarin には、 [Android サポートライブラリ V4 NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)が用意されており、サポートライブラリを xamarin android アプリケーションに簡単に追加できます。サポートパッケージを Xamarin. android アプリケーションに含めるには、次のスクリーンショットに示すように、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)コンポーネントを xamarin android プロジェクトに追加します。 

[![プロジェクトに追加されている Android サポートライブラリ v4 パッケージのスクリーンショット](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

これらの手順を実行すると、以前のバージョンの Android でフラグメントを使用できるようになります。 これらの以前のバージョンでは、Fragment Api は同じように動作しますが、次の例外があります。 

- **Android の最小バージョンを変更する**&ndash;次に示すように、アプリケーションは Android 3.0 以降を対象にする必要がなくなりました。 

    [![Android マニフェストで設定されている最小 Android ターゲットのスクリーンショット](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **FragmentActivity の拡張**フラグメントをホストしているアクティビティは、から`Android.Support.V4.App.FragmentActivity`ではなく`Android.App.Activity` 、から継承する必要があります。 &ndash; 

- **名前空間の更新**から継承するクラスは、から`Android.Support.V4.App.Fragment`継承する必要があります。 `Android.App.Fragment` &ndash; ソースコードファイルの先頭`using Android.App;`にある using ステートメント "" を削除し、" `using Android.Support.V4.App` " に置き換えます。 

- **SupportFragmentManager の使用**への参照を`SupportingFragmentManager`取得するために使用する必要があるプロパティを公開します。 `FragmentManager` &ndash; `Android.Support.V4.App.FragmentActivity` 例えば: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

これらの変更が適用されると、Android 1.6 または2.x だけでなく、Honeycomb とアイスクリームでもフラグメントベースのアプリケーションを実行できるようになります。 

## <a name="related-links"></a>関連リンク

- [Android サポートライブラリ v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
