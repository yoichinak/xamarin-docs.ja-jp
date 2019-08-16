---
title: アクティビティのライフサイクル
description: アクティビティは Android アプリケーションの基本的な構成要素であり、さまざまな状態に存在できます。 アクティビティのライフサイクルは、インスタンス化で始まり、破棄で終了し、間に多くの状態が含まれます。 アクティビティが状態を変更すると、適切なライフサイクルイベントメソッドが呼び出され、発生した状態変更のアクティビティが通知され、その変更に合わせてコードを実行できるようになります。 この記事では、アクティビティのライフサイクルについて説明し、これらの各状態の変化におけるアクティビティが、適切に動作し、信頼性の高いアプリケーションの一部となる責任について説明します。
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: 91e49c387818ca4d7472325efa665a5c2bfd9e64
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522047"
---
# <a name="activity-lifecycle"></a>アクティビティのライフサイクル

_アクティビティは Android アプリケーションの基本的な構成要素であり、さまざまな状態に存在できます。アクティビティのライフサイクルは、インスタンス化で始まり、破棄で終了し、間に多くの状態が含まれます。アクティビティが状態を変更すると、適切なライフサイクルイベントメソッドが呼び出され、発生した状態変更のアクティビティが通知され、その変更に合わせてコードを実行できるようになります。この記事では、アクティビティのライフサイクルについて説明し、これらの各状態の変化におけるアクティビティが、適切に動作し、信頼性の高いアプリケーションの一部となる責任について説明します。_

## <a name="activity-lifecycle-overview"></a>アクティビティのライフサイクルの概要

アクティビティは、Android に固有の通常とは異なるプログラミング概念です。 従来のアプリケーション開発では、通常、静的な main メソッドがあります。これは、アプリケーションを起動するために実行されます。 ただし、Android では、何も異なります。Android アプリケーションは、アプリケーション内の任意の登録済みアクティビティを使用して起動できます。 実際には、ほとんどのアプリケーションは、アプリケーションのエントリポイントとして指定された特定のアクティビティのみを持ちます。 ただし、アプリケーションがクラッシュした場合、または OS によって終了された場合、OS は、最後に開いたアクティビティまたは前のアクティビティスタック内の他の任意の場所でアプリケーションの再起動を試みることができます。
また、OS は、アクティブでないときにアクティビティを一時停止し、メモリが不足している場合はそれらを再利用することができます。 アクティビティが再開された場合にアプリケーションが状態を正常に復元できるようにするには、慎重に考慮する必要があります。これは特に、アクティビティが前のアクティビティのデータに依存している場合に発生します。

アクティビティのライフサイクルは、アクティビティのライフサイクル全体にわたって OS が呼び出すメソッドのコレクションとして実装されます。 これらのメソッドを使用すると、開発者は、アプリケーションの状態とリソース管理の要件を満たすために必要な機能を実装できます。

アプリケーション開発者は、各アクティビティの要件を分析して、アクティビティライフサイクルによって公開されるメソッドを実装する必要があることを判断することが非常に重要です。 この操作を行わないと、アプリケーションの不安定性、クラッシュ、リソースの膨張、および場合によっては、基になる OS の不安定性が発生する可能性があります。

この章では、アクティビティのライフサイクルについて詳しく説明します。

- アクティビティの状態
- ライフサイクル メソッド
- アプリケーションの状態を保持する


このセクションには、アクティビティのライフサイクル中に状態を効率的に保存する方法についての実用的な例を提供する[チュートリアル](~/android/app-fundamentals/activity-lifecycle/saving-state.md)も含まれています。 この章を終了すると、アクティビティのライフサイクルと、Android アプリケーションでそれをサポートする方法を理解できるようになります。

## <a name="activity-lifecycle"></a>アクティビティのライフサイクル

Android アクティビティのライフサイクルは、開発者にリソース管理フレームワークを提供するアクティビティクラス内で公開されるメソッドのコレクションで構成されています。 このフレームワークにより、開発者は、アプリケーション内の各アクティビティの固有の状態管理要件を満たし、リソース管理を適切に処理することができます。

### <a name="activity-states"></a>アクティビティの状態

