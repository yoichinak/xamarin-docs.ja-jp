---
title: Xamarin.Android のパフォーマンス
description: Xamarin.Android でビルドされたアプリケーションのパフォーマンスを高めるための方法は多数あります。 これらの方法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。 この記事では、これらの方法について説明します。
ms.prod: xamarin
ms.assetid: dc2e27f2-7f71-4d57-9cf9-165528276613
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e49c6d92d4c6bcabf45047321e183fe36d19096b
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454716"
---
# <a name="xamarinandroid-performance"></a>Xamarin.Android のパフォーマンス

_Xamarin.Android でビルドされたアプリケーションのパフォーマンスを高めるための方法は多数あります。これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。この記事では、これらの方法について説明します。_

## <a name="performance-overview"></a>パフォーマンスの概要

アプリケーションのパフォーマンス低下は、さまざまな挙動で現れます。 たとえば、アプリケーションが応答しなかったり、スクロールが遅くなったり、バッテリーの寿命が縮まったりすることがあります。 ただし、パフォーマンスを最適化するには、単に効率的なコードを実装するだけでは不十分です。 アプリケーション パフォーマンスに関わるユーザー エクスペリエンスも考慮する必要があります。 たとえば、操作の実行によって、ユーザーが他の操作を実行できない状況にならないようにすることで、ユーザー エクスペリエンスを改善できます。

Xamarin.Android でビルドされたアプリケーションのパフォーマンスとユーザーの体感パフォーマンスを高めるための方法は多数あります。 それには以下が含まれます。

