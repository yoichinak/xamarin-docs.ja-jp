---
title: "回転の処理"
description: "このトピックでは、Xamarin.Android でデバイスの向きの変更を処理する方法について説明します。 自動的に変更をプログラムによって印刷の向きを処理する方法と同様、特定のデバイスの向きのリソースを読み込む Android リソース システムを操作する方法を説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c31dbfeea3134de95f3275a7fa79c508a94d6a91
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="handling-rotation"></a>回転の処理

_このトピックでは、Xamarin.Android でデバイスの向きの変更を処理する方法について説明します。自動的に変更をプログラムによって印刷の向きを処理する方法と同様、特定のデバイスの向きのリソースを読み込む Android リソース システムを操作する方法を説明します。_


## <a name="overview"></a>概要

モバイル デバイスはローテーション簡単にため組み込みの回転はモバイル Os で標準的な機能です。 Android では、XML またはプログラムによってコード内で、ユーザー インターフェイスを宣言して作成するかどうかに、アプリケーション内での回転を処理するため、高度なフレームワークを提供します。 回転したデバイスでの宣言型のレイアウト変更を自動的に処理する場合、アプリケーションは、Android リソース システムと緊密に統合を活用できます。 プログラムによるレイアウトの場合は、変更を手動で処理する必要があります。 これにより、さらに細かく制御、実行時より多くの作業を犠牲にして、開発者向けです。 アプリケーションは、アクティビティの再起動からオプトアウトおよび向きの変更の手動で制御を取得する選択もできます。

このガイドでは、次のトピックの印刷の向きが調べられます。

-   **宣言型のレイアウト回転** &ndash; Android リソース システムを使用して、レイアウトと特定の向きのドロウアブルの両方を読み込む方法を含め、印刷の向きに対応するアプリケーションを作成する方法です。

-   **プログラムによるレイアウト回転**&ndash;コントロールをプログラムで追加する方法と印刷の向きの変更を手動で処理する方法です。


## <a name="handling-rotation-declaratively-with-layouts"></a>レイアウトを使用して宣言によって処理回転

名前付け規則に従う必要のあるフォルダーにファイルを含めることで Android では、方向が変更されたときに、適切なファイルが自動的に読み込みます。
これには、サポートが含まれます。

-   *レイアウト リソース*&ndash;レイアウト ファイルの向きごとに増大するかを指定します。

-   *ドロウアブル リソース*&ndash;の向きごとに読み込まれるどのドロウアブルを指定します。


### <a name="layout-resources"></a>レイアウトのリソース

既定では、Android XML (AXML) ファイルが含まれている、**リソース/レイアウト**アクティビティのフォルダー ビューのレンダリングのために使用されます。 このフォルダーのリソースは、ランドス ケープの専用のリソースの追加のレイアウトが指定されていない場合、方向を縦長と横長の両方に使用されます。 既定のプロジェクト テンプレートによって作成されたプロジェクトの構造を考慮してください。