Android OS は、その状態に基づいてアクティビティを判別します。 これにより、Android では使用されなくなったアクティビティを識別できるため、OS はメモリとリソースを再利用できます。 次の図は、アクティビティが有効期間中に実行できる状態を示しています。

[![アクティビティの状態の図](images/image1-sml.png)](images/image1.png#lightbox)

これらの状態は、次の4つのメイングループに分けることができます。

1. *アクティブまたは実行中*&ndash;アクティビティは、アクティビティスタックの最上位とも呼ばれる、フォアグラウンドに存在する場合、アクティブまたは実行中と見なされます。 これは、Android で最も優先度の高いアクティビティと見なされます。そのため、アクティビティがデバイスで使用可能なメモリよりも多くのメモリを使用しようとした場合 (UI が応答しなくなる可能性があるため) など、極端な状況では OS によってのみ強制終了されます。

1. *一時停止*&ndash;デバイスがスリープ状態のとき、またはアクティビティが依然として表示されていますが、新しい、または透過的なアクティビティによって部分的に非表示になっている場合、アクティビティは一時停止していると見なされます。 一時停止されたアクティビティはまだ生きています。つまり、すべての状態とメンバーの情報を保持し、ウィンドウマネージャーにアタッチされたままになります。 これは、Android の優先順位が2番目のアクティビティであると見なされます。そのため、このアクティビティを終了すると、アクティブまたは実行中のアクティビティを安定して応答性を維持するために必要なリソース要件を満たす場合にのみ、OS によって強制終了されます。

1. *停止/Backgrounded*&ndash;別のアクティビティによって完全に隠されているアクティビティは、停止またはバックグラウンドであると見なされます。
    停止したアクティビティは、可能な限り、状態とメンバーの情報を保持しようとしますが、停止されたアクティビティは3つの状態の最も低い優先度と見なされます。そのため、OS は、この状態のアクティビティを最初に終了してリソースを満たすようにします。優先順位の高いアクティビティの要件。

1. *再起動*&ndash; Android によってメモリから削除されるまで、ライフサイクル内で一時停止から停止されるまでのアクティビティが発生する可能性があります。 ユーザーがアクティビティに戻ると、再起動する必要があります。これは、以前に保存された状態に復元され、ユーザーに表示されます。


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>構成の変更に応じたアクティビティの再作成

さらに複雑になるように、Android では、構成の変更と呼ばれるミックスにもう1つのレンチがスローされます。 構成の変更は、アクティビティの構成が変更されたときに発生する迅速なアクティビティ破棄/再作成サイクルです。たとえば、デバイスが[ローテーション](~/android/app-fundamentals/handling-rotation.md)されるとき (およびアクティビティを横向きまたは縦モードで再構築する必要がある場合など)、キーボードが表示され (それ自体のサイズを変更するための機会がアクティビティに表示されます)、またはデバイスが他のデバイスとドックに配置されている場合。

構成の変更によっても、アクティビティの停止と再起動の間に発生するのと同じアクティビティ状態の変更が発生します。 ただし、アプリケーションの応答性を保証し、構成の変更中にアプリケーションを正常に実行できるようにするには、アプリケーションができるだけ迅速に処理されることが重要です。 このため、Android には、構成の変更中に状態を維持するために使用できる特定の API が用意されています。
これについては、後の「[ライフサイクルを通して状態を管理](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle)する」で説明します。

### <a name="activity-lifecycle-methods"></a>アクティビティのライフサイクルメソッド

Android SDK と、拡張機能により、Xamarin Android フレームワークは、アプリケーション内のアクティビティの状態を管理するための強力なモデルを提供します。 アクティビティの状態が変化すると、アクティビティには、そのアクティビティの特定のメソッドを呼び出す OS によって通知されます。 次の図は、アクティビティのライフサイクルに関連するこれらのメソッドを示しています。

[![アクティビティのライフサイクルフローチャート](images/image2-sml.png)](images/image2.png#lightbox)

開発者は、アクティビティ内でこれらのメソッドをオーバーライドすることによって、状態の変更を処理できます。 ただし、すべてのライフサイクルメソッドが UI スレッドで呼び出され、現在のアクティビティの非表示、新しいアクティビティの表示など、UI の次の作業を実行できないことに注意することが重要です。そのため、これらのメソッドのコードは、アプリケーションのパフォーマンスを十分に高めるために、可能な限り簡潔にする必要があります。 実行時間の長いタスクは、バックグラウンドスレッドで実行する必要があります。

これらのライフサイクルの各方法とその用途について説明します。

#### <a name="oncreate"></a>OnCreate

[OnCreate](xref:Android.App.Activity.OnCreate*)は、アクティビティの作成時に最初に呼び出されるメソッドです。
`OnCreate`は常にオーバーライドされ、次のようなアクティビティによって必要になる可能性があるスタートアップの初期化を実行します。

- ビューの作成
- 初期化 (変数を)
- バインド (静的データをリストに)

`OnCreate`[バンドル](xref:Android.OS.Bundle)パラメーターを受け取ります。バンドルが null でない場合、アクティビティ間で状態情報およびオブジェクトを格納して渡すためのディクショナリです。これは、アクティビティが再起動され、前のインスタンスから状態を復元する必要があることを示します。 次のコードは、バンドルから値を取得する方法を示しています。

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

完了`OnCreate`すると、Android はを`OnStart`呼び出します。

#### <a name="onstart"></a>OnStart

[OnStart](xref:Android.App.Activity.OnStart)は、の完了後`OnCreate` 、常にシステムによって呼び出されます。 アクティビティ内のビューの現在の値を更新するなど、アクティビティが表示される直前に特定のタスクを実行する必要がある場合、アクティビティはこのメソッドをオーバーライドすることがあります。 Android は、 `OnResume`このメソッドの直後にを呼び出します。

#### <a name="onresume"></a>OnResume

アクティビティがユーザーとの対話を開始する準備が整うと、システムは[Onresume](xref:Android.App.Activity.OnResume)を呼び出します。
アクティビティは、次のようなタスクを実行するために、このメソッドをオーバーライドする必要があります。

- フレームレートを上げる (ゲーム開発の一般的なタスク)
- アニメーションの開始
- GPS の更新をリッスンしています
- 関連するアラートまたはダイアログを表示する
- 外部イベントハンドラーの接続


例として、次のコードスニペットはカメラを初期化する方法を示しています。

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

`OnResume`で実行される`OnPause`すべての操作は、で`OnResume`実行される必要があるため重要です。これは、アクティビティを有効な状態に`OnPause`戻すときに実行が保証される唯一のライフサイクルメソッドであるためです。

#### <a name="onpause"></a>OnPause

[Onpause](xref:Android.App.Activity.OnPause)は、システムがアクティビティをバックグラウンドに配置しようとしているとき、またはアクティビティが部分的に隠されたときに呼び出されます。 アクティビティは次の操作を行う必要がある場合に、このメソッドをオーバーライドする必要があります。

- 保存されていない変更を永続的なデータにコミットする

- リソースを消費している他のオブジェクトを破棄またはクリーンアップする

- フレームレートの縮小とアニメーションの一時停止

- 外部イベントハンドラーまたは通知ハンドラー (サービスに関連付けられているもの) の登録を解除します。 これは、アクティビティのメモリリークを防ぐために行う必要があります。

- 同様に、アクティビティにダイアログや警告が表示されている場合は、 `.Dismiss()`メソッドを使用してクリーンアップする必要があります。

例として、次のコードスニペットでは、一時停止中にアクティビティが使用できないため、カメラが解放されます。

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

の後`OnPause`に呼び出されるライフサイクルメソッドには、次の2つがあります。

1. `OnResume`アクティビティがフォアグラウンドに返される場合は、が呼び出されます。
1. `OnStop`アクティビティがバックグラウンドで配置されている場合は、が呼び出されます。


#### <a name="onstop"></a>OnStop

[OnStop](xref:Android.App.Activity.OnStop)は、アクティビティがユーザーに表示されなくなったときに呼び出されます。 これは、次のいずれかが発生した場合に発生します。

- 新しいアクティビティが開始され、このアクティビティがカバーされます。
- 既存のアクティビティがフォアグラウンドに取り込まれます。
- アクティビティが破棄されています。


`OnStop`は、Android がリソースを使用していない場合や、アクティビティを適切にバックグラウンドで処理できない場合など、メモリ不足の状況では常に呼び出されるとは限りません。 このため、破棄のためにアクティビティを準備する`OnStop`ときに、の呼び出しに依存しないことをお勧めします。 この`OnDestroy`後に呼び出される可能性がある次のライフサイクルメソッドは、アクティビティが終了した場合`OnRestart` 、またはアクティビティがユーザーとの対話に戻る場合に発生します。

#### <a name="ondestroy"></a>OnDestroy

[OnDestroy](xref:Android.App.Activity.OnDestroy)は、破棄されてメモリから完全に削除される前に、アクティビティインスタンスで呼び出される最後のメソッドです。 極端な状況では、Android はアクティビティをホストしているアプリケーションプロセスを強制終了`OnDestroy`するため、呼び出されません。 ほとんどの`OnPause`クリーンアップおよびシャットダウンは、メソッドと`OnStop`メソッドで実行されているため、ほとんどのアクティビティはこのメソッドを実装しません。 通常`OnDestroy` 、メソッドは、リソースをリークする可能性のある実行時間の長いリソースをクリーンアップするためにオーバーライドされます。 この例としては、で`OnCreate`開始されたバックグラウンドスレッドがあります。

アクティビティが破棄された後、ライフサイクルメソッドは呼び出されません。

#### <a name="onrestart"></a>OnRestart

[Onrestart](xref:Android.App.Activity.OnRestart)は、アクティビティが停止した後、再度開始される前に呼び出されます。 この例は、ユーザーがアプリケーションのアクティビティで [ホーム] ボタンをクリックしたときに発生します。 このエラーが`OnPause`発生し`OnStop`た後、メソッドが呼び出され、アクティビティはバックグラウンドに移動されますが、破棄されることはありません。 その後、ユーザーがタスクマネージャーまたは同様のアプリケーションを使用してアプリケーションを復元した場合、 `OnRestart` Android はアクティビティのメソッドを呼び出します。

で`OnRestart`実装する必要があるロジックの種類に関する一般的なガイドラインはありません。 これは、 `OnStart`アクティビティが作成されるか再起動されるかにかかわらず、常にが呼び出されるためです`OnRestart`。そのため、 `OnStart`アクティビティに必要なすべてのリソースをではなく、で初期化する必要があります。

の後`OnRestart`に呼び出される次のライフ`OnStart`サイクルメソッドは、になります。

### <a name="back-vs-home"></a>バックとホーム (Home)

多くの Android デバイスには、"戻る" ボタンと "ホーム" ボタンという2つのボタンがあります。 この例については、Android 4.0.3 の次のスクリーンショットをご覧ください。

[![戻るボタンとホームボタン](images/image4-sml.png)](images/image4.png#lightbox)

アプリケーションを背景に配置した場合と同じ効果があるように見えても、2つのボタンの間には微妙な違いがあります。 ユーザーは、[戻る] ボタンをクリックすると、アクティビティを使用して実行されたことを Android に指示します。 Android によってアクティビティが破棄されます。 これに対して、ユーザーが [ホーム] ボタンをクリックしても、アクティビティ&ndash;がバックグラウンドに配置されるだけで、アクティビティは終了しません。

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>ライフサイクル全体の状態の管理

アクティビティが停止または破棄されると、システムは、後で復元できるようにアクティビティの状態を保存する機会を提供します。
この保存された状態は、"インスタンスの状態" と呼ばれます。 Android には、アクティビティのライフサイクル中にインスタンスの状態を格納するための3つのオプションがあります。

1. Android が状態を保存`Dictionary`するために使用するバンドルとして、プリミティブ値を[バンドル](xref:Android.OS.Bundle)として格納します。

1. ビットマップなどの複雑な値を保持するカスタムクラスを作成する。 Android では、このカスタムクラスを使用して状態を保存します。

1. 構成の変更のライフサイクルを回避し、アクティビティの状態を維持するための完全な責任を想定します。

このガイドでは、最初の2つのオプションについて説明します。

### <a name="bundle-state"></a>バンドルの状態

インスタンスの状態を保存するための主なオプションは、[バンドル](xref:Android.OS.Bundle)と呼ばれるキー/値ディクショナリオブジェクトを使用することです。
メソッドに`OnCreate`パラメーターとしてバンドルが渡されるアクティビティが作成されると、このバンドルを使用してインスタンスの状態を復元できることを思い出してください。 キー/値のペア (ビットマップなど) にすばやくまたは簡単にシリアル化できない複雑なデータには、バンドルを使用しないことをお勧めします。代わりに、文字列などの単純な値に使用する必要があります。

アクティビティには、バンドル内のインスタンスの状態を保存および取得するためのメソッドが用意されています。

- [OnSaveInstanceState](xref:Android.App.Activity.OnSaveInstanceState*)&ndash;これは、アクティビティが破棄されるときに Android によって呼び出されます。 キー/値の状態項目を永続化する必要がある場合、アクティビティはこのメソッドを実装できます。

- [OnRestoreInstanceState](xref:Android.App.Activity.OnRestoreInstanceState*)これは、メソッドの`OnCreate`終了後に呼び出され、初期化が完了した後にアクティビティが状態を復元する機会を提供します。 &ndash;

次の図は、これらのメソッドの使用方法を示しています。

[![バンドルの状態のフローチャート](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

アクティビティが停止されると、 [OnSaveInstanceState](xref:Android.App.Activity.OnSaveInstanceState*)が呼び出されます。 アクティビティが状態を格納できるバンドルパラメーターを受け取ります。 デバイスで構成の変更が発生した場合、アクティビティは`Bundle` 、をオーバーライド`OnSaveInstanceState`することによって、アクティビティの状態を保持するために渡されたオブジェクトを使用できます。 次に例を示します。

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

`c`上記のコードで`incrementCounter`は、という名前のボタンをクリックしたときにという名前`output`の整数をインクリメントし、 `TextView`という名前ので結果を表示しています。 構成の変更が発生した場合 (たとえば、デバイスがローテーションされた場合)、上の図`c`に示す`bundle`ように`null`、上のコードはの値を失います。

[![表示に以前の値が表示されない](images/07-sml.png)](images/07.png#lightbox)

この例のの`c`値を保持するために、アクティビティは次に示すように、バンドルに値を保存してオーバーライド`OnSaveInstanceState`できます。

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

デバイスを新しい方向に回転させると、整数がバンドルに保存され、次の行を使用して取得されます。

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> ビュー階層の状態も保存できるように、 `OnSaveInstanceState`常にの基本実装を呼び出すことが重要です。

##### <a name="view-state"></a>状態の表示

オーバーライド`OnSaveInstanceState`は、前の例のカウンターなど、方向の変化に応じてアクティビティ内の一時的なデータを保存するための適切なメカニズムです。 ただし、各ビューに ID `OnSaveInstanceState`が割り当てられている限り、の既定の実装では、すべてのビューの UI に一時的なデータが保存されます。 たとえば、次のように、アプリケーション`EditText`に XML で定義された要素があるとします。

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

コントロールには`id`が割り当てられているため、ユーザーがデータを入力してデバイスを回転すると、次のようにデータが表示されます。 `EditText`

[![データは横モードで保持されます](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](xref:Android.App.Activity.OnRestoreInstanceState*)は、の後`OnStart`に呼び出されます。 これにより、以前に`OnSaveInstanceState`バンドルに保存されていたすべての状態を復元する機会がアクティビティに提供されます。 ただし、これはに`OnCreate`用意されているバンドルと同じです。

次のコードは、で`OnRestoreInstanceState`状態を復元する方法を示しています。

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

このメソッドは、状態を復元するときの柔軟性を提供するために存在します。 場合によっては、インスタンスの状態を復元する前にすべての初期化が完了するまで待機する方が適切です。 また、既存のアクティビティのサブクラスでは、インスタンス状態から特定の値のみを復元する必要がある場合があります。 多くの場合、をオーバーライド`OnRestoreInstanceState`する必要はありません。ほとんどのアクティビティは、に`OnCreate`用意されているバンドルを使用して状態を復元できるためです。

を使用して`Bundle`状態を保存する例については、「[チュートリアル-アクティビティの状態を保存](saving-state.md)する」を参照してください。


#### <a name="bundle-limitations"></a>バンドルの制限事項

`OnSaveInstanceState`では一時的なデータを簡単に保存できますが、いくつかの制限があります。

- 常に呼び出されるわけではありません。 たとえば、[ `OnSaveInstanceState` **ホーム**] または **[戻る]** を押してアクティビティを終了しても、は呼び出されません。

- に`OnSaveInstanceState`渡されるバンドルは、イメージなどのラージオブジェクト用には設計されていません。 大きなオブジェクトの場合は、以下で説明するように、 [OnRetainNonConfigurationInstance](xref:Android.App.Activity.OnRetainNonConfigurationInstance)からオブジェクトを保存することをお勧めします。

- バンドルを使用して保存されたデータはシリアル化されるため、遅延が発生する可能性があります。

バンドル状態は、大量のメモリを使用しない単純なデータに役立ちます。一方、*非構成インスタンスデータ*は、より複雑なデータや、web サービス呼び出しや複雑なデータベースクエリなどのデータの取得に負荷がかかるデータには役立ちます。 構成以外のインスタンスデータは、必要に応じてオブジェクトに保存されます。 次のセクションで`OnRetainNonConfigurationInstance`は、構成の変更によってより複雑なデータ型を保持する方法を紹介します。


### <a name="persisting-complex-data"></a>複合データの保持

Android では、バンドル内のデータの永続化に加えて、 [OnRetainNonConfigurationInstance](xref:Android.App.Activity.OnRetainNonConfigurationInstance)をオーバーライドし、永続化する`Java.Lang.Object`データを含むのインスタンスを返すことによって、データの保存もサポートしています。 を使用して`OnRetainNonConfigurationInstance`状態を保存すると、主に次の2つの利点があります。

- によって返さ`OnRetainNonConfigurationInstance`れるオブジェクトは、メモリがこのオブジェクトを保持するため、より大規模で複雑なデータ型でも正常に動作します。

- `OnRetainNonConfigurationInstance`メソッドは、必要な場合にのみ、要求時に呼び出されます。 これは、手動キャッシュを使用するよりも経済的です。

を`OnRetainNonConfigurationInstance`使用すると、web サービスの呼び出しなど、データを複数回取得するのが高価な場合に適しています。 たとえば、Twitter を検索する次のコードについて考えてみます。

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

このコードは、次のスクリーンショットに示すように、JSON 形式の web から結果を取得し、解析して、結果をリストに表示します。

[![画面に表示される結果](images/06-sml.png)](images/06.png#lightbox)

構成の変更が発生した場合 (たとえば、デバイスがローテーションされている場合)、コードはプロセスを繰り返します。 最初に取得した結果を再利用し、余分な不要なネットワーク呼び出しを`OnRetainNonconfigurationInstance`発生させないようにするには、次に示すように、を使用して結果を保存します。

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

これでデバイスがローテーションされると、元の結果が`LastNonConfiguartionInstance`プロパティから取得されます。 この例では、結果はツイートを`string[]`含むで構成されます。 で`OnRetainNonConfigurationInstance`はが返さ`Java.Lang.Object` れる必要`Java.Lang.Object`があるため、は次のようにサブクラスを持つクラスでラップされます。`string[]`

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

たとえば、次のコードに示す`TextView`ように、をから`OnRetainNonConfigurationInstance`返されたオブジェクトとして使用しようとすると、アクティビティがリークします。

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

このセクションでは`Bundle`、を使用して単純な状態データを保持し、より複雑なデータ`OnRetainNonConfigurationInstance`型を永続化する方法を学習しました。

## <a name="summary"></a>Summary

Android アクティビティのライフサイクルは、アプリケーション内のアクティビティの状態管理のための強力なフレームワークを提供しますが、理解して実装するのは厄介です。 この章では、アクティビティの有効期間中に発生する可能性のあるさまざまな状態と、それらの状態に関連付けられているライフサイクルメソッドについて説明しました。 次に、これらの各メソッドで実行する必要があるロジックの種類に関するガイダンスが提供されました。


## <a name="related-links"></a>関連リンク

- [回転の処理](~/android/app-fundamentals/handling-rotation.md)
- [Android アクティビティ](xref:Android.App.Activity)
