---
title: Android サポート パッケージとの下位互換性の提供
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/12/2017
ms.openlocfilehash: c32d666da1347b947c55209ed7c7ec31a97a70e0
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027295"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Android サポート パッケージとの下位互換性の提供

フラグメントの有用性は、Android 3.0 (API レベル 11) より前のデバイスとの下位互換性がないと限定的です。 この機能を提供するために、Google では[サポート ライブラリ](https://developer.android.com/sdk/compatibility-library.html) (リリース時のもともとの名前は "*Android 互換性ライブラリ*" でした) が導入されました。これにより、新しいバージョンの Android から古いバージョンの Android に API の一部がバックポートされます。 Android サポート パッケージにより、デバイスで Android 1.6 (API レベル 4) から Android 2.3.3 (API レベル 10) を実行できるようになります。

> [!NOTE]
> Android サポート パッケージで使用できるのは、`ListFragment` と `DialogFragment` のみです。 `PreferenceFragment,` などの他の Fragment サブクラスは、Android サポート パッケージではサポートされていません。 Android 3.0 より前のアプリケーションでは機能しません。 

## <a name="adding-the-support-package"></a>サポート パッケージの追加

Xamarin.Android アプリケーションには、Android サポート パッケージは自動的に追加されません。 Xamarin では、Xamarin.Android アプリケーションへのサポート ライブラリの追加を簡単にするため、[Android Support Library v4 NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)が提供されています。サポート パッケージを Xamarin.Android アプリケーションに含めるには、次のスクリーンショットに示すように、[Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) コンポーネントを Xamarin.Android プロジェクトに組み込みます。 

[![プロジェクトに追加された Android Support Library v4 パッケージのスクリーンショット](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

これらの手順を実行すると、以前のバージョンの Android でフラグメントを使用できるようになります。 Fragment API は以前のバージョンでも同じように動作しますが、次の例外があります。 

- **Android の最小バージョンを変更する** &ndash; 次に示すように、アプリケーションでは Android 3.0 以降を対象にする必要がなくなります。 

    [![Android マニフェストで設定されている Android 最小ターゲットのスクリーンショット](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **FragmentActivity を拡張する** &ndash; フラグメントをホストしているアクティビティは、`Android.App.Activity` ではなく `Android.Support.V4.App.FragmentActivity` から継承する必要があります。 

- **名前空間を更新する** &ndash; `Android.App.Fragment` から継承しているクラスは、今後は `Android.Support.V4.App.Fragment` から継承する必要があります。 ソース コード ファイルの先頭にある using ステートメント "`using Android.App;`" を削除し、"`using Android.Support.V4.App`" に置き換えます。 

- **SupportFragmentManager を使用する** &ndash; `FragmentManager` への参照を取得するには、`Android.Support.V4.App.FragmentActivity` によって公開されている `SupportingFragmentManager` プロパティを使用する必要があります。 次に例を示します。 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

これらの変更を適用すると、フラグメント ベースのアプリケーションを、Android 1.6 または 2.x だけでなく、Honeycomb と Ice Cream Sandwich でも実行できるようになります。 

## <a name="related-links"></a>関連リンク

- [Android Support Library v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