[![既定のプロジェクト テンプレートの構造](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

このプロジェクトは 1 つ作成**Main.axml**ファイルで、**リソース/レイアウト**フォルダーです。 ときに、アクティビティの`OnCreate`メソッドが呼び出されるで定義されているビューを拡張**Main.axml、** XML を次に示すように宣言ボタン。

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

横向きに、アクティビティのかどうか、デバイスを回転`OnCreate`メソッドが再度呼び出さと同じ**Main.axml**次のスクリーン ショットに示すように、ファイルが大ききます。

[![しかし、同じ画面の向きを横](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>印刷の向きに固有のレイアウト

レイアウト フォルダーに加え (既定値は縦方向とも明示的に名前を指定できます*レイアウト ポート*という名前のフォルダーを含めることによって`layout-land`)、アプリケーションはコードなしのランドス ケープのときに必要なビューを定義できます変更します。

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

かどうかフォルダーの名前を含む、追加のレイアウト土地**Main.axml**ランドス ケープのときに、レイアウトを膨張させるようになりましたが新しく追加されたを読み込む Android プロジェクトにファイルが追加された**[main.axml] です。** 横のバージョンを検討してください、 **Main.axml**を次のコードを含むファイル (わかりやすくするため、この XML は、コードの既定の縦のバージョンに似ていますで別の文字列を使用して、 `TextView`)。

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

このコードを実行し、縦から横に、デバイスの回転は、次のように、新しい XML を読み込みを示しています。

[![縦向きモードの印刷縦長と横長のスクリーン ショット](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)


### <a name="drawable-resources"></a>ドロウアブル リソース

回転時に Android 同様にレイアウトのリソースにドロウアブル リソースを処理します。 この場合、システムがからドロウアブルを取得、**リソース/描画**と**リソース/ドロウアブル土地**フォルダー、それぞれします。

たとえば、プロジェクトにはで Monkey.png をという名前のイメージが含まれています、**リソース/描画**場所ドロウアブルから参照されるフォルダー、`ImageView`次のような XML で。

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

さらにあると仮定しましょう別のバージョンの**Monkey.png**下を含める**リソース/ドロウアブル土地**です。 レイアウト ファイルが、デバイスが場合と同様、回転ドロウアブル変更の特定の向きでは、次のように。

[![別のバージョンの縦方向と横モードで表示される Monkey.png](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>回転をプログラムで処理

場合によってコードでのレイアウトを定義します。 さまざまな理由により、技術的な制限により、開発者の基本設定などを含む可能性があります。プログラムによってコントロールを追加する際、アプリケーションがデバイスの方向は、XML リソースを使用する場合は自動的に処理を考慮手動で必要があります。


### <a name="adding-controls-in-code"></a>コードでコントロールを追加します。

コントロールを追加するプログラムで、アプリケーションは、次の手順を実行する必要があります。

-  レイアウトを作成します。
-  レイアウトのパラメーターを設定します。
-  コントロールを作成します。
-  コントロールのレイアウトのパラメーターを設定します。
-  コントロールをレイアウトに追加します。
-  コンテンツ ビューとして、レイアウトを設定します。

たとえば、1 つで構成されるユーザー インターフェイス`TextView`に追加したコントロール、`RelativeLayout`次のコードに示すように、します。

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

このコードのインスタンスを作成、`RelativeLayout`クラスとセットをその`LayoutParameters`プロパティです。 `LayoutParams`クラスは、Android の再利用可能な方法でコントロールを配置する方法をカプセル化する方法です。 レイアウトのインスタンスが作成されると、コントロールを作成および追加できます。 コントロールが持つも`LayoutParameters`、ように、`TextView`この例ではします。 後に、`TextView`作成されるに追加すること、`RelativeLayout`と設定、`RelativeLayout`でアプリケーションを表示して、コンテンツ ビューの結果として、`TextView`ように。

[![縦長と横長の両方のモードで示す増分カウンター ボタン](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>コード内の方向を検出します。

アプリケーションがの向きごとに別のユーザー インターフェイスをロードしようとしたかどうかと`OnCreate`は呼び出されます (これが発生するたびに、デバイスの回転)、印刷の向きを検出し、目的のユーザー インターフェイスのコードを読み込む必要がありますか。 Android と呼ばれるクラスがある、 `WindowManager`、経由で現在のデバイスの回転を使用できる、`WindowManager.DefaultDisplay.Rotation`プロパティ、次のようにします。

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

このコードを設定、`TextView`最上部からピクセルが、新しいレイアウトを次に示すように、横に回転したときに自動的にアニメーション化、画面の左位置指定の 100 にします。

[![縦方向と横モード ビュー状態が保持されます。](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>アクティビティの再起動の回避

内のすべての処理だけでなく`OnCreate`、アプリケーションもできなくなるアクティビティを設定して、方向が変更されたときに再起動されている`ConfigurationChanges`で、`ActivityAttribute`次のようにします。

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
これでデバイスを回転したときに、アクティビティは再起動されません。 ここで向きの変更を手動で処理をするために、アクティビティをオーバーライドできます、`OnConfigurationChanged`メソッドからの向きを決定し、`Configuration`アクティビティの下の新しい実装と同様に、渡されるオブジェクト。

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

ここで、`TextView's`縦と横の両方のレイアウトのパラメーターが初期化されます。 クラスの変数では、パラメーターを保持と共に、`TextView`自体であるため、方向が変更されたときに、活動が再作成されないためです。 コードがまだ使用して、`surfaceOrientartion`で`OnCreate`の初期レイアウトを設定する、`TextView`です。 その後、`OnConfigurationChanged`すべての後続のレイアウト変更を処理します。

アプリケーションを実行したときに、Android は、デバイスの回転が発生し、アクティビティが再起動しないと、ユーザー インターフェイスの変更を読み込みます。


## <a name="preventing-activity-restart-for-declarative-layouts"></a>レイアウトの宣言型アクティビティの再起動の回避

XML のレイアウトを定義している場合、デバイスの回転によってアクティビティの再起動を回避もできます。 アクティビティ再起動を回避する場合、この方法を使用おなど、(パフォーマンス上の理由から、おそらく) し、さまざまな向きに対して新しいリソースを読み込む必要はありません。

これを行うをプログラムでのレイアウトで使用する同じ手順に従います。 設定するだけ`ConfigurationChanges`で、`ActivityAttribute`行ったように、`CodeLayoutActivity`前です。 すべてのコードをでもう一度向きの変更を実装するを実行する必要がありますが、`OnConfigurationChanged`メソッドです。


## <a name="maintaining-state-during-orientation-changes"></a>印刷の向きの変更時に状態を維持します。

宣言またはプログラムによっては、回転を処理するかどうか、すべての Android アプリケーションは、デバイスの方向が変更されたときに状態を管理すると同じ手法を実装する必要があります。 Android デバイスを回転したときに、システムが実行中のアクティビティを再起動するため、状態を管理することが重要です。 Android は、レイアウトとは、特定の向きの専用に設計されたドロウアブルなど、代替のリソースを読み込む簡単にします。 再起動すると、アクティビティには、クラスのローカル変数に格納されている可能性がありますが、一時的な状態が失われます。 したがって、アクティビティが依存して状態の場合は、アプリケーション レベルの状態を保持する必要があります。 処理する必要があるアプリケーションの保存と向きの変更を維持する必要がある任意のアプリケーション状態を復元します。

Android での永続化の状態の詳細についてを参照してください、[アクティビティのライフ サイクル](~/android/app-fundamentals/activity-lifecycle/index.md)ガイドです。


## <a name="summary"></a>まとめ

この記事では、Android の組み込みの機能を使用して、回転を操作する方法について説明します。 最初に、Android リソース システムを使用して、印刷の向きに対応アプリケーションを作成する方法を説明します。 次のコードにコントロールを追加する方法と印刷の向きの変更を手動で処理する方法が表示されます。



## <a name="related-links"></a>関連リンク

- [回転デモ (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [ランタイムの変更の処理](http://developer.android.com/guide/topics/resources/runtime-changes.html)
- [高速画面の向きの変更](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
