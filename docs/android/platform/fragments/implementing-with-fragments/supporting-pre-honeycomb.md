---
title: "サポート パッケージを使用して事前 Honeycomb Android のサポート"
ms.topic: article
ms.prod: xamarin
ms.assetid: DACD0C14-5DDF-7BDE-6844-80550D301307
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 242adc94f04b3bda40238370ca924c1807a0c787
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="supporting-pre-honeycomb-android-using-support-packages"></a>サポート パッケージを使用して事前 Honeycomb Android のサポート

*Android のサポート パッケージ*戻るポートの新しい API の一部がライブラリから成る&ndash;フラグメントなど&ndash;Android の古いバージョンにします。 ように、Android のサポート パッケージを追加すると、実行してアプリケーション Android 2.3 デバイスは次の画面に示すように。

![フラグメント チュートリアルのスクリーン ショット](supporting-pre-honeycomb-images/00.png)

![アクティビティの詳細のスクリーン ショット](supporting-pre-honeycomb-images/01.png)


## <a name="adding-the-support-package"></a>サポート パッケージを追加します。

Android のサポート パッケージは Xamarin.Android アプリケーションに自動的には追加されません。 Xamarin では提供、 [v4 NuGet パッケージの Android サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)Xamarin.Android アプリケーションへのサポート ライブラリの追加を簡単にします。
サポート パッケージをアプリケーションに含まれて、Xamarin.Android に含める、 [Android サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Xamarin.Android プロジェクトに、次のスクリーン ショットに示すようにコンポーネント。

![Android のサポート ライブラリ v4 パッケージを追加します。](supporting-pre-honeycomb-images/02.png)

パッケージが追加されると、Android 2.2 またはそれ以上のターゲット フレームワークを変更します。

![ターゲット フレームワーク API レベルの変更のスクリーン ショット](supporting-pre-honeycomb-images/03.png)

また、同じ API レベルの対象である最低限の Android バージョンを確認します。

![最低限の Android バージョンを設定のスクリーン ショット](supporting-pre-honeycomb-images/04.png)



### <a name="change-mainactivity-to-derive-from-fragmentactivity"></a>MainActivity を FragmentActivity から派生させるための変更します。

フラグメントを使用するすべての活動を継承する必要があります`Xamarin.Support.V4.App.FragmentActivity`です。 このクラスは、Android のバージョンに関係なく、アクティビティによってホストされるフラグメントを許容するためのサポート パッケージのために必要な部分です。 `MainActivity` 1 つだけわずかな変更が必要です: から継承する必要があります、 `Android.Support.V4.App.FragmentActivity`:

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```


### <a name="change-detailsactivity-to-derive-from-fragmentactivity"></a>FragmentActivity から派生する DetailsActivity を変更します。

`DetailsActivity` 変更する必要があります、`Activity`を`FragmentActivity`です。 として`FragmentManager`と互換性のない Honeycomb 前のバージョンの Android、Android のサポート パッケージが含まれています、ラッパー クラスには`SupportFragmentManager`、旧バージョンとの互換性を提供します。 各`FragmentActivity`が、`SupportFragmentManager`プロパティ、および`DetailsActivity`を使用する変更は、`SupportFragmentManager`の代わりに、 `FragmentManager`:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("index", 0);
       var details = DetailsFragment.NewInstance(index);
       var fragmentTransaction = SupportFragmentManager.BeginTransaction(); // Notice the change from FragmentManager to SupportFragmentManager
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

これらの変更を完了しました今すぐ Android 1.6 でおよび以降を実行することができ、フラグメントを使用して、ターゲット デバイスのサイズに UI を調整するアプリケーションがあります。


## <a name="related-links"></a>関連リンク

- [Android ライブラリ v4 のサポート](https://www.nuget.org/packages/Xamarin.Android.Support.v4)
