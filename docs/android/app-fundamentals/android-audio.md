---
title: Android Audio
description: "Android OS は、オーディオとビデオの両方を含むマルチ メディア、広範なサポートを提供します。 このガイドでは、Android でのオーディオを重視し、再生組み込みオーディオ プレーヤーとレコーダー クラスだけでなく、低レベルのオーディオ API を使用してオーディオを録音したりについて説明します。 開発者が適切に動作のアプリケーションを構築できるように、他のアプリケーションによってブロードキャストされたオーディオ イベントの処理も取り上げています。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: 91bd5ae83cd0d59872e11a6b1bdc7b84c751e64f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="android-audio"></a>Android Audio

_Android OS は、オーディオとビデオの両方を含むマルチ メディア、広範なサポートを提供します。このガイドでは、Android でのオーディオを重視し、再生組み込みオーディオ プレーヤーとレコーダー クラスだけでなく、低レベルのオーディオ API を使用してオーディオを録音したりについて説明します。開発者が適切に動作のアプリケーションを構築できるように、他のアプリケーションによってブロードキャストされたオーディオ イベントの処理も取り上げています。_


## <a name="overview"></a>概要

最新のモバイル デバイスは、以前の生成が必要な機器専用の機能を採用している&ndash;カメラ、ミュージック プレーヤー、ビデオ レコーダーです。 このため、モバイル Api のファーストクラスの機能をマルチ メディア フレームワークになっています。

Android では、マルチ メディアの広範なサポートを提供します。 この記事は、Android でのオーディオに関する作業を検査し、次のトピックについて説明

1.  **Media Player でオーディオの再生**&ndash;組み込みを使用して`MediaPlayer`ローカル オーディオ ファイルを含むオーディオ ファイルのストリーミングなど、オーディオを再生するクラス、`AudioTrack`クラスです。

2.  **オーディオの録音**&ndash;組み込みを使用して`MediaRecorder`オーディオを録音するクラス。

3.  **オーディオの通知の使用**&ndash;中断するか、それらのオーディオ出力を取り消すことによって適切に動作するアプリケーション (電話の着信呼び出し) などのイベントに応答を作成するオーディオの通知を使用します。

4.  **オーディオの低水準の操作**&ndash;オーディオを使用して再生、`AudioTrack`メモリ バッファーに直接書き込むことによってクラスです。 使用してオーディオを記録、`AudioRecord`メモリ バッファーから直接読み取ったりクラスです。


## <a name="requirements"></a>必要条件

このガイドには、Android 2.0 (API レベル 5) が必要がありますまたはそれ以降。 デバイスで Android でのオーディオのデバッグを行う必要がある注意してください。

要求する必要がある、`RECORD_AUDIO`でアクセス許可**AndroidManifest.XML**:

![Android マニフェストのレコードでのアクセス許可のセクションに必要な\_オーディオを有効になっています。](android-audio-images/image01.png)



## <a name="playing-audio-with-the-mediaplayer-class"></a>Media Player クラスを使用したオーディオの再生

Android でオーディオを再生する最も簡単な方法は組み込み[MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)クラスです。
`MediaPlayer` ファイルのパスを渡すことによって、ローカルまたはリモートのファイルを再生できます。 ただし、`MediaPlayer`非常に状態を区別し、正しくない状態で独自のメソッドのいずれかを呼び出してがスローされる例外が発生します。 対話することが重要`MediaPlayer`エラーを避けるために次に示す順序で。



### <a name="initializing-and-playing"></a>初期化し、再生します。

オーディオを再生`MediaPlayer`次のシーケンスが必要です。

1. 新しいインスタンスを作成[MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)オブジェクト。

1. 構成ファイルを使用して再生、 [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/)メソッドです。

1. 呼び出す、[準備](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/)プレーヤーを初期化します。

1. 呼び出す、[開始](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/)オーディオの再生を開始するメソッド。


次のコード例は、この使用法を示しています。

```csharp
protected MediaPlayer player;
public void StartPlayer(String  filePath)
{
  if (player == null) {
    player = new MediaPlayer();
  } else {
    player.Reset();
    player.SetDataSource(filePath);
    player.Prepare();
    player.Start();
  }
}
```


### <a name="suspending-and-resuming-playback"></a>保留および再生を再開します。

再生を呼び出すことによって中断されることができます、 [Pause](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/)メソッド。

```csharp
player.Pause();
```

一時停止中の再生を再開するには、呼び出し、[開始](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/)メソッドです。
これは、再生時に一時停止した場所から再開されます。

```csharp
player.Start();
```

呼び出す、[停止](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/)プレーヤーでメソッドが継続的な再生を終了します。

```csharp
player.Stop();
```

呼び出して、リソースを解放する必要があります、player を不要になったとき、[リリース](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/)メソッド。

