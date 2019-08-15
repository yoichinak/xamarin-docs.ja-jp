---
title: Android オーディオ
description: Android OS では、オーディオとビデオの両方を含むマルチメディアを広範囲にサポートしています。 このガイドでは、Android のオーディオに焦点を当て、組み込みのオーディオプレーヤーとレコーダークラス、および低レベルのオーディオ API を使用したオーディオの再生と録音について説明します。 また、開発者が適切に動作するアプリケーションを構築できるように、他のアプリケーションによってブロードキャストされるオーディオイベントの操作についても説明します。
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: 256871fd225808af1fdebf14c4eb2b43e575e105
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644347"
---
# <a name="android-audio"></a>Android オーディオ

_Android OS では、オーディオとビデオの両方を含むマルチメディアを広範囲にサポートしています。このガイドでは、Android のオーディオに焦点を当て、組み込みのオーディオプレーヤーとレコーダークラス、および低レベルのオーディオ API を使用したオーディオの再生と録音について説明します。また、開発者が適切に動作するアプリケーションを構築できるように、他のアプリケーションによってブロードキャストされるオーディオイベントの操作についても説明します。_


## <a name="overview"></a>概要

最新のモバイルデバイスでは、以前は専用の機器&ndash;カメラ、音楽プレーヤー、ビデオレコーダーが必要だった機能が採用されています。 このため、マルチメディアフレームワークはモバイル Api のファーストクラス機能になりました。

Android では、マルチメディアが幅広くサポートされています。 この記事では、Android でのオーディオの使用について説明し、次のトピックについて説明します。

1.  **MediaPlayer でのオーディオの再生**組み込みクラスを使用してオーディオを再生します。これには、ローカルオーディオファイルや`AudioTrack` 、クラスを使用したストリーミングオーディオファイルが含まれます。 `MediaPlayer` &ndash;

2.  **オーディオの録音**&ndash; 組み込み`MediaRecorder`クラスを使用してオーディオを記録する。

3.  **オーディオ通知の操作**&ndash;オーディオ通知を使用して、オーディオ出力を中断またはキャンセルすることによって、イベント (着信通話など) に正しく応答する、適切に動作するアプリケーションを作成します。

4.  **低レベルのオーディオの操作**メモリバッファーに直接`AudioTrack`書き込むことによって、クラスを使用してオーディオを再生します。 &ndash; `AudioRecord`クラスを使用してオーディオを記録し、メモリバッファーから直接読み取る。


## <a name="requirements"></a>必要条件

このガイドには、Android 2.0 (API レベル 5) 以上が必要です。 Android でのオーディオのデバッグはデバイスで実行する必要があることに注意してください。

**Androidmanifest .xml**で`RECORD_AUDIO`アクセス許可を要求する必要があります。

![レコード\_オーディオが有効になっている Android マニフェストの必要なアクセス許可セクション](android-audio-images/image01.png)



## <a name="playing-audio-with-the-mediaplayer-class"></a>MediaPlayer クラスを使用したオーディオの再生

Android でオーディオを再生する最も簡単な方法は、組み込みの[MediaPlayer](xref:Android.Media.MediaPlayer)クラスを使用することです。
`MediaPlayer`では、ファイルパスを渡すことによって、ローカルまたはリモートのいずれかのファイルを再生できます。 ただし、 `MediaPlayer`の状態は非常に重要であり、そのメソッドの1つを間違った状態で呼び出すと、例外がスローされます。 エラーを回避するには`MediaPlayer` 、以下で説明する順序でと対話することが重要です。



### <a name="initializing-and-playing"></a>初期化と再生

でオーディオを`MediaPlayer`再生するには、次の順序が必要です。

1. 新しい[MediaPlayer](xref:Android.Media.MediaPlayer)オブジェクトをインスタンス化します。

1. [Setdatasource](xref:Android.Media.MediaPlayer.SetDataSource*)メソッドを使用して再生するようにファイルを構成します。

1. [Prepare](xref:Android.Media.MediaPlayer.Prepare)メソッドを呼び出して、プレーヤーを初期化します。

1. [Start](xref:Android.Media.MediaPlayer.Start)メソッドを呼び出して、オーディオ再生を開始します。


次のコード例は、この使用方法を示しています。

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


### <a name="suspending-and-resuming-playback"></a>再生の中断と再開

再生を中断するには、 [Pause](xref:Android.Media.MediaPlayer.Pause)メソッドを呼び出します。

```csharp
player.Pause();
```

