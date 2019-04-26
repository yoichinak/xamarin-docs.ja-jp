---
title: 回転の処理
description: このトピックでは、Xamarin.Android でデバイスの向きの変更を処理する方法について説明します。 プログラムで印刷の向きを処理するために変更する方法も、特定のデバイスの向きのリソースを自動的に読み込む Android リソース システムを使用する方法を説明します。
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 62e64be89e26e5a8412cd34221da581e99fc5e6a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61017334"
---
# <a name="handling-rotation"></a>回転の処理

_このトピックでは、Xamarin.Android でデバイスの向きの変更を処理する方法について説明します。プログラムで印刷の向きを処理するために変更する方法も、特定のデバイスの向きのリソースを自動的に読み込む Android リソース システムを使用する方法を説明します。_


## <a name="overview"></a>概要

モバイル デバイスのローテーションは簡単に、ため、組み込みの回転はモバイル Os で標準的な機能です。 Android では、XML またはプログラムによってコード内でユーザー インターフェイスを宣言によって作成するかどうかに、アプリケーション内での回転を扱う場合の高度なフレームワークを提供します。 回転したデバイスでの宣言型のレイアウトの変更を自動的に処理する際は、アプリケーションが Android リソース システムとの緊密な統合を利用できます。 プログラムによるレイアウトの場合は、変更を手動で処理する必要があります。 これにより、さらに細かく制御より多くの作業が、実行時に開発者向け。 アプリケーションは、向きの変更の手動で制御するアクティビティの再起動を無効にできます。

このガイドでは、次のトピックの印刷の向きが調べられます。

-   **宣言型のレイアウト回転** &ndash; Android リソース システムを使用して、レイアウトと特定の向きのドローアブルの両方を読み込む方法を含め、印刷の向きに対応するアプリケーションを構築する方法。

-   **プログラムによるレイアウトの回転**&ndash;プログラムでコントロールを追加する方法と、向きの変更を手動で処理する方法。


## <a name="handling-rotation-declaratively-with-layouts"></a>レイアウトを宣言的回転の処理

名前付け規則に従うフォルダーにファイルを含めることで Android では、方向が変更されたときに、適切なファイルが自動的に読み込みます。
これには、サポートが含まれます。

-   *レイアウト リソース*&ndash;レイアウト ファイルの向きごとに増大するかを指定します。

-   *ドローアブル リソース*&ndash;の向きごとにどのドローアブルが読み込まれるかを指定します。


### <a name="layout-resources"></a>レイアウトのリソース

既定では、Android XML (AXML) ファイルが含まれる、**リソース/レイアウト**アクティビティのビューのレンダリングのフォルダーが使用されます。 このフォルダーのリソースは、ランドス ケープを具体的には追加のレイアウトのリソースが指定されていない場合、縦長と横長の両方の向きに使用されます。 既定のプロジェクト テンプレートによって作成されたプロジェクトの構造を検討してください。

