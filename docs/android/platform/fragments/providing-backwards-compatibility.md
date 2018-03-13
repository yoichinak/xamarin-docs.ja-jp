---
title: "旧バージョンとの Android サポート パッケージとの互換性を提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/12/2017
ms.openlocfilehash: 670ec465843bbe819b41a53fff71b01ab78b0059
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>旧バージョンとの Android サポート パッケージとの互換性を提供します。

フラグメントの実用性になるせず後方の事前 Android 3.0 (API レベル 11) デバイスとの互換性を制限します。 この機能を提供するには、Google が導入されました、[サポート ライブラリ](http://developer.android.com/sdk/compatibility-library.html)(最初と呼ばれる、 *Android Compatibility Library*リリースされたとき) どの backports から新しいバージョンの Api の一部Android の古いバージョンを android。 Android のサポート パッケージを Android 2.3.3 に Android 1.6 (API レベル 4) を実行しているデバイスを有効にすることをお勧めします。 (API レベル 10)。

> [!NOTE]
> のみ、`ListFragment`と`DialogFragment`は Android のサポート パッケージで使用できます。 サブクラスをなどの他のフラグメント、 `PreferenceFragment,` Android のサポート パッケージではサポートされています。 事前 Android 3.0 アプリケーションでは機能しません。 


## <a name="adding-the-support-package"></a>サポート パッケージを追加します。

Android のサポート パッケージは Xamarin.Android アプリケーションに自動的には追加されません。 Xamarin では提供、 [v4 NuGet パッケージの Android サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)Xamarin.Android アプリケーションへのサポート ライブラリの追加を簡単にします。サポート パッケージをアプリケーションに含まれて、Xamarin.Android に含める、 [Android サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Xamarin.Android プロジェクトに、次のスクリーン ショットに示すようにコンポーネント。 

[![プロジェクトに追加されている Android のサポート ライブラリのスクリーン ショット v4 パッケージ](providing-backwards-compatibility-images/02.png)](providing-backwards-compatibility-images/02.png#lightbox)

次の手順を実行すると後で以前のバージョンの Android フラグメントを使用する可能になります。 フラグメントの Api には、次の例外を除き、これらの以前のバージョンで同じここでは機能します。 

-   **最低限の Android バージョンを変更する**&ndash;を次に示すように Android 3.0 以上を対象とする必要がなくなったアプリケーション。 

    [![アプリケーションのプロパティで設定されている最低限 Android のスクリーン ショットのターゲット](providing-backwards-compatibility-images/03.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **拡張 FragmentActivity** &ndash;フラグメントをホストしている、アクティビティから継承する必要があります`Android.Support.V4.App.FragmentActivity`、およびからではなく`Android.App.Activity`です。 

-   **名前空間を更新**&ndash;から継承するクラス`Android.App.Fragment`から継承する必要があります`Android.Support.V4.App.Fragment`です。 使用して、削除するステートメント" `using Android.App;` "ソース コードの上部にあるファイルし、置き換える" `using Android.Support.V4.App` "です。 

-   **SupportFragmentManager を使用して** &ndash; `Android.Support.V4.App.FragmentActivity`公開、`SupportingFragmentManager`プロパティへの参照を取得するために使用する必要があります、`FragmentManager`です。 例: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

これらの場所の変更は Android 1.6 または 2.x および Honeycomb とアイスクリーム サウスサンドウィッチ フラグメント ベースのアプリケーションを実行することはできます。 


## <a name="related-links"></a>関連リンク

- [Android のサポート ライブラリ v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