一時停止中の再生を再開するには、 [Start](xref:Android.Media.MediaPlayer.Start)メソッドを呼び出します。
これは、再生中の一時停止した場所から再開します。

```csharp
player.Start();
```

プレーヤーで[Stop](xref:Android.Media.MediaPlayer.Stop)メソッドを呼び出すと、進行中の再生が終了します。

```csharp
player.Stop();
```

プレーヤーが不要になったら、 [Release](xref:Android.Media.MediaPlayer.Release)メソッドを呼び出してリソースを解放する必要があります。

```csharp
player.Release();
```



## <a name="using-the-mediarecorder-class-to-record-audio"></a>MediaRecorder クラスを使用したオーディオの記録

Android でオーディオ`MediaPlayer`を記録するためのには、 [mediarecorder](xref:Android.Media.MediaRecorder)クラスが使用されています。 `MediaPlayer`と同様に、状態を区別し、いくつかの状態を遷移して、記録を開始できるポイントに移動します。 オーディオを記録するには、 `RECORD_AUDIO`アクセス許可を設定する必要があります。 アプリケーションのアクセス許可を設定する方法については、「 [AndroidManifest .xml の操作](~/android/platform/android-manifest.md)」を参照してください。


### <a name="initializing-and-recording"></a>初期化と記録

でオーディオを記録`MediaRecorder`するには、次の手順を実行する必要があります。

1. 新しい[Mediarecorder](xref:Android.Media.MediaRecorder)オブジェクトをインスタンス化します。

2. [Setaudiosource](xref:Android.Media.MediaRecorder.SetAudioSource*)メソッドを使用してオーディオ入力のキャプチャに使用するハードウェアデバイスを指定します。