```csharp
player.Release();
```



## <a name="using-the-mediarecorder-class-to-record-audio"></a>オーディオ録音する MediaRecorder クラスを使用します。

結果`MediaPlayer`Android でのオーディオの記録は、 [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)クラスです。 同様に、 `MediaPlayer`、その状態に依存しへの記録を開始するポイントを取得するいくつかの状態を遷移します。 オーディオを記録するために、`RECORD_AUDIO`アクセス許可を設定する必要があります。 アプリケーションを設定する方法についてのアクセス許可を参照してください[AndroidManifest.xml 扱う](~/android/platform/android-manifest.md)です。


### <a name="initializing-and-recording"></a>初期化と記録

オーディオの録音、`MediaRecorder`次の手順が必要です。

1. 新しいインスタンスを作成[MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)オブジェクト。

2. 使用して経由でのオーディオ入力をキャプチャするには、どのハードウェア デバイスを指定、 [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/)メソッドです。

3. 出力ファイルのオーディオ ファイルを使用して、設定、 [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/)メソッドです。 サポートされているオーディオ型の一覧については、次を参照してください。 [Android のサポートされているメディア形式](http://developer.android.com/guide/appendix/media-formats.html)です。

4. 呼び出す、 [SetAudioEncoder](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioEncoder/p/Android.Media.AudioEncoder/)オーディオのエンコードの種類を設定します。

5. 呼び出す、 [SetOutputFile](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFile/p/System.String/)にオーディオ データが書き込まれる出力ファイルの名前を指定します。

6. 呼び出す、[準備](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Prepare%28%29/)レコーダーを初期化します。

7. 呼び出す、[開始](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Start%28%29/)記録を開始するメソッド。


次のコード サンプルは、このシーケンスを示しています。

```csharp
protected MediaRecorder recorder;
void RecordAudio (String filePath)
{
  try {
    if (File.Exists (filePath)) {
      File.Delete (filePath);
    }
    if (recorder == null) {
      recorder = new MediaRecorder (); // Initial state.
    } else {
      recorder.Reset ();
      recorder.SetAudioSource (AudioSource.Mic);
      recorder.SetOutputFormat (OutputFormat.ThreeGpp);
      recorder.SetAudioEncoder (AudioEncoder.AmrNb);
      // Initialized state.
      recorder.SetOutputFile (filePath);
      // DataSourceConfigured state.
      recorder.Prepare (); // Prepared state
      recorder.Start (); // Recording state.
    }
  } catch (Exception ex) {
    Console.Out.WriteLine( ex.StackTrace);
  }
}
```


### <a name="stopping-recording"></a>記録を停止します。

記録を停止するには、呼び出し、`Stop`メソッドを`MediaRecorder`:

```csharp
recorder.Stop();
```



### <a name="cleaning-up"></a>クリーンアップ

1 回、`MediaRecorder`された呼び出しが停止している、[リセット](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/)に配置する方法がアイドル状態に戻します。

```csharp
recorder.Reset();
```

ときに、`MediaRecorder`は必要なくなったそのリソース解放する必要が呼び出すことによって、[リリース](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/)メソッド。

```csharp
recorder.Release();
```


## <a name="managing-audio-notifications"></a>オーディオの通知を管理します。



### <a name="the-audiomanager-class"></a>AudioManager クラス

[AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)クラスはアプリケーションのオーディオ イベントが発生したときを知ることのできるオーディオの通知へのアクセスを提供します。 このサービスでは、ボリュームおよび着信音モード コントロールなどのオーディオの機能へのアクセスも提供します。 `AudioManager`オーディオの再生を制御するオーディオの通知を処理するアプリケーションを使用します。



### <a name="managing-audio-focus"></a>オーディオのフォーカスを管理します。

(組み込みのプレーヤーとレコーダー) デバイスのオーディオのリソースは、実行中のすべてのアプリケーションで共有されます。

これは、1 つだけのアプリケーションがキーボード フォーカスを持つデスクトップ コンピューター上のアプリケーションのような概念的には、: キーボード入力がそのアプリケーションにのみがマウス クリックしてでは、実行中のアプリケーションのいずれかを選択すると、します。

オーディオのフォーカス イベントと同様をお勧めし、により、再生やオーディオの録音を同時に複数の 1 つのアプリケーション。 キーボード フォーカスよりも複雑で自主的なであるため&ndash;アプリケーションは、そのという事実にはされていないオーディオ フォーカスがあるかに関係なく再生を無視してかまいません&ndash;さまざまな種類のことができるオーディオのフォーカスがあるため、要求。 など、要求元がのみ必要ですが、非常に短い時間オーディオを再生する場合は、一時的なフォーカスを要求し可能性があります。

オーディオのフォーカス可能性があるすぐに許可または最初に拒否され後で付与します。 たとえば場合、アプリケーション要求オーディオ電話の呼び出し中に、フォーカス拒否されますが、電話の呼び出しが完了したら、フォーカスを付与することが適切です。 この場合、リスナーはオーディオ フォーカスが離れた実行された場合に適切に対応するために登録されます。 オーディオのフォーカスを要求を使用するかどうかは [ok] を再生またはオーディオの録音を決定します。

オーディオのフォーカスの詳細については、次を参照してください。[オーディオのフォーカスを管理する](http://developer.android.com/training/managing-audio/audio-focus.html)です。



#### <a name="registering-the-callback-for-audio-focus"></a>オーディオのフォーカスのコールバックを登録します。

登録、`FocusChangeListener`からのコールバック、`IOnAudioChangeListener`重要な部分を取得し、オーディオのフォーカスを解放します。 これは、後まで遅延できますオーディオ フォーカスの付与するためです。 たとえば、アプリケーションは、進行中の電話の呼び出しがあるときに、音楽を再生する要求可能性があります。 通話が終了するまで、オーディオ フォーカスは許可されません。

このため、コールバック オブジェクトにパラメーターとして渡される、`GetAudioFocus`のメソッド、`AudioManager`であり、この呼び出し、コールバックを登録します。 オーディオのフォーカスが最初に拒否しても後で、許可アプリケーションが通知を起動して`OnAudioFocusChange`コールバックでします。 同じメソッドを使用して、オーディオのフォーカスが離れた行われていることをアプリケーションに指示します。

アプリケーションは、オーディオのリソースの使用が完了したら、それを呼び出す、`AbandonFocus`のメソッド、 `AudioManager`、し、もう一度コールバックに渡します。 これにより、コールバックの登録を解除し、他のアプリケーションは、オーディオのフォーカスを取得可能性がありますので、オーディオのリソースを解放します。



#### <a name="requesting-audio-focus"></a>オーディオのフォーカスを要求します。

デバイスのオーディオのリソースを要求するための手順は、次のとおりです。

1.  ハンドルを取得、`AudioManager`システム サービスです。

2.  コールバック クラスのインスタンスを作成します。

3.  デバイスのオーディオのリソースを呼び出すことによって要求、`RequestAudioFocus`メソッドを`AudioManager`です。 パラメーターは、コールバック オブジェクト、ストリームの種類 (音楽、音声通話、リングなど) と右要求されているアクセスの種類 (オーディオ リソースを要求できます一時的にまたは、無期限など)。

4.  要求が与えられている場合、`playMusic`メソッドがすぐに、呼び出され、オーディオが再生を開始します。

5.  要求が拒否された場合、これ以上の操作は行われません。 この場合、後で、要求が許可される場合、オーディオを再生のみです。


次のコード例は、次の手順を示しています。

```csharp
Boolean RequestAudioResources(INotificationReceiver parent)
{
  AudioManager audioMan = (AudioManager) GetSystemService(Context.AudioService);
  AudioManager.IOnAudioFocusChangeListener listener  = new MyAudioListener(this);
  var ret = audioMan.RequestAudioFocus (listener, Stream.Music, AudioFocus.Gain );
  if (ret == AudioFocusRequest.Granted) {
    playMusic();
    return (true);
  } else if (ret == AudioFocusRequest.Failed) {
    return (false);
  }
  return (false);
}
```


#### <a name="releasing-audio-focus"></a>オーディオのフォーカスを解放します。

トラックの再生が完了したら、`AbandonFocus`メソッド`AudioManager`が呼び出されます。 これにより、デバイスのオーディオのリソースを取得するために別のアプリケーションです。 他のアプリケーションは、独自のリスナーを登録した場合、このオーディオのフォーカス変更の通知が受信されます。


## <a name="low-level-audio-api"></a>低レベルのオーディオ API

低レベルのオーディオの Api は、オーディオを再生およびファイルの Uri を使用する代わりにメモリ バッファーと直接やり取りするための録画をさらに制御を提供します。 このアプローチをお勧め、いくつかのシナリオがあります。 このようなシナリオは次のとおりです。

1.  ときに、暗号化されたオーディオ ファイルから再生します。

2.  一連の短いクリップの再生時にします。

3.  オーディオ ストリーミングします。


### <a name="audiotrack-class"></a>AudioTrack クラス

[AudioTrack](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)クラスの記録、低レベルのオーディオの Api を使用して、低レベルと同じでは、`MediaPlayer`クラスです。


#### <a name="initializing-and-playing"></a>初期化し、再生します。

オーディオの新しいインスタンスを再生する`AudioTrack`インスタンス化する必要があります。 渡される引数リスト、[コンス トラクター](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist)バッファーに格納されているオーディオ サンプルを再生する方法を指定します。 引数は次のとおりです。

1.  ストリームの種類&ndash;音声、着信音、音楽、システムまたはアラームです。

2.  頻度&ndash;サンプリング レートを Hz で表されます。

3.  チャネル構成&ndash;モノラルまたはステレオです。

4.  オーディオ形式&ndash;8 ビットまたは 16 ビット エンコードします。

5.  バッファー サイズ&ndash;(バイト単位)。

6.  バッファー モード&ndash;ストリーミングまたは静的です。


作成した後、[再生](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/)のメソッド`AudioTrack`呼び出されると、再生の開始まで設定できます。 オーディオのバッファーを記述、`AudioTrack`再生を開始します。

```csharp
void PlayAudioTrack(byte[] audioBuffer)
{
  AudioTrack audioTrack = new AudioTrack(
    // Stream type
    Stream.Music,
    // Frequency
    11025,
    // Mono or stereo
    ChannelOut.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length,
    // Mode. Stream or static.
    AudioTrackMode.Stream);

    audioTrack.Play();
    audioTrack.Write(audioBuffer, 0, audioBuffer.Length);
}
```


#### <a name="pausing-and-stopping-the-playback"></a>一時停止と再生を停止しています

呼び出す、[を一時停止](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/)再生を一時停止する方法。

```csharp
audioTrack.Pause();
```

呼び出す、[停止](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/)メソッドでは、再生を完全に終了されます。

```csharp
audioTrack.Stop();
```


#### <a name="cleanup"></a>クリーンアップ

ときに、`AudioTrack`は必要なくなったそのリソース解放する必要が呼び出すことによって[リリース](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```


### <a name="the-audiorecord-class"></a>クラス

[は](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)クラスは、それと同等の`AudioTrack`記録側でします。 同様に`AudioTrack`ファイルと Uri の代わりにメモリ バッファーを直接使用します。 いる必要があります、`RECORD_AUDIO`マニフェストにアクセス許可を設定します。


#### <a name="initializing-and-recording"></a>初期化と記録

新しい構築するためには、まず[は](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)オブジェクト。 渡される引数リスト、[コンス トラクター](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist)の記録に必要なすべての情報を提供します。 異なりで`AudioTrack`大きく列挙型、同等の引数は、引数はここで、`AudioRecord`の整数です。 次の設定があります。

1.  ハードウェア オーディオ入力ソース マイクなどです。

2.  ストリームの種類&ndash;音声、着信音、音楽、システムまたはアラームです。

3.  頻度&ndash;サンプリング レートを Hz で表されます。

4.  チャネル構成&ndash;モノラルまたはステレオです。

5.  オーディオ形式&ndash;8 ビットまたは 16 ビット エンコードします。

6.  バッファー サイズのバイト数


1 回、`AudioRecord`が構築されたその[StartRecording](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/)メソッドが呼び出されます。 記録を開始する準備ができました。 `AudioRecord`継続的に、オーディオ入力バッファーを読み書きをオーディオ ファイルを入力します。

```csharp
void RecordAudio()
{
  byte[] audioBuffer = new byte[100000];
  var audRecorder = new AudioRecord(
    // Hardware source of recording.
    AudioSource.Mic,
    // Frequency
    11025,
    // Mono or stereo
    ChannelIn.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length
  );
  audRecorder.StartRecording();
  while (true) {
    try
    {
      // Keep reading the buffer while there is audio input.
      audRecorder.Read(audioBuffer, 0, audioBuffer.Length);
      // Write out the audio file.
    } catch (Exception ex) {
      Console.Out.WriteLine(ex.Message);
      break;
    }
  }
}
```


#### <a name="stopping-the-recording"></a>記録を停止します。

呼び出す、[停止](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/)メソッドは、録画を終了します。

```csharp
audRecorder.Stop();
```


#### <a name="cleanup"></a>クリーンアップ

ときに、`AudioRecord`オブジェクトが不要になった、呼び出すことその[リリース](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/)メソッドが関連付けられているすべてのリソースを解放します。

```csharp
audRecorder.Release();
```


## <a name="summary"></a>まとめ

Android OS では、再生、録音、およびオーディオを管理するための強力なフレームワークを提供します。 この記事の説明を再生する方法と、高度なを使用してオーディオを録音`MediaPlayer`と`MediaRecorder`クラスです。 次に、オーディオの通知を使用して、異なるアプリケーション間で、デバイスのオーディオのリソースを共有する方法を説明します。 最後に、どのように再生は、低レベルの Api を使用してオーディオを録音インターフェイス メモリ バッファーと直接対処とします。


## <a name="related-links"></a>関連リンク

- [オーディオの操作 (サンプル)](https://developer.xamarin.com/samples/Example_WorkingWithAudio/)
- [Media Player](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [メディア レコーダー](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [Audio Manager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [オーディオ トラック](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [音声のレコーダー](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
