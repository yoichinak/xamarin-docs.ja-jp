---
title: Android のオーディオ
description: Android OS は、オーディオとビデオの両方を含むマルチ メディア、広範なサポートを提供します。 このガイドでは、Android でのオーディオを重視し、再生および録音オーディオの低レベルの API だけでなく、組み込みのオーディオ プレーヤーとレコーダー クラス、使用について説明します。 開発者が適切に動作するアプリケーションを作成できるように、他のアプリケーションによってブロードキャストされたオーディオ イベントの処理も説明します。
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: f34a76879c2a38ec8253d8f3dd3230f03d9af32e
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827320"
---
# <a name="android-audio"></a>Android のオーディオ

_Android OS は、オーディオとビデオの両方を含むマルチ メディア、広範なサポートを提供します。このガイドでは、Android でのオーディオを重視し、再生および録音オーディオの低レベルの API だけでなく、組み込みのオーディオ プレーヤーとレコーダー クラス、使用について説明します。開発者が適切に動作するアプリケーションを作成できるように、他のアプリケーションによってブロードキャストされたオーディオ イベントの処理も説明します。_


## <a name="overview"></a>概要

最新のモバイル デバイスが必要な機器の専用の情報が以前の機能を採用している&ndash;カメラ、ミュージック プレーヤー、ビデオ レコーダーです。 このため、マルチ メディアのフレームワークにモバイル Api の優れた機能になります。

Android では、マルチ メディアの広範なサポートを提供します。 この記事では、Android でのオーディオに関する作業を検査し、次のトピックについて説明します

1.  **Media Player でオーディオの再生**&ndash;組み込みの`MediaPlayer`オーディオ ファイルをローカルでストリーミングされたオーディオ ファイルなど、オーディオを再生するにはクラス、`AudioTrack`クラス。

2.  **オーディオの録音**&ndash;組み込みの`MediaRecorder`オーディオを録音するクラス。

3.  **オーディオの通知の操作**&ndash;オーディオの通知を使用して中断するか、それらのオーディオ出力を取り消すことによって (電話の着信呼び出し) などのイベントに応答するアプリケーションを適切に動作を作成します。

4.  **オーディオの低レベルの操作**&ndash;オーディオを使用して再生、`AudioTrack`メモリ バッファーに直接記述することでクラス。 使用してオーディオを記録、`AudioRecord`クラスし、メモリ バッファーから直接読み取る。


## <a name="requirements"></a>必要条件

このガイドには、Android 2.0 (API レベル 5) が必要がありますまたはそれ以降。 デバイスで Android 上のオーディオをデバッグを行う必要あることに注意してください。

要求する必要があります、`RECORD_AUDIO`でのアクセス許可**AndroidManifest.XML**:

![Android マニフェストをレコードのアクセス許可のセクションに必要な\_オーディオを有効になっています。](android-audio-images/image01.png)



## <a name="playing-audio-with-the-mediaplayer-class"></a>MediaPlayer クラスを使用してオーディオを再生

Android でオーディオを再生する最も簡単な方法は、組み込みの[MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)クラス。
`MediaPlayer` ファイルのパスを渡すことで、ローカルまたはリモートのいずれかのファイルを再生できます。 ただし、`MediaPlayer`非常に状態を区別し、正しくない状態でそのメソッドのいずれかの呼び出しがスローされる例外が発生します。 対話することが重要`MediaPlayer`エラーを回避するために以下の順序で。



### <a name="initializing-and-playing"></a>初期化し、再生します。

オーディオを再生`MediaPlayer`次のシーケンスが必要です。

1. 新しいインスタンスを作成[MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)オブジェクト。

1. 構成ファイルを使用して再生、 [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/)メソッド。

1. 呼び出す、[準備](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/)プレイヤーを初期化します。

1. 呼び出す、[開始](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/)オーディオ再生を開始するメソッド。


次のコード例では、この使用法を示します。

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


### <a name="suspending-and-resuming-playback"></a>保留および再開の再生