3. [SetOutputFormat](xref:Android.Media.MediaRecorder.SetOutputFormat*)メソッドを使用して、出力ファイルのオーディオ形式を設定します。 サポートされるオーディオの種類の一覧については、「 [Android でサポートされるメディア形式](https://developer.android.com/guide/appendix/media-formats.html)」を参照してください。

4. [Setaudioencoder](xref:Android.Media.MediaRecorder.SetAudioEncoder*)メソッドを呼び出して、オーディオエンコードの種類を設定します。

5. [SetOutputFile](xref:Android.Media.MediaRecorder.SetOutputFile*)メソッドを呼び出して、オーディオデータが書き込まれる出力ファイルの名前を指定します。

6. [準備](xref:Android.Media.MediaRecorder.Prepare)メソッドを呼び出してレコーダーを初期化します。

7. [Start](xref:Android.Media.MediaRecorder.Start)メソッドを呼び出して、記録を開始します。


次のコードサンプルは、このシーケンスを示しています。

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


### <a name="stopping-recording"></a>記録の停止

記録を停止するには、 `Stop` `MediaRecorder`でメソッドを呼び出します。

```csharp
recorder.Stop();
```



### <a name="cleaning-up"></a>クリーンアップしています

`MediaRecorder` が停止したら、[Reset](xref:Android.Media.MediaRecorder.Reset) メソッドを呼び出して、アイドル状態に戻します。

```csharp
recorder.Reset();
```

`MediaRecorder` が不要になった場合は、[Release](xref:Android.Media.MediaRecorder.Release) メソッドを呼び出してそのリソースを解放する必要があります。

```csharp
recorder.Release();
```


## <a name="managing-audio-notifications"></a>オーディオ通知の管理



### <a name="the-audiomanager-class"></a>AudioManager クラス

[Audiomanager](xref:Android.Media.AudioManager)クラスは、オーディオイベントが発生したときにアプリケーションが認識できるようにするオーディオ通知へのアクセスを提供します。 このサービスでは、ボリュームや着信音モードの制御など、他のオーディオ機能にもアクセスできます。 を`AudioManager`使用すると、アプリケーションはオーディオの再生を制御するためにオーディオ通知を処理できます。



### <a name="managing-audio-focus"></a>オーディオフォーカスの管理

デバイスのオーディオリソース (組み込みのプレーヤーとレコーダー) は、実行中のすべてのアプリケーションで共有されます。

概念的には、これは、1つのアプリケーションのみがキーボードフォーカスを持つデスクトップコンピューターのアプリケーションに似ています。実行中のアプリケーションの1つをクリックして選択した後、キーボード入力はそのアプリケーションにのみ適用されます。

オーディオフォーカスは同様のアイデアであり、複数のアプリケーションが同時にオーディオを再生または録音できないようにします。 これは、キーボードフォーカスよりも複雑です。これ&ndash;は、アプリケーションでは、現在オーディオフォーカスがなく&ndash;再生されていないことを無視できます。これは、さまざまな種類のオーディオフォーカスがあるためです。指定. たとえば、要求元が非常に短い時間だけオーディオを再生する場合、一時的なフォーカスを要求することがあります。

オーディオフォーカスはすぐに許可されるか、または最初に拒否され、後で付与されることがあります。 たとえば、通話中にアプリケーションがオーディオフォーカスを要求した場合、その通話は拒否されますが、通話が終了すると、フォーカスが付与される可能性があります。 この場合、オーディオフォーカスが離れると、それに応じて応答するようにリスナーが登録されます。 オーディオフォーカスの要求は、オーディオを再生または録音することができているかどうかを判断するために使用されます。

オーディオフォーカスの詳細については、「[オーディオフォーカスの管理](https://developer.android.com/training/managing-audio/audio-focus.html)」を参照してください。



#### <a name="registering-the-callback-for-audio-focus"></a>オーディオフォーカスのコールバックの登録

からの`IOnAudioChangeListener`コールバックの登録は、オーディオフォーカスの取得と解放の重要な部分です。 `FocusChangeListener` これは、オーディオフォーカスの許可が、後で遅延される可能性があるためです。 たとえば、アプリケーションでは、通話中に音楽を再生するように要求できます。 通話が終了するまで、オーディオフォーカスは付与されません。

このため、コールバックオブジェクトは`GetAudioFocus` `AudioManager`のメソッドにパラメーターとして渡され、この呼び出しによってコールバックが登録されます。 オーディオフォーカスが最初に拒否されたが、後で許可され`OnAudioFocusChange`た場合は、コールバックでを呼び出すことによって、アプリケーションに通知されます。 同じメソッドを使用して、オーディオフォーカスが奪われていることをアプリケーションに通知します。

アプリケーションでオーディオリソースの使用が完了すると、 `AbandonFocus` `AudioManager`のメソッドが呼び出され、コールバックが再度渡されます。 これにより、コールバックが解除され、他のアプリケーションがオーディオフォーカスを取得できるように、オーディオリソースが解放されます。



#### <a name="requesting-audio-focus"></a>要求 (オーディオフォーカスを)

デバイスのオーディオリソースを要求するために必要な手順は次のとおりです。

1.  System サービスへの`AudioManager`ハンドルを取得します。

2.  コールバッククラスのインスタンスを作成します。

3.  でメソッドを`RequestAudioFocus`呼び出して、デバイスのオーディオリソースを要求します。`AudioManager` パラメーターは、コールバックオブジェクト、ストリームの種類 (音楽、音声通話、リングなど) と要求されているアクセス権の種類 (オーディオリソースは、一時的に要求することも、無期限に要求することもできます) です。

4.  要求が許可されると、 `playMusic`メソッドが直ちに呼び出され、オーディオが再生を開始します。

5.  要求が拒否された場合、それ以上の操作は行われません。 この場合、オーディオは、要求が後で許可された場合にのみ再生されます。


次のコードサンプルは、これらの手順を示しています。

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


#### <a name="releasing-audio-focus"></a>リリース (オーディオフォーカスを)

トラックの再生が完了すると、 `AbandonFocus`の`AudioManager`メソッドが呼び出されます。 これにより、別のアプリケーションがデバイスのオーディオリソースを取得できるようになります。 他のアプリケーションは、独自のリスナーを登録している場合に、このオーディオフォーカス変更の通知を受け取ります。


## <a name="low-level-audio-api"></a>低レベルのオーディオ API

低レベルのオーディオ Api は、ファイルの Uri を使用する代わりにメモリバッファーと直接やり取りするため、オーディオの再生と記録をより細かく制御できます。 このアプローチが推奨されるシナリオがいくつかあります。 このようなシナリオを次に示します。

1.  暗号化されたオーディオファイルから再生する場合。

2.  短いクリップを連続再生する場合。

3.  オーディオストリーミング。


### <a name="audiotrack-class"></a>AudioTrack クラス

[Audiotrack](xref:Android.Media.AudioTrack)クラスは、記録のために低レベルのオーディオ api を使用します。これは`MediaPlayer`クラスの下位レベルのオーディオ api です。


#### <a name="initializing-and-playing"></a>初期化と再生

オーディオを再生するには、の`AudioTrack`新しいインスタンスをインスタンス化する必要があります。 [コンストラクター](xref:Android.Media.AudioTrack)に渡された引数リストは、バッファーに格納されているオーディオサンプルの再生方法を指定します。 引数は次のとおりです。

1.  音声、 &ndash;着信音、音楽、システム、またはアラームのストリーム型。

2.  頻度&ndash; -Hz で表されたサンプリングレート。

3.  チャネル構成&ndash; Mono またはステレオ。

4.  オーディオ形式&ndash; 8 ビットまたは16ビットエンコード。

5.  バイト単位&ndash;のバッファーサイズ。

6.  バッファーモード&ndash;のストリーミングまたは静的。


構築後、 `AudioTrack`の[Play](xref:Android.Media.AudioTrack.Play)メソッドが呼び出され、再生を開始するように設定されます。 にオーディオバッファーを書き込むと`AudioTrack` 、再生が開始されます。

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


#### <a name="pausing-and-stopping-the-playback"></a>再生の一時停止と停止

再生を一時停止するには、 [pause](xref:Android.Media.AudioTrack.Pause)メソッドを呼び出します。

```csharp
audioTrack.Pause();
```

[Stop](xref:Android.Media.AudioTrack.Stop)メソッドを呼び出すと、再生が完全に終了します。

```csharp
audioTrack.Stop();
```


#### <a name="cleanup"></a>クリーンアップ

`AudioTrack` が不要になった場合は、[Release](xref:Android.Media.AudioTrack.Release) を呼び出してリソースを解放する必要があります。

```csharp
audioTrack.Release();
```


### <a name="the-audiorecord-class"></a>AudioRecord クラス

[AudioRecord](xref:Android.Media.AudioRecord)クラスは、記録側の`AudioTrack`に相当します。 と`AudioTrack`同様に、ファイルと uri の代わりにメモリバッファーを直接使用します。 マニフェストに`RECORD_AUDIO`アクセス許可を設定する必要があります。


#### <a name="initializing-and-recording"></a>初期化と記録

最初の手順では、新しい[AudioRecord](xref:Android.Media.AudioRecord)オブジェクトを構築します。 [コンストラクター](xref:Android.Media.AudioRecord)に渡される引数リストは、記録に必要なすべての情報を提供します。 引数が主に列挙されるとは異なり、の`AudioRecord`等価の引数は整数です。 `AudioTrack` 不足している機能には次が含まれます。

1.  マイクなどのハードウェアオーディオ入力ソース。

2.  音声、 &ndash;着信音、音楽、システム、またはアラームのストリーム型。

3.  頻度&ndash; -Hz で表されたサンプリングレート。

4.  チャネル構成&ndash; Mono またはステレオ。

5.  オーディオ形式&ndash; 8 ビットまたは16ビットエンコード。

6.  バッファーサイズ (バイト単位)


`AudioRecord` が構築されると、その [startrecording](xref:Android.Media.AudioRecord.StartRecording) メソッドが呼び出されます。 これで、記録を開始する準備ができました。 は`AudioRecord` 、入力のためにオーディオバッファーを継続的に読み取り、この入力をオーディオファイルに書き込みます。

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


#### <a name="stopping-the-recording"></a>記録を停止しています

[Stop](xref:Android.Media.AudioRecord.Stop)メソッドを呼び出すと、記録が終了します。

```csharp
audRecorder.Stop();
```


#### <a name="cleanup"></a>クリーンアップ

`AudioRecord` オブジェクトが不要になった場合、[Release](xref:Android.Media.AudioRecord.Release) メソッドを呼び出すと、関連付けられているすべてのリソースが解放されます。

```csharp
audRecorder.Release();
```


## <a name="summary"></a>まとめ

Android OS は、オーディオを再生、記録、および管理するための強力なフレームワークを提供します。 この記事では、高レベル`MediaPlayer`のクラスと`MediaRecorder`クラスを使用してオーディオを再生および録音する方法について説明します。 次に、オーディオ通知を使用して、デバイスのオーディオリソースを異なるアプリケーション間で共有する方法を説明します。 最後に、メモリバッファーと直接やり取りする低レベルの Api を使用してオーディオを再生し、録音する方法について説明します。


## <a name="related-links"></a>関連リンク

- [オーディオの操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/example-workingwithaudio)
- [Media Player](xref:Android.Media.MediaPlayer)
- [メディアレコーダー](xref:Android.Media.MediaRecorder)
- [オーディオマネージャー](xref:Android.Media.AudioManager)
- [オーディオトラック](xref:Android.Media.AudioTrack)
- [オーディオレコーダー](xref:Android.Media.AudioRecord)
