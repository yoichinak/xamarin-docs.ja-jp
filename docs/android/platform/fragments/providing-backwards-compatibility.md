---
title: 旧バージョンと Android のサポート パッケージとの互換性を提供します。
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/12/2017
ms.openlocfilehash: 48ef40ce8560fd9fbb842dde70622d968591ab98
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666906"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>旧バージョンと Android のサポート パッケージとの互換性を提供します。

フラグメントの有用性になることがなく下位事前 Android 3.0 (API レベル 11) デバイスとの互換性に制限されています。 この機能を提供するには、Google が導入された、[サポート ライブラリ](https://developer.android.com/sdk/compatibility-library.html)(最初と呼ばれる、 *Android Compatibility Library*が解放されるときに) どの backports から新しいバージョンの Api の一部以前のバージョンの Android を android。 Android デバイスを Android 2.3.3 Android 1.6 (API レベル 4) を実行できるようにするサポート パッケージになります。 (API レベル 10)。

> [!NOTE]
> のみ、`ListFragment`と`DialogFragment`は Android のサポート パッケージで利用できます。 など、サブクラスをフラグメントの他、 `PreferenceFragment,` Android サポート パッケージでサポートされます。 前の Android 3.0 アプリケーションでは機能しません。 


## <a name="adding-the-support-package"></a>サポート パッケージを追加します。

Android のサポート パッケージは Xamarin.Android アプリケーションに自動的には追加されません。 Xamarin には、 [Android サポート ライブラリ v4 の NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)を簡単にサポート ライブラリを Xamarin.Android アプリケーションに追加します。サポート パッケージをアプリケーションに含める、Xamarin.Android に含める、 [Android サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)次のスクリーン ショットに示すように、Xamarin.Android プロジェクトにコンポーネント。 

[![Android サポート ライブラリのスクリーン ショット v4 のパッケージをプロジェクトに追加します。](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

次の手順を実行すると後で以前のバージョンの Android フラグメントを使用することになります。 フラグメント Api には、次の例外が、以前のバージョンで同じここでは機能します。 

-   **Android の最小バージョンを変更する**&ndash;アプリケーションが次に示すように Android 3.0 以降を対象とする必要がなくなった。 

    [![Android マニフェストで設定されている最小 Android のスクリーン ショットのターゲット](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **拡張 FragmentActivity** &ndash;フラグメントをホストしている、アクティビティから継承する必要があります`Android.Support.V4.App.FragmentActivity`、なく`Android.App.Activity`します。 

-   **名前空間を更新**&ndash;から継承するクラス`Android.App.Fragment`から継承する必要があります`Android.Support.V4.App.Fragment`します。 使用して、削除ステートメント" `using Android.App;` "ソース コードの上部にあるファイルし、コードに置き換えます" `using Android.Support.V4.App` "。 

-   **SupportFragmentManager を使用して、** &ndash; `Android.Support.V4.App.FragmentActivity`公開、`SupportingFragmentManager`プロパティへの参照を取得するために使用する必要があります、`FragmentManager`します。 例えば: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

これらの場所の変更と Honeycomb と Ice Cream Sandwich Android 1.6 または 2.x では、フラグメント ベースのアプリケーションを実行することができます。 


## <a name="related-links"></a>関連リンク

- [Android サポート ライブラリ v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