呼び出すことによって、再生を中断できる、[一時停止](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/)メソッド。

```csharp
player.Pause();
```

一時停止中の再生を再開するには、呼び出し、[開始](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/)メソッド。
これは、再生時に一時停止した場所から再開されます。

```csharp
player.Start();
```

呼び出す、[停止](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/)プレーヤーでメソッドが進行中の再生を終了します。

```csharp
player.Stop();
```

呼び出すことによって、リソースを解放する必要があります、プレーヤーを不要になったとき、[リリース](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/)メソッド。

```csharp
player.Release();
```



## <a name="using-the-mediarecorder-class-to-record-audio"></a>オーディオの記録を MediaRecorder クラスを使用します。

それぞれ異なるため、の`MediaPlayer`Android でオーディオの録音が、 [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)クラス。 ように、`MediaPlayer`状態を区別する、記録を開始できるポイントを取得するいくつかの状態を遷移します。 オーディオを記録するために、`RECORD_AUDIO`アクセス許可を設定する必要があります。 アプリケーションを設定する方法についてのアクセス許可を参照してください[AndroidManifest.xml 操作](~/android/platform/android-manifest.md)します。


### <a name="initializing-and-recording"></a>初期化と記録

オーディオの録音、`MediaRecorder`次の手順が必要です。

1. 新しいインスタンスを作成[MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)オブジェクト。

2. 使用して経由でのオーディオ入力をキャプチャするハードウェア デバイスを指定して、 [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/)メソッド。

