---
title: アクティビティのライフサイクル
description: アクティビティは、Android アプリケーションの基本的なビルディング ブロックと、さまざまな異なる状態で存在することができます。 アクティビティのライフ サイクルは、インスタンス化で始まり、破棄で終わると間に多くの状態が含まれています。 アクティビティの状態が変わるときに、適切なライフ サイクル イベント メソッドは起こる状態のアクティビティに通知され、その変更に合わせて調整するコードを実行するように呼び出されます。 この記事は、アクティビティのライフ サイクルを検査し、責任について説明します、適切に動作で信頼性の高いアプリケーションの一部としてこれらの状態変更の各アクティビティがあります。
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: f35f3e59d8b669795ade3d370894e45866cea1ff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="activity-lifecycle"></a>アクティビティのライフサイクル

_アクティビティは、Android アプリケーションの基本的なビルディング ブロックと、さまざまな異なる状態で存在することができます。アクティビティのライフ サイクルは、インスタンス化で始まり、破棄で終わると間に多くの状態が含まれています。アクティビティの状態が変わるときに、適切なライフ サイクル イベント メソッドは起こる状態のアクティビティに通知され、その変更に合わせて調整するコードを実行するように呼び出されます。この記事は、アクティビティのライフ サイクルを検査し、責任について説明します、適切に動作で信頼性の高いアプリケーションの一部としてこれらの状態変更の各アクティビティがあります。_

## <a name="activity-lifecycle-overview"></a>アクティビティ ライフ サイクルの概要

アクティビティは、特殊なプログラミング概念 Android に固有です。 従来のアプリケーション開発で通常は静的 main メソッドを実行して、アプリケーションを起動します。 Android とは、点が異なっています。Android アプリケーションは、アプリケーション内で登録されているアクティビティを使用して起動できます。 実際には、ほとんどのアプリケーションはアプリケーションのエントリ ポイントとして指定されている特定のアクティビティのみがあります。 ただし、アプリケーションがクラッシュした場合、または終了が OS で、OS がオープンの最後のアクティビティにアプリケーションや、以前の活動スタック内でその他の場所の再起動を試みることができます。
さらに、OS はアクティブでない場合は、活動を一時停止し、メモリ不足である場合は、それらを解放可能性があります。 アプリケーションの状態を正しく復元アクティビティが再起動される場合に特にアクティビティは、前の活動からのデータに依存していることを許可する、慎重に検討を行わなければなりません。

アクティビティのライフ サイクルは、アクティビティのライフ サイクル全体を通じて、OS のメソッド呼び出しのコレクションとして実装されます。 これらのメソッドでは、開発者は各自のアプリケーションの状態とリソース管理の要件を満たすために必要な機能を実装できるようにします。

実装する必要なアクティビティのライフ サイクルによって公開されるメソッドを各アクティビティの要件を分析するアプリケーション開発者にとって非常に重要です。 この作業を怠ると、アプリケーションが不安定になったり、クラッシュ、リソースの肥大化、および基になる可能性がありますも OS が不安定になるがあります。

この章の検査の詳細、アクティビティのライフ サイクルを含みます。

-  アクティビティの状態
-  ライフ サイクル メソッド
-  アプリケーションの状態を保持


このセクションの内容も含まれています、[チュートリアル](~/android/app-fundamentals/activity-lifecycle/saving-state.md)アクティビティのライフ サイクル中に状態を効率的に保存する方法の実際の例を提供します。 この章の末尾では、アクティビティのライフ サイクルと Android アプリケーションでサポートする方法の理解が必要です。

## <a name="activity-lifecycle"></a>アクティビティのライフサイクル

Android のアクティビティのライフ サイクルには、リソース管理フレームワークを開発者に提供するアクティビティ クラス内で公開されるメソッドのコレクションが構成されています。 このフレームワークでは、アプリケーション内の各活動の一意の状態管理の要件を満たしているし、リソース管理を適切に処理をすることができます。

### <a name="activity-states"></a>アクティビティの状態

