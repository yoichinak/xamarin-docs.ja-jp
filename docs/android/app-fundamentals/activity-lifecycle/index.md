---
title: アクティビティのライフサイクル
description: アクティビティは、Android アプリケーションの基本的なビルディング ブロックと、さまざまな種類の状態で存在することができます。 アクティビティのライフ サイクルはインスタンス化で始まる破棄、終わるし、間に多くの状態が含まれています。 アクティビティの状態が変更されたとき適切なライフ サイクル イベントのメソッドは、アクティビティの兆候の状態の変更を通知して、その変更に合わせてコードを実行することができますに呼び出されます。 この記事では、責任について説明します調べアクティビティのライフ サイクル中に適切に動作、信頼性の高いアプリケーションの一部としてこれらの状態変更の各アクティビティがあります。
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: 3592a3027469cb9997d973db53d636ddea9e679d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61024308"
---
# <a name="activity-lifecycle"></a>アクティビティのライフサイクル

_アクティビティは、Android アプリケーションの基本的なビルディング ブロックと、さまざまな種類の状態で存在することができます。アクティビティのライフ サイクルはインスタンス化で始まる破棄、終わるし、間に多くの状態が含まれています。アクティビティの状態が変更されたとき適切なライフ サイクル イベントのメソッドは、アクティビティの兆候の状態の変更を通知して、その変更に合わせてコードを実行することができますに呼び出されます。この記事では、責任について説明します調べアクティビティのライフ サイクル中に適切に動作、信頼性の高いアプリケーションの一部としてこれらの状態変更の各アクティビティがあります。_

## <a name="activity-lifecycle-overview"></a>アクティビティ ライフ サイクルの概要

アクティビティは、特殊なプログラミング概念 Android に固有です。 従来のアプリケーション開発では、通常は静的 main メソッドを実行して、アプリケーションを起動します。 Android、ただし、事情が異なります。アプリケーション内で、登録済みのアクティビティを使用して、android アプリケーションを起動できます。 実際には、ほとんどのアプリケーションはアプリケーションのエントリ ポイントとして指定されている特定のアクティビティのみがあります。 ただし、アプリケーションがクラッシュした場合は、終了または、OS によって、OS はアプリケーションで開いて最後のアクティビティまたはアクティビティの前のスタック内でその他の場所を再起動する試すことができます。
さらに、OS は、アクティブにしていない場合は、アクティビティを一時停止し、メモリが不足である場合は、それらを解放する可能性があります。 アプリケーションは、アクティビティが再起動される場合に特にアクティビティが前のアクティビティからのデータに依存していること、その状態を正しく復元できるようにするのには、慎重に検討を行う必要があります。

アクティビティのライフ サイクルは、アクティビティのライフ サイクル全体で OS のメソッド呼び出しのコレクションとして実装されます。 これらのメソッドを使用すると、開発者が自分のアプリケーションの状態とリソース管理の要件を満たすために必要な機能を実装します。

アクティビティのライフ サイクルによって公開されるメソッドを実装する必要がありますを決定する各アクティビティの要件の分析のアプリケーション開発者にとって非常に重要です。 アプリケーションが不安定になる、クラッシュ、リソースが肥大化、および基になる可能性がありますも OS が不安定になるこれを行うにエラーが発生します。

この章はアクティビティのライフ サイクルの詳細を含みます。

-  アクティビティの状態
-  ライフサイクル メソッド
-  アプリケーションの状態の保持


このセクションにも含まれています、[チュートリアル](~/android/app-fundamentals/activity-lifecycle/saving-state.md)アクティビティのライフ サイクル中に状態を効率的に保存する方法の実際の例を提供します。 この章の末尾では、アクティビティのライフ サイクルと Android のアプリケーションでサポートする方法の理解が必要です。

## <a name="activity-lifecycle"></a>アクティビティのライフサイクル

Android アクティビティのライフ サイクルには、リソース管理フレームワークを開発者に提供するアクティビティ クラス内で公開されるメソッドのコレクションが構成されています。 このフレームワークにより、開発者は、アプリケーション内の各アクティビティの一意の状態管理の要件を満たし、リソース管理を適切に処理します。