3. 出力ファイル形式のオーディオ形式を使用して、設定、 [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/)メソッド。 サポートされているオーディオの種類の一覧については、次を参照してください。 [Android サポートされているメディア形式](https://developer.android.com/guide/appendix/media-formats.html)します。

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


### <a name="stopping-recording"></a>記録を停止しています

記録を停止するには、呼び出し、`Stop`メソッドを`MediaRecorder`:

```csharp
recorder.Stop();
```



### <a name="cleaning-up"></a>クリーンアップします。

1 回、`MediaRecorder`が停止すると、呼び出し、[リセット](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/)がアイドル状態に配置する方法。

```csharp
recorder.Reset();
```

ときに、`MediaRecorder`が不要になったそのリソース解放する必要が呼び出すことによって、[リリース](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/)メソッド。

```csharp
recorder.Release();
```


## <a name="managing-audio-notifications"></a>オーディオの通知を管理します。



### <a name="the-audiomanager-class"></a>AudioManager クラス

[AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)クラスには、アプリケーションがオーディオのイベントが発生した場合を知ることができますオーディオの通知へのアクセスが用意されています。 このサービスには、ボリュームや着信音のモードの管理などの他のオーディオ機能へのアクセスも提供します。 `AudioManager`により、オーディオ再生を制御するオーディオの通知を処理するアプリケーション。



### <a name="managing-audio-focus"></a>オーディオのフォーカスを管理します。

オーディオ デバイス (組み込みのプレーヤーとレコーダー) のリソースは、実行中のすべてのアプリケーションによって共有されます。

概念的には、これは 1 つだけのアプリケーションがキーボード フォーカスを持つデスクトップ コンピューター上のアプリケーションに似ています。 マウス クリックでは、実行中のアプリケーションのいずれかを選択すると、キーボード入力がそのアプリケーションにのみ移動できます。

オーディオのフォーカスは、同様の考え方ですし、再生またはオーディオの録音を同時に 1 つ以上のアプリケーションを防止します。 キーボード フォーカスよりも複雑で自主的なであるため&ndash;ことは現在オーディオのフォーカスがあるし、再生かに関係なく、事実を無視してかまいませんアプリケーション&ndash;とさまざまな種類のことができるオーディオのフォーカスがあるため、要求されました。 たとえば、要求元が非常に短い時間でオーディオを再生するのみ必要ですが場合、は、一時的なフォーカスを要求できます。

オーディオ フォーカス可能性がある、すぐに許可または最初に拒否され後で付与します。 たとえば場合、アプリケーション要求オーディオ フォーカス電話の呼び出し中に、拒否されますが、電話呼び出しが完了すると、フォーカスを与えるも可能性があります。 この場合、オーディオのフォーカスがすぐに実行された場合、それに応じて応答するためにリスナーが登録されています。 オーディオのフォーカスを要求すると、かを判断するかどうか、[ok] を再生するオーディオの録音に使用されます。

オーディオのフォーカスの詳細については、次を参照してください。[オーディオのフォーカスを管理する](https://developer.android.com/training/managing-audio/audio-focus.html)します。



#### <a name="registering-the-callback-for-audio-focus"></a>オーディオのフォーカスのコールバックを登録します。

登録、`FocusChangeListener`からのコールバック、`IOnAudioChangeListener`取得およびオーディオのフォーカスを解放するのに重要です。 これはため後まで延期されるオーディオのフォーカスを与える可能性があります。 たとえば、アプリケーションは、電話の呼び出しが進行中の中に音楽を再生する要求可能性があります。 電話の呼び出しが完了するまで、オーディオのフォーカスは付与されません。

このため、コールバック オブジェクトにパラメーターとして渡される、`GetAudioFocus`のメソッド、 `AudioManager`、このコールバックを登録する呼び出しと。 呼び出すことによって、アプリケーションは通知オーディオ フォーカスは、最初に拒否が後で与え、`OnAudioFocusChange`コールバック。 オーディオのフォーカスが離れた行われていることをアプリケーションに指示する、同じメソッドを使用します。

アプリケーションは、オーディオのリソースの使用が完了したら、それを呼び出す、`AbandonFocus`のメソッド、 `AudioManager`、し、もう一度コールバックに渡します。 これにより、コールバックの登録を解除し、他のアプリケーションでは、オーディオ フォーカスを取得します。 ようににオーディオのリソースを解放します。



#### <a name="requesting-audio-focus"></a>オーディオのフォーカスを要求します。

デバイスのオーディオのリソースを要求するために必要な手順は次のようです。

1.  ハンドルを取得、`AudioManager`システム サービスです。

2.  コールバック クラスのインスタンスを作成します。

3.  デバイスのオーディオのリソースを呼び出すことによって要求、`RequestAudioFocus`メソッドを`AudioManager`します。 パラメーターは、コールバック オブジェクト、ストリームの種類 (音楽、音声通話、リングなど) と右要求されているアクセスの種類 (オーディオのリソースを要求できます一時的または無期限など)。

4.  要求が与えられている場合、 `playMusic` 、すぐにメソッドが呼び出され、オーディオが再生を開始します。

5.  要求が拒否された場合、これ以上の操作は行われません。 この場合は、後で、要求が与えられている場合、オーディオを再生のみ。


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


#### <a name="releasing-audio-focus"></a>オーディオのフォーカスを解放

トラックの再生が完了すると、`AbandonFocus`メソッド`AudioManager`が呼び出されます。 これにより、別のアプリケーション、デバイスのオーディオのリソースを取得できます。 他のアプリケーションは、独自のリスナーを登録した場合、このオーディオのフォーカスが変更の通知が受信されます。


## <a name="low-level-audio-api"></a>低レベルのオーディオ API

オーディオの低レベルの Api は、オーディオ再生およびファイルの Uri を使用する代わりに、メモリ バッファーと直接やり取りしているので、録画をさらに制御を提供します。 この方法が望ましいいくつかのシナリオがあります。 このようなシナリオは次のとおりです。

1.  ときに、暗号化されたオーディオ ファイルから再生します。

2.  一連の短いクリップの再生時にします。

3.  オーディオをストリーミングします。


### <a name="audiotrack-class"></a>AudioTrack クラス

[AudioTrack](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)クラスは、の記録のオーディオの低レベルの Api を使用して、低レベルと同等のでは、`MediaPlayer`クラス。


#### <a name="initializing-and-playing"></a>初期化し、再生します。

新しいインスタンスのオーディオを再生する`AudioTrack`インスタンス化する必要があります。 渡される引数リスト、[コンス トラクター](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist)バッファーに含まれるオーディオのサンプルを再生する方法を指定します。 引数は次のとおりです。

1.  型が Stream&ndash;音声、着信音、音楽、システムまたはアラームです。

2.  頻度&ndash;サンプリング レートが Hz で表されます。

3.  チャネル構成&ndash;Mono またはステレオします。

4.  オーディオ形式&ndash;8 ビットまたは 16 ビット エンコードします。

5.  バッファー サイズ&ndash;(バイト単位)。

6.  バッファー モード&ndash;ストリーミングまたは静的です。


作成した後、[再生](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/)メソッドの`AudioTrack`再生の開始するための設定に呼び出されます。 オーディオのバッファーを記述、`AudioTrack`再生を開始します。

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

呼び出す、[を一時停止](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/)再生を一時停止するメソッド。

```csharp
audioTrack.Pause();
```

呼び出す、[停止](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/)メソッドが、再生を完全に終了します。

```csharp
audioTrack.Stop();
```


#### <a name="cleanup"></a>クリーンアップ

ときに、`AudioTrack`が不要になったそのリソース解放する必要が呼び出すことによって[リリース](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```


### <a name="the-audiorecord-class"></a>クラス

[は](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)クラスと同等の`AudioTrack`記録側でします。 ような`AudioTrack`ファイルと Uri の代わりにメモリ バッファーを直接使用します。 必要があります、`RECORD_AUDIO`マニフェストにアクセス許可を設定します。


#### <a name="initializing-and-recording"></a>初期化と記録

新しいを作成するには、まず[は](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)オブジェクト。 渡される引数リスト、[コンス トラクター](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist)記録に必要なすべての情報を提供します。 異なりで`AudioTrack`引数には、大きく列挙型、引数は同等である場合、`AudioRecord`は整数です。 不足している機能には次が含まれます。

1.  ハードウェア オーディオ入力ソース マイクなど。

2.  型が Stream&ndash;音声、着信音、音楽、システムまたはアラームです。

3.  頻度&ndash;サンプリング レートが Hz で表されます。

4.  チャネル構成&ndash;Mono またはステレオします。

5.  オーディオ形式&ndash;8 ビットまたは 16 ビット エンコードします。

6.  バッファーのサイズ (バイト)


1 回、`AudioRecord`が構築されたその[StartRecording](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/)メソッドが呼び出されます。 場合によっては、録音を開始する準備ができました。 `AudioRecord`継続的に、入力のオーディオのバッファーの読み書きをオーディオ ファイルを入力します。

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

呼び出す、[停止](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/)メソッドは、記録を終了します。

```csharp
audRecorder.Stop();
```


#### <a name="cleanup"></a>クリーンアップ

ときに、`AudioRecord`オブジェクトが不要になった呼び出しその[リリース](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/)メソッドが関連付けられているすべてのリソースを解放します。

```csharp
audRecorder.Release();
```


## <a name="summary"></a>まとめ

Android OS では、再生を記録し、オーディオを管理するための強力なフレームワークを提供します。 この記事で説明を再生する方法と、高度なを使用してオーディオを録音`MediaPlayer`と`MediaRecorder`クラス。 次に、オーディオの通知を使用して、異なるアプリケーション間でデバイスのオーディオのリソースを共有する方法について説明しました。 最後に、どのようにメモリ バッファーと直接インターフェイスをで再生して低レベルの Api を使用してオーディオを録音する扱うとします。


## <a name="related-links"></a>関連リンク

- [オーディオ ファイルの操作 (サンプル)](https://developer.xamarin.com/samples/monodroid/Example_WorkingWithAudio/)
- [Media Player](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [メディア レコーダー](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [オーディオ マネージャー](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [オーディオ トラック](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [音声のレコーダー](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