Android OS では、アクティビティの状態に基づいてを介しです。 これにより、Android、OS メモリおよびリソースを再利用を許可する使用されなくなったアクティビティを識別します。 次の図は、その有効期間中にアクティビティを通過できる状態を示しています。

[![アクティビティ状態図](images/image1-sml.png)](images/image1.png#lightbox)

これらの状態は、4 つの主要グループに次のように分類されます。

1.  *作業中または実行*&ndash;活動がアクティブと見なされますまたは、フォア グラウンドを実行して、スタックの一番上のアクティビティとも呼ばれます。 これは、android での優先順位の高いアクティビティと見なされますようのみ強制終了されます極端な状況で OS によってとなど場合と、アクティビティより多くのメモリを使用しようとしています。 デバイスで使用できるようにこの、UI が応答しなくなる可能性があります。

1.  *一時停止している*&ndash;アクティビティと見なされます、デバイスは、スリープ状態またはアクティビティが引き続き表示されますが、新しい、フル サイズではないまたは透過的なアクティビティが部分的に非表示、一時停止します。 一時停止中のアクティビティはまだ動作しているすべての状態とメンバーの情報の管理し、ウィンドウ マネージャーに接続したまま、つまり、します。 これは、2 番目であると見なされますの優先順位の高いアクティビティ Android で、どのようなのみに中止となりますが OS でこのアクティビティを強制終了、安定性と応答性の高いアクティブ/実行されるアクティビティを保持するために必要なリソース要件を満たしている場合。

1.  *停止/Backgrounded* &ndash;停止中またはバック グラウンドで別のアクティビティによって完全に隠されている活動と見なされます。
    停止しているアクティビティをまだ限り可能ですが、停止のアクティビティは 3 つの状態の最も低い優先順位と見なされ、OS がリソースを満たすために最初にこの状態のアクティビティを終了するような場合の状態とメンバーの情報を保持する再試行してください。高い優先度の活動の要件。

1.  *再起動*&ndash;がどこからであるアクティビティが Android によってメモリから削除するライフ サイクルの停止に一時停止します。 場合は、ユーザーが移動するのにそれを再起動する必要があります、アクティビティは、以前に保存した状態に復元し、ユーザーに表示されます。


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>アクティビティの構成の変更に応答の再作成

複雑なことをするためには、Android には、構成の変更と呼ばれる、ミックスで 1 つ以上レンチをスローします。 構成の変更は迅速なアクティビティ破棄/再 creation のサイクルが、デバイスの場合などのアクティビティの構成が変更されたときに発生する[回転](~/android/app-fundamentals/handling-rotation.md)(およびアクティビティは、横または縦向きに再ビルドを取得する必要がありますモード) と、キーボードが表示されます (それ自体のサイズを変更する営業案件のアクティビティが表示されます)、またはその他、ドッキング ステーションにデバイスを配置するとします。

でも、構成の変更には、停止して再起動するアクティビティの間に行われるのと同じアクティビティの状態の変更が生じる。 ただし、ことを確認して、アプリケーションが応答性の高い構成の変更時に実行するには、するためにこれが可能な限り早く処理することが重要です。 Android では、このため、構成を変更するときの状態を維持するために使用する特定の API がします。
この後で取り上げる、[管理状態全体にわたって、ライフ サイクル](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle)セクションです。

### <a name="activity-lifecycle-methods"></a>アクティビティのライフ サイクル メソッド

Android SDK、拡張機能によって Xamarin.Android フレームワークは、アプリケーション内の活動の状態を管理する強力なモデルを提供します。 アクティビティの状態を変更する場合は、アクティビティが、OS では、そのアクティビティの特定のメソッドを呼び出してによって通知されます。 次の図は、アクティビティのライフ サイクルへのリレーションシップでこれらのメソッドを示しています。

[![アクティビティ ライフ サイクルのフローチャート](images/image2-sml.png)](images/image2.png#lightbox)

開発者は、アクティビティ内でこれらのメソッドをオーバーライドすることで、状態の変更を処理できます。 ただし、すべてのライフ サイクル メソッドが UI スレッドで呼び出され、新しいアクティビティなどを表示する現在のアクティビティを非表示にするなどの UI の作業の次の項目の実行から OS をブロックに重要です。そのため、これらのメソッドでコードがもを実行すると思われるアプリケーションの作成をできるだけ短くする必要があります。 実行時間の長いタスクは、バック グラウンド スレッドで実行する必要があります。

これらのライフ サイクル メソッドとその使用法の各を調べてみましょう。

#### <a name="oncreate"></a>OnCreate

[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)アクティビティが作成されたときに呼び出される最初のメソッドです。
`OnCreate` 常にオーバーライドすることなどのアクティビティによって必要とされるすべてのスタートアップ初期化を実行します。

-  ビューを作成します。
-  変数の初期化
-  リストへの静的なデータをバインディング


`OnCreate` [バンドル](https://developer.xamarin.com/api/type/Android.OS.Bundle/)パラメーターを格納して、バンドルが null でない場合のアクティビティ間で状態情報とオブジェクトを渡すのためのディクショナリは、これを示し、アクティビティが再開されてからの状態を復元する必要があります、以前のインスタンス。 次のコードでは、バンドルから値を取得する方法を示します。

```csharp
protected override void OnCreate(Bundle bundle)
{
   base.OnCreate(bundle);

   string intentString;
   bool intentBool;

   if (bundle != null)
   {
      intentString = bundle.GetString("myString");
      intentBool = bundle.GetBoolean("myBool");
   }

   // Set our view from the "main" layout resource
   SetContentView(Resource.Layout.Main);
}
```

1 回`OnCreate`が終了し、Android が呼び出す`OnStart`です。

#### <a name="onstart"></a>OnStart

[OnStart](https://developer.xamarin.com/api/member/Android.App.Activity.OnStart/)後にシステムによって常に呼び出される`OnCreate`が終了します。 アクティビティは、アクティビティ、アクティビティ内でビューの現在の値を更新するなどで表示するには、特定のタスクの権限をすべてを実行する必要がある場合、このメソッドをオーバーライド可能性があります。 Android が呼び出す`OnResume`このメソッドの直後にします。

#### <a name="onresume"></a>OnResume

システム コール[OnResume](https://developer.xamarin.com/api/member/Android.App.Activity.OnResume/)アクティビティがユーザーとの対話を開始する準備がの場合。
アクティビティは、タスクを実行するようには、このメソッドをオーバーライドする必要があります。

-  フレーム レート (ゲームの構築の一般的なタスク) の急増
-  アニメーションの開始
-  GPS の更新プログラムのリッスン
-  すべての関連するアラートまたはダイアログを表示します。
-  外部のイベント ハンドラーを


例としては、次のコード スニペットは、カメラを初期化する方法を示します。

```csharp
public void OnResume()
{
    base.OnResume(); // Always call the superclass first.

    if (_camera==null)
    {
        // Do camera initializations here
    }
}
```

`OnResume` すべての操作であるためには重要で行われます`OnPause`で ne 解除する必要があります`OnResume`後に実行することが保証する唯一のライフ サイクル メソッドであるため、`OnPause`にするときに、アクティビティを現実に戻る。

#### <a name="onpause"></a>OnPause

[OnPause](https://developer.xamarin.com/api/member/Android.App.Activity.OnPause/)システムは、アクティビティを配置する背景を使用するか、アクティビティは部分的に覆い隠さ時に呼び出されます。 アクティビティは、する必要がある場合、このメソッドをオーバーライドする必要があります。

-   永続的なデータに未保存の変更をコミットします。

-   破棄またはリソースを消費する他のオブジェクトをクリーンアップします。

-   フレーム レートと一時停止のアニメーションをごとの傾斜増加

-   外部のイベント ハンドラーまたは (つまり、サービスに関連付けられているもの) の通知のハンドラーの登録を解除します。 これは、アクティビティのメモリ リークを防ぐために行う必要があります。

-   同様に、表示する場合は、アクティビティが、ダイアログや警告、する必要があるクリーンアップされましたが、`.Dismiss()`メソッドです。

例として、次のコード スニペットは、カメラ、解放アクティビティが行うことはできませんの一時停止中にそれを使用します。

```csharp
public void OnPause()
{
    base.OnPause(); // Always call the superclass first

    // Release the camera as other activities might need it
    if (_camera != null)
    {
        _camera.Release();
        _camera = null;
    }
}
```

2 つの可能なライフ サイクル メソッドの後に呼び出される`OnPause`:

1.  `OnResume` アクティビティが前面に返される場合は、呼び出されます。
1.  `OnStop` バック グラウンドでアクティビティを配置する場合が呼び出されます。


#### <a name="onstop"></a>OnStop

[OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/)は、アクティビティがユーザーに表示されない場合に呼び出されます。 これは、次のいずれかが発生したときに発生します。

-  新しい活動が開始しています、このアクティビティを隠しています。
-  既存の活動は、前面に表示になっています。
-  アクティビティを破棄しています。


`OnStop` いない常に呼び出すことは Android がリソースが不足しているし、アクティビティがバック グラウンド正しくできませんなど、メモリ不足の状況で。 このため、お勧めに依存しないように`OnStop`破棄のアクティビティを準備するときに呼び出されました。 この後に呼び出すことが次のライフ サイクル メソッド`OnDestroy`場合は、アクティビティが消滅または`OnRestart`アクティビティは、ユーザーとの対話に戻る場合。

#### <a name="ondestroy"></a>OnDestroy

[OnDestroy](https://developer.xamarin.com/api/member/Android.App.Activity.OnDestroy/)破棄され、メモリから完全に削除されますが、前に、アクティビティ インスタンスに対して呼び出される最終的なメソッドです。 極端な状況では、Android が原因となるアクティビティをホストしているアプリケーション プロセスを終了可能性があります`OnDestroy`中は呼び出されません。 ほとんどをクリーンアップして、シャット ダウンが行われているために、ほとんどアクティビティはこのメソッドを実装できませんが、`OnPause`と`OnStop`メソッドです。 `OnDestroy`通常メソッドは時間の長いをクリーンアップするリソースを実行している可能性がありますがリソースをリークします。 この例で開始されたバック グラウンド スレッドがあります`OnCreate`です。

アクティビティが破棄された後に呼び出されますライフ サイクル メソッドされません。

#### <a name="onrestart"></a>OnRestart

[OnRestart](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestart/)は、アクティビティが停止されて、再度開始する前にした後に呼び出されます。 これの良い例は、ユーザーがアプリケーションでアクティビティ中に、[ホーム] ボタンを押したときになります。 その場合、`OnPause`し`OnStop`メソッドが呼び出されると、アクティビティがバック グラウンドに移動しますが、破棄されることはありません。 ユーザーが Android を呼び出し、タスク マネージャーまたは類似したアプリケーションを使用してアプリケーションを復元する場合は、`OnRestart`アクティビティのメソッドです。

一般的なガイドラインでどのような種類のロジックを実装する必要がない`OnRestart`です。 これは、ため`OnStart`アクティビティが作成されているかどうかに関係なく常に起動または再起動中で、アクティビティに必要なすべてのリソースを初期化する必要がありますので`OnStart`、なく`OnRestart`です。

次のライフ サイクル メソッドの後に呼び出されます`OnRestart`なります`OnStart`です。

### <a name="back-vs-home"></a>Vs をバックアップします。Home

多くの Android デバイスがある 2 つの個別のボタン:「戻る」ボタンと [ホーム] ボタン。 この例は、Android 4.0.3 以降の次のスクリーン ショットに表示されることができます。

[![戻るボタンとホーム ボタン](images/image4-sml.png)](images/image4.png#lightbox)

2 つのボタンの微妙な違いがある、バック グラウンドでアプリケーションを配置するのと同じ効果が表示される場合でもです。 ユーザーは、[戻る] ボタンをクリックすると、それらはという Android アクティビティを終了することです。 Android、アクティビティが破棄されます。 これに対し、ユーザーは、[ホーム] ボタンをクリックすると、アクティビティは単に配置されます、バック グラウンド&ndash;Android は、アクティビティを中止しません。

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>ライフ サイクル全体の状態を管理します。

アクティビティが停止しているか、破棄されるときに、システムは、後で復元の動作状況の状態を保存する機会を提供します。
この状態が保存されているが、インスタンスの状態と呼ばれます。 Android では、アクティビティのライフ サイクル中にインスタンスの状態を格納するための 3 つのオプションを提供します。

1. プリミティブ値を格納する、`Dictionary`と呼ばれる、[バンドル](https://developer.xamarin.com/api/type/Android.OS.Bundle/)状態を保存する Android を使用します。

1. カスタム クラスを作成すると、ビットマップなどの複雑な値が保持されます。 Android では、このカスタム クラスを状態の保存に使用します。

1. 構成の変更のライフ サイクルを回避する方法と仮定した場合は、アクティビティの状態を保持する責任を完了します。


このガイドでは、最初の 2 つのオプションについて説明します。



### <a name="bundle-state"></a>バンドルの状態

インスタンスの状態を保存するための主なオプションと呼ばれるキーと値の辞書オブジェクトを使用して、[バンドル](https://developer.xamarin.com/api/type/Android.OS.Bundle/)です。
アクティビティが作成されたときにことに注意してください、`OnCreate`バンドル メソッドは、パラメーターとして渡されます、このバンドルは、インスタンスの状態を復元するために使用できます。 複雑なデータをすばやくしませんまたはキー/値のペア (ビットマップ); のように簡単にシリアル化のバンドルを使用するには使用しないでください。代わりに、単純な値の文字列のように使用する必要があります。

アクティビティは、保存して、バンドル内のインスタンス状態の取得に役立ちますメソッドを提供します。

-   [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) &ndash;アクティビティが破棄されるときに Android によって呼び出されるこのです。 アクティビティは、キー/値の状態の項目を保持する必要がある場合、このメソッドを実装できます。

-   [OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) &ndash;後にこれと呼ばれますが、`OnCreate`メソッドが終了し、初期化が完了した後、その状態を復元するアクティビティの別の機会を提供します。

次の図は、これらのメソッドの使用方法を示しています。

[![バンドルの状態のフローチャート](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/)ように、アクティビティを停止していますが、呼び出されます。 アクティビティの状態を格納できるバンドル パラメーターが適用されます。 デバイス構成の変更が発生しているときに、アクティビティに使用できる、`Bundle`をオーバーライドすることで、アクティビティの状態を保持する場合に渡されるオブジェクト`OnSaveInstanceState`です。 次に例を示します。

```csharp
int c;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  this.SetContentView (Resource.Layout.SimpleStateView);

  var output = this.FindViewById<TextView> (Resource.Id.outputText);

  if (bundle != null) {
    c = bundle.GetInt ("counter", -1);
  } else {
    c = -1;
  }

  output.Text = c.ToString ();

  var incrementCounter = this.FindViewById<Button> (Resource.Id.incrementCounter);

  incrementCounter.Click += (s,e) => {
    output.Text = (++c).ToString();
  };
}
```

上記のコードは、という名前の整数をインクリメント`c`ときという名前のボタン`incrementCounter`をクリックするで結果を表示する、`TextView`という`output`です。 構成の変更が行われる場合、たとえば、ときに、デバイスの回転 - の値は失われますが、上記のコード`c`ため、`bundle`なります`null`次の図に示すように、します。

[![前の値は表示されません。](images/07-sml.png)](images/07.png#lightbox)

値を保持するために`c`この例では、アクティビティをオーバーライドできます`OnSaveInstanceState`、次に示すように、バンドルに値を保存します。

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

ようになりました、デバイスが、新しい方向を回転させるとと、整数は、バンドルには保存され、行が取得されます。

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> 基本実装を常に重要な呼び出しである`OnSaveInstanceState`階層の表示の状態を保存することもできるようにします。



##### <a name="view-state"></a>ビューの状態

オーバーライドする`OnSaveInstanceState`は、上記の例では、カウンターなどの向きの変更の間でアクティビティに一時的なデータを保存するための適切なメカニズムです。 ただし、既定の実装`OnSaveInstanceState`くれます ui にすべてのビューでは、一時的なデータを保存する限り、各ビューには ID が割り当てられます。 たとえば、アプリケーションが、 `EditText` XML で次のように定義された要素。

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

以降、`EditText`コントロールが、`id`割り当て、ユーザーが一部のデータを入力し、デバイスの回転時に、データは引き続き表示されます、次のように。

[![データが横モードで保持されます。](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/)後に呼び出される`OnStart`です。 アクティビティを前の中に、バンドルに保存されているいずれかの状態を復元する機会を提供`OnSaveInstanceState`です。 これは、同じバンドルに提供される`OnCreate`、ただしです。

次のコードでの状態の復元方法を示しています`OnRestoreInstanceState`:

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

このメソッドは、柔軟性を提供するいくつかの周囲の状態を復元するときに存在します。 インスタンスの状態を復元する前にすべての初期化が完了するまで待機するより適切な場合があります。 さらに、既存の活動のサブクラス可能性がありますのみから復元する特定の値、インスタンスの状態。 多くの場合、その必要はありませんをオーバーライドする`OnRestoreInstanceState`ほとんどの活動に指定されたバンドルを使用して状態を復元できますので、`OnCreate`です。

使用して状態の保存の例については、`Bundle`を参照してください、[チュートリアル - アクティビティの保存状態](saving-state.md)です。


#### <a name="bundle-limitations"></a>バンドルの制限事項

`OnSaveInstanceState`は、これがフォルダーを簡単に一時的なデータを保存する、いくつかの制限。

-   すべての場合は呼び出されません。 たとえば、キーを押して**ホーム**または**戻る**アクティビティを終了するのには発生しません`OnSaveInstanceState`呼び出されています。

-   渡されたバンドル`OnSaveInstanceState`画像などのラージ オブジェクトのものではありません。 オブジェクトの保存、ラージ オブジェクトの場合[OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/)をお勧め後で説明します。

-   遅延が生じる可能性がバンドルを使用して、保存されたデータはシリアル化します。

バンドルの状態は適しています多くのメモリを使用しない単純なデータが*構成以外のインスタンス データ*より複雑なデータ、またはデータを取得するとコストが便利で、web サービス呼び出しや複雑な 2016/2/14 などには。データベースのクエリ。 非構成インスタンス データは、必要に応じて、オブジェクトに保存を取得します。 次のセクションで紹介`OnRetainNonConfigurationInstance`構成の変更によってより複雑なデータ型を維持するための手段として。


### <a name="persisting-complex-data"></a>複雑なデータの永続化

に加えて、バンドル内のデータを保持する、Android がサポートされますデータの保存をオーバーライドして[OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/)のインスタンスを返すと、`Java.Lang.Object`を保持するデータを格納します。 使用する 2 つの主な利点がある`OnRetainNonConfigurationInstance`状態を保存します。

-   返されたオブジェクト`OnRetainNonConfigurationInstance`メモリは、このオブジェクトを保持しているのでより大規模で複雑なデータ型でもを実行します。

-   `OnRetainNonConfigurationInstance`メソッドは、要求時に呼び出されたために必要な場合にのみ、します。 これは、手動のキャッシュを使用するよりも経済的です。

使用して`OnRetainNonConfigurationInstance`が複数回データを取得する高価なシナリオに適したなどが web サービス呼び出しのようにします。 たとえば、Twitter を検索する次のコードがあるとします。

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);
    SearchTwitter ("xamarin");
  }

  public void SearchTwitter (string text)
  {
    string searchUrl = String.Format("http://search.twitter.com/search.json?" + "q={0}&rpp=10&include_entities=false&" + "result_type=mixed", text);

    var httpReq = (HttpWebRequest)HttpWebRequest.Create (new Uri (searchUrl));
    httpReq.BeginGetResponse (new AsyncCallback (ResponseCallback), httpReq);
  }

  void ResponseCallback (IAsyncResult ar)
  {
    var httpReq = (HttpWebRequest)ar.AsyncState;

    using (var httpRes = (HttpWebResponse)httpReq.EndGetResponse (ar)) {
      ParseResults (httpRes);
    }
  }

  void ParseResults (HttpWebResponse httpRes)
  {
    var s = httpRes.GetResponseStream ();
    var j = (JsonObject)JsonObject.Load (s);

    var results = (from result in (JsonArray)j ["results"] let jResult = result as JsonObject select jResult ["text"].ToString ()).ToArray ();

    RunOnUiThread (() => {
      PopulateTweetList (results);
    });
  }

  void PopulateTweetList (string[] results)
  {
    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
  }
}
```

このコードは、JSON として書式設定、web から結果を取得、解釈し、次のスクリーン ショットに示すように、ボックスの一覧で結果を示します。

[![画面に表示される結果](images/06-sml.png)](images/06.png#lightbox)

構成の変更 - たとえば、デバイスを回すと発生のコードは、プロセスを繰り返します。 使用できるを最初に取得した結果を再利用し、不要な冗長なネットワークの呼び出しは起こりません、`OnRetainNonconfigurationInstance`を次に示すように、結果を保存します。

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  TweetListWrapper _savedInstance;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    var tweetsWrapper = LastNonConfigurationInstance as TweetListWrapper;

    if (tweetsWrapper != null) {
      PopulateTweetList (tweetsWrapper.Tweets);
    } else {
      SearchTwitter ("xamarin");
    }

    public override Java.Lang.Object OnRetainNonConfigurationInstance ()
    {
      base.OnRetainNonConfigurationInstance ();
      return _savedInstance;
    }

    ...

    void PopulateTweetList (string[] results)
    {
      ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
      _savedInstance = new TweetListWrapper{Tweets=results};
    }
}
```

デバイスを回転したときに、元の結果がから取得されますので、`LastNonConfiguartionInstance`プロパティです。 この例では、結果で構成されているの`string[]`ツイートを含むです。 `OnRetainNonConfigurationInstance`いる必要があります、`Java.Lang.Object`返される、`string[]`をサブクラスとして持つクラスでラップされた`Java.Lang.Object`、次のように。

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

たとえば、使用すると、`TextView`から返されるオブジェクトとして`OnRetainNonConfigurationInstance`で次のコードに示すように、アクティビティがリークします。

```csharp
TextView _textView;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  var tv = LastNonConfigurationInstance as TextViewWrapper;

  if(tv != null) {
    _textView = tv;
    var parent = _textView.Parent as FrameLayout;
    parent.RemoveView(_textView);
  } else {
    _textView = new TextView (this);
    _textView.Text = "This will leak.";
  }

  SetContentView (_textView);
}

public override Java.Lang.Object OnRetainNonConfigurationInstance ()
{
  base.OnRetainNonConfigurationInstance ();
  return _textView;
}
```

簡単な状態データを保持する方法がわかったため、このセクションで、`Bundle`とで複雑なデータ型が保持`OnRetainNonConfigurationInstance`です。

## <a name="summary"></a>まとめ

Android のアクティビティのライフ サイクルの強力なフレームワーク アプリケーション内の活動の状態管理を提供が、これはの理解し、実装を複雑になることができます。 この章では、これらの状態に関連付けられているライフ サイクル メソッドと同様に、有効期間中にアクティビティを通過がさまざまな状態が導入されました。 次に、これらの各メソッドで実行する必要がありますロジックの種類に関するガイダンスが指定されました。


## <a name="related-links"></a>関連リンク

- [回転の処理](~/android/app-fundamentals/handling-rotation.md)
- [Android のアクティビティ](https://developer.xamarin.com/api/type/Android.App.Activity/)