### <a name="activity-states"></a>アクティビティの状態

Android OS を介し、状態に基づいてアクティビティ。 これにより、Android、OS メモリとリソースを解放することができます、使用されなくなったアクティビティを識別できます。 次の図は、その有効期間中にアクティビティを経由できる状態を示しています。

[![アクティビティ状態図](images/image1-sml.png)](images/image1.png#lightbox)

これらの状態は、とおり 4 つの主なグループに分けることができます。

1.  *アクティブまたは実行中*&ndash;活動がアクティブと見なされますまたはアクティビティ スタックの先頭とも呼ばれる場合は、フォア グラウンドで実行されています。 これは Android では、最高の優先度のアクティビティと見なされます、よう、極端な状況で、OS によってのみ終了などアクティビティよりも多くのメモリを使用しようとする場合とは、デバイスで使用できるように、UI が応答しなくなる可能性が。

1.  *一時停止している*&ndash;アクティビティと見なされます、デバイスが、スリープ状態になるか、アクティビティがまだ表示されているが、新規または透過的なフル サイズでは非アクティビティによって部分的に非表示には、一時停止します。 一時停止中のアクティビティはまだ動作しているすべての状態とメンバー情報の管理し、ウィンドウ マネージャーに接続したまま、つまり、します。 これは、2 つ目にすると見なされます Android では、同様に、最高の優先度のアクティビティはのみ強制終了されます、OS によってこのアクティビティを強制終了は安定性と応答性の高いアクティブ/実行中のアクティビティを保持するために必要なリソース要件を満たします。

1.  *停止/Backgrounded* &ndash;別のアクティビティによって完全に隠されている活動は停止しているか、バック グラウンドでと見なされます。
    アクティビティを停止している限り、可能であればが停止しているアクティビティは 3 つの状態の最も低い優先順位と見なされ、OS は、リソースを満たすために最初にこの状態のアクティビティを終了するよう、自分用の状態とメンバー情報を保持する試行します。高い優先度の活動の要件。

1.  *再起動*&ndash;がどこからであるアクティビティが Android によってメモリから削除するライフ サイクルの停止に一時停止します。 ユーザーが移動した場合、再起動する必要が、アクティビティは、以前に保存した状態に復元し、ユーザーに表示されます。


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>構成の変更に応答のアクティビティの再作成

複雑な問題をするためには、Android は、構成の変更と呼ばれるミックス内の 1 つ以上のレンチをスローします。 構成の変更はアクティビティの迅速な破棄/再 creation サイクル、デバイスの場合などのアクティビティの構成が変更されたときに発生する[回転](~/android/app-fundamentals/handling-rotation.md)(および横または縦向きに再ビルドする必要のあるアクティビティモード)、キーボードが表示されます (とそれ自体のサイズを変更する機会は、アクティビティが表示されます)、またはデバイスを他のユーザーの間でのドックに配置するとします。

構成の変更を停止して再起動活動の間に行われるのと同じアクティビティの状態の変更もあります。 ただし、アプリケーション応答性を感じていることを確認の構成の変更時に実行するには、するために可能な限り早く処理することが重要です。 このため、Android では、構成の変更時に状態を永続化に使用できる特定の API があります。
この後で取り上げます、 [the Lifecycle 全体で状態を管理する](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle)セクション。

### <a name="activity-lifecycle-methods"></a>アクティビティのライフ サイクル メソッド

Android SDK と、拡張機能によって Xamarin.Android フレームワークは、アプリケーション内のアクティビティの状態を管理する強力なモデルを提供します。 アクティビティの状態が変更されるときに、アクティビティは、そのアクティビティの特定のメソッドを呼び出すと、OS によって通知されます。 次の図は、アクティビティのライフ サイクルの関係におけるこれらのメソッドを示しています。

[![アクティビティのライフ サイクルのフローチャート](images/image2-sml.png)](images/image2.png#lightbox)

開発者は、アクティビティ内でこれらのメソッドをオーバーライドすることで状態の変更を処理できます。 ただし、すべてのライフ サイクル メソッドが UI スレッドでと呼ばれ、新しいアクティビティなどを表示する、次の項目の現在のアクティビティを非表示などの UI の作業を実行するから OS をブロックことに重要です。そのため、これらのメソッドのコードもを実行すると思われるアプリケーションをできるだけ短くしてください。 バック グラウンド スレッドで実行時間の長いタスクを実行する必要があります。

これらのライフ サイクル メソッドとその使用のそれぞれを調べてみましょう。

#### <a name="oncreate"></a>OnCreate

[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)はアクティビティが作成されたときに呼び出される最初の方法です。
`OnCreate` などのアクティビティによって必要とされるすべてのスタートアップ初期化を実行する常にオーバーライドされます。

-  ビューを作成します。
-  変数の初期化
-  リストへの静的なデータをバインディング


`OnCreate` [バンドル](https://developer.xamarin.com/api/type/Android.OS.Bundle/)アクティビティを再起動してからの状態を復元する必要がありますを示しますこのパラメーターを格納して、バンドルが null でない場合、アクティビティ間で状態情報とオブジェクトを渡すのためのディクショナリ、以前のインスタンス。 次のコードでは、バンドルから値を取得する方法を示します。

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

1 回`OnCreate`が Android の呼び出しが完了したら、`OnStart`します。

#### <a name="onstart"></a>OnStart

[OnStart](https://developer.xamarin.com/api/member/Android.App.Activity.OnStart/)後にシステムによって必ず呼び出される`OnCreate`が終了します。 アクティビティは、アクティビティ、アクティビティ内でビューの現在の値の更新などを表示するには、任意の特定のタスクの右側を実行する必要がある場合、このメソッドをオーバーライド可能性があります。 Android が呼び出す`OnResume`このメソッドの直後にします。

#### <a name="onresume"></a>OnResume

システム コール[OnResume](https://developer.xamarin.com/api/member/Android.App.Activity.OnResume/)アクティビティがユーザーとの対話を開始する準備です。
アクティビティなどのタスクを実行するには、このメソッドをオーバーライドする必要があります。

-  (ゲームの構築に一般的なタスク) のフレーム レートの急増
-  アニメーションの開始
-  GPS の更新プログラムのリッスン
-  関連するアラートや、ダイアログを表示します。
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

`OnResume` いずれかの操作であるためですが、重要で行う`OnPause`で ne されていないことがあります`OnResume`後に実行することが保証されるだけのライフ サイクル メソッドであるため、`OnPause`元に戻すにはアクティビティを取り込むときに。

#### <a name="onpause"></a>OnPause

[OnPause](https://developer.xamarin.com/api/member/Android.App.Activity.OnPause/)システムは、バック グラウンドに、または、アクティビティが覆い隠さ部分的に、アクティビティを配置ときに呼び出されます。 アクティビティは、する必要がある場合、このメソッドをオーバーライドする必要があります。

-   永続的なデータに保存されていない変更をコミットします。

-   破棄、またはリソースを消費して他のオブジェクトをクリーンアップします。

-   フレーム レートとアニメーションが一時停止をごとの傾斜増加

-   外部のイベント ハンドラーや通知のハンドラー (つまり、サービスに関連付けられているもの) の登録を解除します。 これは、アクティビティのメモリ リークを防ぐために実行する必要があります。

-   同様に、アクティビティは、ダイアログやアラートに表示されますが場合する必要があるクリーンアップと、`.Dismiss()`メソッド。

たとえば、次のコード スニペットはリリース、カメラ、アクティビティが行うことはできませんの一時停止中にそれを使用します。

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

1.  `OnResume` フォア グラウンドに返される場合は、アクティビティが呼び出されます。
1.  `OnStop` バック グラウンドでアクティビティを配置する場合に呼び出されます。


#### <a name="onstop"></a>OnStop

[OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/)アクティビティは、ユーザーに表示されるが不要になったときに呼び出されます。 これは、次のいずれかが発生したときに発生します。

-  新しい活動を開始していますが、このアクティビティを隠してします。
-  既存のアクティビティは、フォア グラウンドに表示になっています。
-  アクティビティを破棄しています。


`OnStop` 常に呼ぶことなど、Android がリソースが不足し、アクティビティがバック グラウンド正しくことはできません、メモリ不足の状況でします。 このためには依存しない`OnStop`破棄のアクティビティを準備するときに呼び出されます。 この 1 つ後に呼び出すことができ、[次へ] のライフ サイクル メソッド`OnDestroy`場合は、アクティビティが消滅または`OnRestart`ユーザーと対話するアクティビティが返される場合。

#### <a name="ondestroy"></a>onDestroy

[OnDestroy](https://developer.xamarin.com/api/member/Android.App.Activity.OnDestroy/)は破棄され、完全にメモリから削除されますが、前に、アクティビティ インスタンスで呼び出される最終的なメソッドです。 極端な状況で、Android のアプリケーション プロセスが発生すると、アクティビティをホストしている可能性があります強制終了`OnDestroy`中は呼び出されません。 ほとんどのアクティビティはほとんどをクリーンアップして、シャット ダウンがで実行されたため、このメソッド実装しません、`OnPause`と`OnStop`メソッド。 `OnDestroy`メソッドをオーバーライドは、通常は時間の長いをクリーンアップするリソースを実行している可能性がありますがリソースをリークします。 この例のバック グラウンド スレッドで開始された可能性があります`OnCreate`します。

ライフ サイクル メソッドのアクティビティが破棄された後に呼び出されますされません。

#### <a name="onrestart"></a>OnRestart

[OnRestart](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestart/)は後、アクティビティが停止したら、もう一度開始する前に呼び出されます。 このよい例は、ユーザーがアプリケーションのアクティビティ中に [ホーム] ボタンを押したときになります。 この場合`OnPause`し`OnStop`メソッドが呼び出されると、アクティビティがバック グラウンドに移動されますは破棄されません。 ユーザーが Android を呼び出し、タスク マネージャーまたは同様のアプリケーションを使用して、アプリケーションを復元する場合、`OnRestart`アクティビティのメソッド。

一般的なガイドラインでどのような種類のロジックを実装する必要がない`OnRestart`します。 これは、ため`OnStart`アクティビティが作成されているかどうかに関係なく常に呼び出されるまたは再起動されるためで、アクティビティに必要なすべてのリソースを初期化する必要があります`OnStart`、なく`OnRestart`します。

次のライフ サイクル メソッドが後に呼び出される`OnRestart`なります`OnStart`します。

### <a name="back-vs-home"></a>Vs をバックアップします。Home

多くの Android デバイスがある 2 つの異なるボタン: [戻る] ボタンと [ホーム] ボタン。 この例は、Android 4.0.3 の次のスクリーン ショットで確認できます。

[![戻るボタンとホーム ボタン](images/image4-sml.png)](images/image4.png#lightbox)

2 つのボタン間の微妙な違いがある場合でも、バック グラウンドでアプリケーションを配置することの効果は同じように見えます。 ユーザーは、[戻る] ボタンをクリックすると、これらは、指示 Android アクティビティにこれらが行われること。 Android アクティビティが破棄されます。 これに対し、ユーザーが [ホーム] ボタンをクリックすると、アクティビティは単に配置されます、バック グラウンド&ndash;Android は、アクティビティを終了していません。

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>ライフ サイクル全体の状態を管理します。

アクティビティが停止しているか破棄されるときに、システムは、以降のリハイド レートのアクティビティの状態を保存する機会を提供します。
この状態が保存されているが、インスタンスの状態と呼ばれます。 Android では、アクティビティのライフ サイクル中にインスタンスの状態を格納するための 3 つのオプションが用意されています。

1. プリミティブ型の値を格納する、`Dictionary`と呼ばれる、[バンドル](https://developer.xamarin.com/api/type/Android.OS.Bundle/)Android が状態の保存に使用することです。

1. カスタム クラスを作成すると、ビットマップなどの複合型の値が保持されます。 Android では、このカスタム クラスを状態の保存に使用します。

1. 構成変更のライフ サイクルを回避し、アクティビティの状態を維持するための完全な責任を負います。


このガイドでは、最初の 2 つのオプションについて説明します。



### <a name="bundle-state"></a>バンドルの状態

インスタンスの状態を保存するための主なオプションと呼ばれるキーと値のディクショナリ オブジェクトを使用する、[バンドル](https://developer.xamarin.com/api/type/Android.OS.Bundle/)します。
アクティビティが作成されたときにを思い出してください、`OnCreate`メソッドにはパラメーターとしてバンドルが渡されると、このバンドルは、インスタンスの状態を復元するために使用できます。 複雑なデータをすばやくはありません。 または、キー/値ペア (ビットマップ) などを簡単にシリアル化のバンドルを使用するには使用しないでください。代わりに、文字列などの単純な値を使用する必要があります。

アクティビティは、保存と、バンドル内のインスタンスの状態の取得を支援するメソッドを提供します。

-   [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) &ndash;アクティビティが破棄されるときに Android によって呼び出されるこのします。 アクティビティは、任意のキー/値の状態の項目を保持する必要がある場合、このメソッドを実装できます。

-   [OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) &ndash;これが後に呼び出されますが、`OnCreate`メソッドが完了したら、別の機会を提供初期化が完了したら、その状態を復元するアクティビティ。

次の図は、これらのメソッドを使用する方法を示しています。

[![バンドルの状態のフローチャート](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/)ように、アクティビティを停止していますが、呼び出されます。 アクティビティの状態を格納できるバンドル パラメーターが表示されます。 アクティビティを使用できるデバイスに、構成の変更が発生した場合、`Bundle`でオーバーライドすることによってアクティビティの状態を保持するために渡されるオブジェクト`OnSaveInstanceState`します。 次に例を示します。

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

上記のコードは、という名前の整数をインクリメント`c`という名前のボタンと`incrementCounter`をクリックするで結果を表示する、`TextView`という名前の`output`します。 構成の変更が発生したとき、たとえば、ときに、デバイスの回転 - 上記のコードの値を失う`c`ため、`bundle`なります`null`次の図に示すように。

[![前の値は表示されません。](images/07-sml.png)](images/07.png#lightbox)

値を保持するために`c`、この例では、アクティビティをオーバーライドできます`OnSaveInstanceState`、次に示すように、バンドルに値を保存しています。

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

ようになりました、デバイスが新しい方向に回転させると、整数は、バンドルには保存され、行を取得します。

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> 基本実装を常に重要な呼び出しは、その`OnSaveInstanceState`ビュー階層の状態を保存することもできるようにします。



##### <a name="view-state"></a>ビュー ステート

オーバーライドする`OnSaveInstanceState`は上記の例では、カウンターなどの向きの変更の間でアクティビティでの一時的なデータを保存するための適切なメカニズムです。 ただし、既定の実装の`OnSaveInstanceState`各ビューでは、ID が割り当てられている限り、すべてのビューの UI に一時的なデータの保存処理されます。 たとえば、アプリケーションが、 `EditText` XML で次のように定義された要素。

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

以降、`EditText`コントロールが、`id`割り当て、ユーザーがデータの一部を入力し、デバイスを回転するときに、データが引き続き表示されます、次に示すよう。

[![データが横モードで保持されます。](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/)後に呼び出される`OnStart`します。 活動中に、前のバンドルに保存されている任意の状態を復元する機会を提供`OnSaveInstanceState`します。 これは、同じバンドルに提供される`OnCreate`、ただしします。

次のコード例での状態の復元方法`OnRestoreInstanceState`:

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

状態を復元するときに、いくつかの柔軟性を提供するこのメソッドが存在します。 インスタンスの状態を復元する前にすべての初期化が完了するまで待機するより適切な場合があります。 さらに、既存の活動のサブクラスは、インスタンスの状態から特定の値を復元することがある場合のみ。 多くの場合、その必要はありませんをオーバーライドする`OnRestoreInstanceState`ほとんどのアクティビティに提供されるバンドルを使用して状態を復元できますので、`OnCreate`します。

使用して状態を保存する例については、`Bundle`を参照してください、[チュートリアル -、アクティビティの保存状態](saving-state.md)します。


#### <a name="bundle-limitations"></a>バンドルの制限事項

`OnSaveInstanceState`によりいくつかの制限がある一時的なデータを保存する簡単な。

-   すべてのケースでは呼び出されません。 たとえば、キーを押して**ホーム**または**戻る**アクティビティを終了するのには発生しません`OnSaveInstanceState`が呼び出されます。

-   渡されるバンドル`OnSaveInstanceState`画像などのラージ オブジェクトのものではありません。 オブジェクトの保存、ラージ オブジェクトの場合[OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/)は以下に説明したように、ことをお勧めします。

-   バンドルを使用して、保存されたデータはシリアル化する遅延が発生する可能性があります。

一方、バンドルの状態が単純なデータを多くのメモリを使用しない場合に便利ですが*構成以外のインスタンス データ*はより複雑なデータ、またはデータの取得にかかるコストが便利で、web サービスの呼び出しや、複雑なこのようなデータベースのクエリ。 非構成インスタンス データは、必要に応じて、オブジェクトに保存を取得します。 次のセクションでは紹介`OnRetainNonConfigurationInstance`構成変更で複雑なデータ型を保持する手段として。


### <a name="persisting-complex-data"></a>複雑なデータを永続化

バンドル内のデータの永続化、だけでなく Android もサポートしていますデータの保存をオーバーライドして[OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/)のインスタンスを返すと、`Java.Lang.Object`を保持するデータを格納しています。 使用する 2 つの主な利点がある`OnRetainNonConfigurationInstance`状態を保存します。

-   返されるオブジェクト`OnRetainNonConfigurationInstance`メモリは、このオブジェクトを保持しているので、大規模でより複雑なデータ型でも実行します。

-   `OnRetainNonConfigurationInstance`メソッドが必要な場合にのみ、オンデマンドで呼び出される。 これは、手動のキャッシュを使用するよりも経済的です。

使用して`OnRetainNonConfigurationInstance`は web サービス呼び出しなどでは、データを複数回取得する高価なシナリオに適しています。 たとえば、Twitter を検索する次のコードがあるとします。

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

このコードは、JSON として書式設定、web から結果を取得、解釈し、次のスクリーン ショットで示すように、リスト、結果を示します。

[![画面に表示される結果](images/06-sml.png)](images/06.png#lightbox)

構成を変更するときに発生します - たとえば、デバイスを回転すると、コードは、プロセスを繰り返します。 最初に取得した結果を再利用していない言うまでもなく、冗長ネットワーク呼び出しには、使用`OnRetainNonconfigurationInstance`を次に示すように、結果を保存します。

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

デバイスが回転したときに、元の結果がから取得されますので、`LastNonConfiguartionInstance`プロパティ。 結果から成る、この例では、`string[]`ツイートを格納しています。 `OnRetainNonConfigurationInstance`いる必要があります、`Java.Lang.Object`返される、`string[]`をサブクラスとして持つクラスでラップされた`Java.Lang.Object`以下に示すように。

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

たとえば、使用しよう、`TextView`から返されるオブジェクトとして`OnRetainNonConfigurationInstance`次のコードに示すように、アクティビティがリークします。

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

単純な状態データを保持する方法をについて説明しましたこのセクションで、`Bundle`より複雑なデータ型を保持し、 `OnRetainNonConfigurationInstance`。

## <a name="summary"></a>まとめ

Android アクティビティのライフ サイクルは、アプリケーション内のアクティビティの状態管理の強力なフレームワークを提供しますが、理解および実装に注意が必要であることができます。 この章では、これらの状態に関連付けられているライフ サイクル メソッドと同様に、有効期間中にアクティビティを通過可能性があるさまざまな状態が導入されました。 次に、これらの各メソッドにどのような種類のロジックを実行するかに関するガイダンスが指定されました。


## <a name="related-links"></a>関連リンク

- [回転の処理](~/android/app-fundamentals/handling-rotation.md)
- [Android アクティビティ](https://developer.xamarin.com/api/type/Android.App.Activity/)