- [レイアウト階層を最適化する](#optimizelayout)
- [リスト ビューを最適化する](#optimizelistviews)
- [アクティビティのイベント ハンドラーを削除する](#removeeventhandlers)
- [サービスの有効期間を制限する](#limitservices)
- [通知時にリソースを解放する](#releaseresources)
- [ユーザー インターフェイスが非表示の場合はリソースを解放する](#releaseresourcesuihidden)
- [イメージ リソースを最適化する](#optimizeimages)
- [未使用のイメージ リソースの破棄](#disposeimages)
- [浮動小数点演算の使用を避ける](#avoidfloats)
- [ダイアログを閉じる](#dismissdialogs)

> [!NOTE]
> この記事を読む前に、まず、「[Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)」(クロスプラットフォーム パフォーマンス) をお読みください。Xamarin プラットフォームを使用してビルドされたアプリケーションのメモリ使用量とパフォーマンスを改善するための、プラットフォーム固有ではない手法について説明されています。

<a name="optimizelayout"></a>

## <a name="optimize-layout-hierarchies"></a>レイアウト階層を最適化する

アプリケーションに追加された各レイアウトには、初期化、レイアウト、および描画が必要です。 `weight` パラメーターを使用する [`LinearLayout`](xref:Android.Widget.LinearLayout) インスタンスを入れ子にすると、レイアウト パスに影響する可能性があります。これは、子がそれぞれ 2 回測定されるためです。 `LinearLayout` の入れ子になったインスタンスを使用すると、ビュー階層が深くなり、その結果、[`ListView`](xref:Android.Widget.ListView) などの、複数回拡大されたレイアウトのパフォーマンスが低下する可能性があります。 したがって、このようなレイアウトを最適化することが重要です。そうすれば、パフォーマンスが向上します。

たとえば、アイコン、タイトル、および説明を含むリスト ビュー行の [`LinearLayout`](xref:Android.Widget.LinearLayout) について考えてみます。 `LinearLayout` には [`ImageView`](xref:Android.Widget.ImageView) と、2 つの [`TextView`](xref:Android.Widget.TextView) インスタンスを含む縦方向の `LinearLayout` が含まれます。

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="fill_parent">
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:gravity="center_vertical"
            android:text="Mei tempor iuvaret ad." />
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:singleLine="true"
            android:ellipsize="marquee"
            android:text="Lorem ipsum dolor sit amet." />
    </LinearLayout>
</LinearLayout>
```

このレイアウトは 3 レベルの深さであり、各 [`ListView`](xref:Android.Widget.ListView) 行で拡大しても無駄になります。 ただし、以下のコード例のように、レイアウトをフラット化することで改善できます。

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_alignParentTop="true"
        android:layout_alignParentBottom="true"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <TextView
        android:id="@+id/secondLine"
        android:layout_width="fill_parent"
        android:layout_height="25dip"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:singleLine="true"
        android:ellipsize="marquee"
        android:text="Lorem ipsum dolor sit amet." />
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_above="@id/secondLine"
        android:layout_alignWithParentIfMissing="true"
        android:gravity="center_vertical"
        android:text="Mei tempor iuvaret ad." />
</RelativeLayout>
```

前の 3 レベルの階層は 2 レベルの階層に減り、1 つの [`RelativeLayout`](xref:Android.Widget.RelativeLayout) で 2 つの [`LinearLayout`](xref:Android.Widget.LinearLayout) インスタンスが置き換えられています。 各 [`ListView`](xref:Android.Widget.ListView) 行のレイアウトを拡大すると、パフォーマンスが大幅に向上します。

<a name="optimizelistviews"></a>

## <a name="optimize-list-views"></a>リスト ビューを最適化する

ユーザーは、[`ListView`](xref:Android.Widget.ListView) インスタンスのスムーズなスクロールと読み込み時間の短縮を期待します。 ただし、各リスト ビュー行に深い入れ子のビュー階層が含まれる場合、またはリスト ビュー行に複雑なレイアウトが含まれる場合、スクロールのパフォーマンスは低下する可能性があります。 ただし、`ListView` のパフォーマンス低下を回避するために使用できる次のような手法があります。

- 行ビューを再利用する。詳細については、「[行ビューを再利用する](#reuserowviews)」を参照してください。
- 可能な場合は、レイアウトをフラット化する。
- Web サービスから取得される行のコンテンツをキャッシュする。
- イメージのスケーリングを避ける。

これらの手法をすべて使用することで、[`ListView`](xref:Android.Widget.ListView) インスタンスのスクロールをスムーズに保つことができます。

<a name="reuserowviews"></a>

### <a name="reuse-row-views"></a>行ビューを再利用する

[`ListView`](xref:Android.Widget.ListView) で数百行を表示する場合、少数のみが 1 画面に表示されるなら、数百単位の [`View`](xref:Android.Views.View) オブジェクトを作成するのはメモリの無駄です。 代わりに、画面の行に表示される `View` オブジェクトのみをメモリに読み込み、その再利用されるオブジェクトに**コンテンツ**を読み込むことができます。 こうすることで、数百単位の追加オブジェクトのインスタンス化を防ぎ、時間とメモリを節約することができます。

そのため、次のコード例のように、行が画面から消えるときに、そのビューを再利用のためのキューに格納できます。

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

ユーザーがスクロールすると、[`ListView`](xref:Android.Widget.ListView) は `GetView` オーバーライドを呼び出して、表示する新しいビューを要求します。使用可能な場合は、`convertView` パラメーターに未使用のビューを渡します。 この値が `null` の場合、コードで新しい [`View`](xref:Android.Views.View) インスタンスが作成されます。それ以外の場合は、`convertView` プロパティをリセットして再利用できます。

詳細については、「[Populating a ListView with Data](~/android/user-interface/layouts/list-view/populating.md)」 (ListView へのデータの設定) の「[Row View Re-Use](~/android/user-interface/layouts/list-view/populating.md#row-view-re-use)」 (行ビューの再利用) を参照してください。

<a name="removeeventhandlers"></a>

## <a name="remove-event-handlers-in-activities"></a>アクティビティのイベント ハンドラーを削除する

Android ランタイムでアクティビティが破棄されても、Mono ランタイムでは有効な場合があります。 したがって、`Activity.OnPause` の外部オブジェクトに対するイベント ハンドラーを削除して、破棄されたアクティビティへの参照がランタイムで保持されないようにします。

次のように、アクティビティで、クラス レベルでイベント ハンドラー (複数可) を宣言します。

```csharp
EventHandler<UpdatingEventArgs> service1UpdateHandler;
```

次に、`OnResume` などの、アクティビティでハンドラーを実装します。

```csharp
service1UpdateHandler = (object s, UpdatingEventArgs args) => {
    this.RunOnUiThread (() => {
        this.updateStatusText1.Text = args.Message;
    });
};
App.Current.Service1.Updated += service1UpdateHandler;
```

アクティビティの実行状態の終了時に、`OnPause` が呼び出されます。 `OnPause` 実装では、次のようにハンドラーを削除します。

```csharp
App.Current.Service1.Updated -= service1UpdateHandler;
```

<a name="limitservices"></a>

## <a name="limit-the-lifespan-of-services"></a>サービスの有効期間を制限する

サービスの開始時に、Android ではサービス プロセスが実行されたままとなります。 これにより、プロセスの負荷が高くなります。そのメモリをページングできないか、他の場所で使用できないためです。 必要でないときにサービスを実行したままにすると、メモリ制約により、アプリケーションのパフォーマンスが低下するリスクが増大します。 また、Android でキャッシュできるプロセスの数が減るため、アプリケーションの切り替え効率が悪くなる可能性があります。

サービスの有効期限は、`IntentService` を使用して制限することができます。これにより、開始元のインテントの処理後にサービス自体が終了します。

<a name="releaseresources"></a>

## <a name="release-resources-when-notified"></a>通知時にリソースを解放する

アプリケーションのライフサイクル中に、[`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*) コールバックにより、デバイスのメモリが少なくなったときに通知が提供されます。 次のメモリ レベルの通知をリッスンするには、このコールバックを実装する必要があります。

- [`TrimMemoryRunningModerate`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningModerate) – アプリケーションはいくつかの不要なリソースを解放することが*できます*。
- [`TrimMemoryRunningLow`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningLow) – アプリケーションは不要なリソースを解放する*必要があります*。
- [`TrimMemoryRunningCritical`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningCritical) – アプリケーションは、可能な限り多くの重要でないプロセスを解放する*必要があります*。

さらに、アプリケーション プロセスがキャッシュされると、次のメモリ レベルの通知が [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*) コールバックで受信される場合があります。

- [`TrimMemoryBackground`](xref:Android.Content.ComponentCallbacks2.TrimMemoryBackground) – ユーザーがアプリに戻った場合に、すばやく効率的に再ビルドできるリソースを解放します。
- [`TrimMemoryModerate`](xref:Android.Content.ComponentCallbacks2.TrimMemoryModerate) – リソースを解放することで、他のプロセスをキャッシュに保持でき、全体的なパフォーマンスが向上します。
- [`TrimMemoryComplete`](xref:Android.Content.ComponentCallbacks2.TrimMemoryComplete) – 多くのメモリがすぐに回復されない場合は、アプリケーション プロセスがすぐに終了します。

受信したレベルに基づいてリソースを解放することで、通知に応答する必要があります。

<a name="releaseresourcesuihidden"></a>

## <a name="release-resources-when-the-user-interface-is-hidden"></a>ユーザー インターフェイスが非表示の場合はリソースを解放する

ユーザーが別のアプリに移動したときに、アプリのユーザー インターフェイスで使用されているすべてのリソースを解放します。キャッシュされたプロセスに対する Android の容量が大幅に増加する場合があり、これにより、ユーザー エクスペリエンスの品質に影響する可能性があります。

ユーザーが UI を終了するときに通知を受信するには、`Activity` クラスで [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*) コールバックを実装し、UI が非表示になっていることを示す [`TrimMemoryUiHidden`](xref:Android.Content.ComponentCallbacks2.TrimMemoryUiHidden) レベルをリッスンします。 この通知を受信するのは、アプリケーションの UI コンポーネントが*すべて* ユーザーに対して非表示になった場合のみです。 この通知の受信時に UI リソースを解放すると、ユーザーがアプリの別のアクティビティから戻った場合に、UI リソースを引き続き使用して、アクティビティをすばやく再開できます。

<a name="optimizeimages"></a>

## <a name="optimize-image-resources"></a>イメージ リソースを最適化する

アプリケーションが使用するリソースのうち最もコストが高いものとして画像があります。多くの場合、画像は高解像度でキャプチャされます。 したがって、画像を表示する場合は、デバイスの画面に必要な解像度で表示します。 画像の解像度が画面よりも高い場合は、スケール ダウンする必要があります。

詳細については、「[Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)」(クロスプラットフォーム パフォーマンス) ガイドの「[Optimize Image Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)」(イメージ リソースの最適化) を参照してください。

<a name="disposeimages"></a>

## <a name="dispose-of-unused-image-resources"></a>未使用のイメージ リソースの破棄

メモリの使用量を節約するには、不要になったサイズの大きいイメージ リソースは破棄することをお勧めします。 ただし、正しくイメージを破棄することが重要です。 明示的な `.Dispose()` 呼び出しを使用せず、[using](/dotnet/csharp/language-reference/keywords/using-statement) ステートメントを使用すると、`IDisposable` オブジェクトを正しく使用できます。 

たとえば、[Bitmap](xref:Android.Graphics.Bitmap) クラスは `IDisposable` を実装します。 `using` ブロックに `BitMap` オブジェクトのインスタンスをラップすると、このブロックを終了するときに、それが正しく破棄されることが保証されます。

```csharp
using (Bitmap smallPic = BitmapFactory.DecodeByteArray(smallImageByte, 0, smallImageByte.Length))
{
    // Use the smallPic bit map here
}
```

破棄可能なリソースの解放の詳細については、「[IDisposable のリソースを解放する](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable)」を参照してください。  

<a name="avoidfloats"></a>

## <a name="avoid-floating-point-arithmetic"></a>浮動小数点演算の使用を避ける

Android デバイスでは、浮動小数点演算が、整数演算の場合より約 2 倍遅くなります。 そのため、可能な場合は、浮動小数点演算を整数演算に置き換えてください。 ただし、最新のハードウェアでは `float` と `double` の演算の実行時間に違いはありません。

> [!NOTE]
> 整数演算の場合でも、CPU によってはハードウェアで除算できないこともあります。 したがって、多くの場合、整数除算と剰余演算はソフトウェアで実行されます。

<a name="dismissdialogs"></a>

## <a name="dismiss-dialogs"></a>ダイアログを閉じる

[`ProgressDialog`](xref:Android.App.ProgressDialog) クラス (または任意のダイアログやアラート) を使用する場合は、ダイアログの目的が完了したときに [`Hide`](xref:Android.App.Dialog.Hide*) メソッドを呼び出す代わりに、[`Dismiss`](xref:Android.App.Dialog.Dismiss*) メソッドを呼び出します。 それ以外の場合、ダイアログは引き続き有効であるため、アクティビティへの参照が保持され、そのアクティビティがリークされます。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Android でビルドされたアプリケーションのパフォーマンスを高めるための手法について説明しました。 これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのパフォーマンス](~/cross-platform/deploy-test/memory-perf-best-practices.md)