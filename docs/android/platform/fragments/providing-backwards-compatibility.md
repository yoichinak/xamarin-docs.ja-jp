---
title: Android サポートパッケージとの下位互換性の提供
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/12/2017
ms.openlocfilehash: c32d666da1347b947c55209ed7c7ec31a97a70e0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027295"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Android サポートパッケージとの下位互換性の提供

フラグメントの有用性は、Android 3.0 (API レベル 11) デバイスとの下位互換性がない限り制限されます。 この機能を提供するために、Google は[サポートライブラリ](https://developer.android.com/sdk/compatibility-library.html)(もともとは*android 互換ライブラリ*がリリースされた時点) を導入しました。これにより、新しいバージョンの android から以前のバージョンの android に api の一部をバックします。 Android 1.6 (API レベル 4) を実行しているデバイスを Android 2.3.3 に対して有効にする Android サポートパッケージです。 (API レベル 10)。

> [!NOTE]
> Android サポートパッケージを通じて入手できるのは、`ListFragment` と `DialogFragment` のみです。 Android サポートパッケージでは、`PreferenceFragment,` など、他のどのフラグメントサブクラスもサポートされていません。 Android 3.0 より前のアプリケーションでは機能しません。 

## <a name="adding-the-support-package"></a>サポートパッケージの追加

Android サポートパッケージは、Xamarin Android アプリケーションに自動的に追加されません。 Xamarin には、 [Android サポートライブラリ V4 NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)が用意されており、サポートライブラリを xamarin android アプリケーションに簡単に追加できます。サポートパッケージを Xamarin. android アプリケーションに含めるには、次のスクリーンショットに示すように、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)コンポーネントを xamarin android プロジェクトに追加します。 

[プロジェクトに追加されている Android サポートライブラリ v4 パッケージのスクリーンショット![](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

これらの手順を実行すると、以前のバージョンの Android でフラグメントを使用できるようになります。 これらの以前のバージョンでは、Fragment Api は同じように動作しますが、次の例外があります。 

- 次に示すように、アプリケーションが Android 3.0 以降を対象にする必要がなくなった &ndash; **android の最小バージョンを変更**します。 

    [Android マニフェストの下に設定されている最小 Android ターゲットのスクリーンショット![](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **FragmentActivity &ndash; 拡張**します。これは、ホストフラグメントであるアクティビティが `Android.App.Activity` ではなく `Android.Support.V4.App.FragmentActivity` から継承する必要があるためです。 

- `Android.App.Fragment` から継承するクラス &ndash;**名前空間の更新**は `Android.Support.V4.App.Fragment` から継承する必要があります。 ソースコードファイルの先頭にある using ステートメント "`using Android.App;`" を削除し、"`using Android.Support.V4.App`" に置き換えます。 

- **SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` は、`FragmentManager` への参照を取得するために使用する必要がある `SupportingFragmentManager` プロパティを公開します。 (例: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

これらの変更が適用されると、Android 1.6 または2.x だけでなく、Honeycomb とアイスクリームでもフラグメントベースのアプリケーションを実行できるようになります。 

## <a name="related-links"></a>関連リンク

- [Android サポートライブラリ v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