[![既定のプロジェクト テンプレートの構造](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

このプロジェクトでは、1 つを作成します。 **Main.axml**ファイル、**リソース/レイアウト**フォルダー。 ときに、アクティビティの`OnCreate`メソッドが呼び出されるで定義されているビューを拡張**Main.axml、** の次の XML に示すように、ボタンを宣言します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
<Button  
  android:id="@+id/myButton"
  android:layout_width="fill_parent" 
  android:layout_height="wrap_content" 
  android:text="@string/hello"/>
</LinearLayout>
```

横向きに、アクティビティのかどうか、デバイスを回転が`OnCreate`メソッドが再度呼び出されると、同じ**Main.axml**次のスクリーン ショットに示すように、ファイルが大ききます。

[![同じ画面の向きを横長](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>印刷の向きに固有のレイアウト

レイアウト フォルダーだけでなく (縦向きの既定値し、も明示的に名前を指定できます*レイアウト ポート*という名前のフォルダーを含めることによって`layout-land`)、アプリケーションは、コードなしのランドス ケープの場合に必要なビューを定義できます変更します。

たとえば、 **Main.axml**ファイルには、次の XML が含まれていました。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is portrait"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

追加を含むレイアウト land というフォルダーにかどうか**Main.axml**ランドス ケープにレイアウトを膨張させることが表示されます、新しく追加されたを読み込む Android で、プロジェクトにファイルが追加された**Main.axml します。** 横のバージョンを検討してください、 **Main.axml**次のコードを含むファイル (わかりやすくするため、この XML は、コードの既定の縦のバージョンに似ていますが、別の文字列を使用して、 `TextView`)。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is landscape"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

このコードを実行して、縦から横へのデバイスを回転は、次に示すよう、新しい XML の読み込みを示しています。

[![縦モードの印刷縦長と横長のスクリーン ショット](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)


### <a name="drawable-resources"></a>描画可能なリソース

回転、中に Android 同様にレイアウトのリソースに描画可能なリソースを扱います。 この場合からドローアブルの取得、システム、**リソース/drawable**と**リソース/drawable land**フォルダー、それぞれします。

たとえば、プロジェクトにはで Monkey.png という名前のイメージが含まれています、**リソース/drawable** 、drawable が参照されている場所から、フォルダー、`ImageView`でこのような XML:

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

さらにあると仮定しましょう別のバージョンの**Monkey.png**含まれている**リソース/drawable land**します。 レイアウト ファイルが、デバイスの場合と同様、回転、描画可能な変更の特定の向きでは、次に示すよう。

[![別のバージョンの Monkey.png 縦長と横長モードで表示](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>プログラムによる回転の処理

場合によってコードでレイアウトを定義します。 さまざまな理由から、技術的な制限により、開発者の基本設定などを含む可能性があります。プログラムでコントロールを追加する際、アプリケーションをデバイスの向きは、XML リソースを使用した場合、自動的に処理されますを考慮する必要があります手動で。


### <a name="adding-controls-in-code"></a>コード内のコントロールの追加

コントロールをプログラムで追加するには、アプリケーションは、次の手順を実行する必要があります。

-  レイアウトを作成します。
-  レイアウトのパラメーターを設定します。
-  コントロールを作成します。
-  コントロールのレイアウトのパラメーターを設定します。
-  コントロールをレイアウトに追加します。
-  コンテンツ ビューとして、レイアウトを設定します。

たとえば、1 つで構成されるユーザー インターフェイス`TextView`に追加されたコントロール、 `RelativeLayout`、次のコードに示すようにします。

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
        
  // create TextView control
  var tv = new TextView (this);

  // set TextView's LayoutParameters
  tv.LayoutParameters = layoutParams;
  tv.Text = "Programmatic layout";

  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

このコードのインスタンスを作成、`RelativeLayout`クラスとセットの`LayoutParameters`プロパティ。 `LayoutParams`クラスは、Android の再利用可能な方法でコントロールを配置する方法をカプセル化する方法。 レイアウトのインスタンスが作成されると、コントロールが作成およびを追加することができます。 コントロールがありますも`LayoutParameters`など、`TextView`この例では。 後に、`TextView`に追加すること、作成、`RelativeLayout`と設定、`RelativeLayout`でアプリケーションを表示して、コンテンツ ビューの結果として、`TextView`よう。

[![縦長と横長の両方のモードで示す増分カウンター ボタン](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>コード内の向きを検出します。

アプリケーションが向きごとに別のユーザー インターフェイスをロードしようとしたかどうかと`OnCreate`が呼び出されます (たびに、デバイスの回転をこれは)、印刷の向きを検出し、目的のユーザー インターフェイスのコードを読み込むかする必要があります。 Android デバイスと呼ばれるクラスには、`WindowManager`を使用して現在のデバイスの回転を使用できる、`WindowManager.DefaultDisplay.Rotation`プロパティを次に示すよう。

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
                        
  // get the initial orientation
  var surfaceOrientation = WindowManager.DefaultDisplay.Rotation;
  // create layout based upon orientation
  RelativeLayout.LayoutParams tvLayoutParams;
                
  if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
  } else {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    tvLayoutParams.LeftMargin = 100;
    tvLayoutParams.TopMargin = 100;
  }
                        
  // create TextView control
  var tv = new TextView (this);
  tv.LayoutParameters = tvLayoutParams;
  tv.Text = "Programmatic layout";
        
  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

このコードを設定、`TextView`上部からのピクセルが、新しいレイアウトを次に示すように、ランドス ケープに回転したときに自動的にアニメーション化、画面の左位置指定の 100 にします。

[![縦長と横長モード ビュー状態が保持されます。](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>アクティビティの再起動の防止

内のすべての処理だけでなく`OnCreate`、アプリケーションもできなくなるアクティビティに設定して、印刷の向きを変更するときに再起動`ConfigurationChanges`で、`ActivityAttribute`次のようにします。

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
これで、デバイスを回転すると、アクティビティは再起動されません。 ここで向きの変更を手動で処理をするためにアクティビティをオーバーライドできます、`OnConfigurationChanged`メソッドから向きを決定し、`Configuration`アクティビティの下の新しい実装のように、渡されるオブジェクト。

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
public class CodeLayoutActivity : Activity
{
  TextView _tv;
  RelativeLayout.LayoutParams _layoutParamsPortrait;
  RelativeLayout.LayoutParams _layoutParamsLandscape;
                
  protected override void OnCreate (Bundle bundle)
  {
    // create a layout
    // set layout parameters
    // get the initial orientation

    // create portrait and landscape layout for the TextView
    _layoutParamsPortrait = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
                
    _layoutParamsLandscape = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    _layoutParamsLandscape.LeftMargin = 100;
    _layoutParamsLandscape.TopMargin = 100;
                        
    _tv = new TextView (this);
                        
    if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
      _tv.LayoutParameters = _layoutParamsPortrait;
    } else {
      _tv.LayoutParameters = _layoutParamsLandscape;
    }
                        
    _tv.Text = "Programmatic layout";
    rl.AddView (_tv);
    SetContentView (rl);
  }
                
  public override void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
  {
    base.OnConfigurationChanged (newConfig);
                        
    if (newConfig.Orientation == Android.Content.Res.Orientation.Portrait) {
      _tv.LayoutParameters = _layoutParamsPortrait;
      _tv.Text = "Changed to portrait";
    } else if (newConfig.Orientation == Android.Content.Res.Orientation.Landscape) {
      _tv.LayoutParameters = _layoutParamsLandscape;
      _tv.Text = "Changed to landscape";
    }
  }
}
```

ここで、`TextView's`縦と横の両方のレイアウトのパラメーターが初期化されます。 クラスの変数とパラメーターを保持する、`TextView`自体であるため、方向が変更されたときに、アクティビティが再作成されないためです。 コードがまだ使用して、`surfaceOrientartion`で`OnCreate`の初期レイアウトを設定する、`TextView`します。 その後、`OnConfigurationChanged`すべての後続のレイアウト変更を処理します。

アプリケーションを実行すると、Android は、デバイスの回転が発生し、アクティビティが再起動しないとユーザー インターフェイスの変更を読み込みます。


## <a name="preventing-activity-restart-for-declarative-layouts"></a>宣言型のレイアウトでのアクティビティの再起動の防止

XML のレイアウトを定義する場合、デバイスの回転によってアクティビティの再起動を防ぐこともできます。 アクティビティの再起動を回避する場合、この方法を使用たとえば、(パフォーマンス上の理由から、おそらく) し、異なる向きの新しいリソースを読み込む必要はありません。

これを行うには、プログラムによるレイアウトとを使用した同じ手順に従います。 設定するだけです`ConfigurationChanges`で、`ActivityAttribute`で行ったように、`CodeLayoutActivity`前。 内向きの変更を実装することができますもう一度実行する必要があるコード、`OnConfigurationChanged`メソッド。


## <a name="maintaining-state-during-orientation-changes"></a>向きの変更中の状態の維持

宣言またはプログラムによって回転の処理、かどうか、すべての Android アプリケーションはデバイスの向きが変更されたときに状態を管理するための同じ手法を実装する必要があります。 Android デバイスを回転したときに、システムの再起動を実行中のアクティビティがあるために、状態を管理することが重要です。 Android は、レイアウト、および具体的には、特定の方向のように設計されたドローアブルなどの別のリソースを読み込むしやすくします。 再起動すると、アクティビティには、クラスのローカル変数に格納されている可能性がありますが、一時的な状態が失われます。 そのため、活動が状態依存の場合は、アプリケーション レベルの状態を保持する必要があります。 アプリケーションに処理するために必要な保存と向きの変更を維持するアプリケーションの状態を復元します。

Android での永続化の状態の詳細についてを参照してください、[アクティビティのライフ サイクル](~/android/app-fundamentals/activity-lifecycle/index.md)ガイド。


## <a name="summary"></a>まとめ

この記事では、Android の組み込みの機能を使用して、回転を操作する方法について説明します。 最初に、Android リソース システムを使用して、向きに対応するアプリケーションを作成する方法を説明します。 次のコードでコントロールを追加する方法と、向きの変更を手動で処理する方法が表示されます。



## <a name="related-links"></a>関連リンク

- [回転のデモ (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [ランタイムの変更の処理](https://developer.android.com/guide/topics/resources/runtime-changes.html)
- [高速画面の向きの変更](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
