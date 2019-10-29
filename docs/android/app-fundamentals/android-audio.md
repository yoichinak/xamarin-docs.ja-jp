---
title: Android オーディオ
description: Android OS では、オーディオとビデオの両方を含むマルチメディアを広範囲にサポートしています。 このガイドでは、Android のオーディオに焦点を当て、組み込みのオーディオプレーヤーとレコーダークラス、および低レベルのオーディオ API を使用したオーディオの再生と録音について説明します。 また、開発者が適切に動作するアプリケーションを構築できるように、他のアプリケーションによってブロードキャストされるオーディオイベントの操作についても説明します。
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/28/2018
ms.openlocfilehash: ac59bcbb062dc9dae784d18054048f50094c2145
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018071"
---
# <a name="android-audio"></a>Android オーディオ

_Android OS では、オーディオとビデオの両方を含むマルチメディアを広範囲にサポートしています。このガイドでは、Android のオーディオに焦点を当て、組み込みのオーディオプレーヤーとレコーダークラス、および低レベルのオーディオ API を使用したオーディオの再生と録音について説明します。また、開発者が適切に動作するアプリケーションを構築できるように、他のアプリケーションによってブロードキャストされるオーディオイベントの操作についても説明します。_

## <a name="overview"></a>概要

最新のモバイルデバイスには、カメラ、音楽プレーヤー、およびビデオレコーダー &ndash; 専用の機器を必要とする機能が採用されています。 このため、マルチメディアフレームワークはモバイル Api のファーストクラス機能になりました。

Android では、マルチメディアが幅広くサポートされています。 この記事では、Android でのオーディオの使用について説明し、次のトピックについて説明します。

1. 組み込みの `MediaPlayer` クラスを使用して、`AudioTrack` クラスでローカルオーディオファイルやストリーミングされたオーディオファイルを含むオーディオを再生 &ndash;、 **MediaPlayer でオーディオを再生**します。

2. オーディオを**記録**するには、組み込みの `MediaRecorder` クラスを使用してオーディオを記録 &ndash; ます。

3. オーディオ通知の使用 &ndash; オーディオ通知を使用して、オーディオ出力を中断またはキャンセルすることによって、イベント (着信電話など) に正しく応答する適切に**動作**するアプリケーションを作成できます。

4. メモリバッファーに直接書き込むことによって、`AudioTrack` クラスを使用してオーディオを再生 &ndash;**低レベルのオーディオを操作**します。 `AudioRecord` クラスを使用してオーディオを記録し、メモリバッファーから直接読み取ります。

## <a name="requirements"></a>［要件］

このガイドには、Android 2.0 (API レベル 5) 以上が必要です。 Android でのオーディオのデバッグはデバイスで実行する必要があることに注意してください。

**Androidmanifest .xml**で `RECORD_AUDIO` アクセス許可を要求する必要があります。

![レコード\_オーディオが有効になっている Android マニフェストの必要なアクセス許可セクション](android-audio-images/image01.png)

## <a name="playing-audio-with-the-mediaplayer-class"></a>MediaPlayer クラスを使用したオーディオの再生

Android でオーディオを再生する最も簡単な方法は、組み込みの[MediaPlayer](xref:Android.Media.MediaPlayer)クラスを使用することです。
`MediaPlayer` は、ファイルパスを渡すことによって、ローカルまたはリモートのいずれかのファイルを再生できます。 ただし、`MediaPlayer` は非常に状態が重要であり、そのメソッドのいずれかを間違った状態で呼び出すと、例外がスローされます。 エラーを回避するには、以下で説明する順序で `MediaPlayer` と対話することが重要です。

### <a name="initializing-and-playing"></a>初期化と再生

`MediaPlayer` でオーディオを再生するには、次の順序が必要です。

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

Android でオーディオを記録するために `MediaPlayer` することは、 [mediarecorder](xref:Android.Media.MediaRecorder)クラスです。 `MediaPlayer`と同様に、状態を区別し、いくつかの状態を遷移して、記録を開始できるポイントに移動します。 オーディオを記録するには、`RECORD_AUDIO` のアクセス許可を設定する必要があります。 アプリケーションのアクセス許可を設定する方法については、「 [AndroidManifest .xml の操作](~/android/platform/android-manifest.md)」を参照してください。

### <a name="initializing-and-recording"></a>初期化と記録

`MediaRecorder` でオーディオを記録するには、次の手順を実行する必要があります。

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

記録を停止するには、`MediaRecorder`で `Stop` メソッドを呼び出します。

```csharp
recorder.Stop();
```

### <a name="cleaning-up"></a>クリーンアップしています

`MediaRecorder` が停止したら、 [Reset](xref:Android.Media.MediaRecorder.Reset)メソッドを呼び出して、アイドル状態に戻します。

```csharp
recorder.Reset();
```

`MediaRecorder` が不要になった場合は、 [Release](xref:Android.Media.MediaRecorder.Release)メソッドを呼び出してそのリソースを解放する必要があります。

```csharp
recorder.Release();
```

## <a name="managing-audio-notifications"></a>オーディオ通知の管理

### <a name="the-audiomanager-class"></a>AudioManager クラス

[Audiomanager](xref:Android.Media.AudioManager)クラスは、オーディオイベントが発生したときにアプリケーションが認識できるようにするオーディオ通知へのアクセスを提供します。 このサービスでは、ボリュームや着信音モードの制御など、他のオーディオ機能にもアクセスできます。 `AudioManager` を使用すると、アプリケーションはオーディオの再生を制御するためにオーディオ通知を処理できます。

### <a name="managing-audio-focus"></a>オーディオフォーカスの管理

デバイスのオーディオリソース (組み込みのプレーヤーとレコーダー) は、実行中のすべてのアプリケーションで共有されます。

概念的には、これは、1つのアプリケーションのみがキーボードフォーカスを持つデスクトップコンピューターのアプリケーションに似ています。実行中のアプリケーションの1つをクリックして選択した後、キーボード入力はそのアプリケーションにのみ適用されます。

オーディオフォーカスは同様のアイデアであり、複数のアプリケーションが同時にオーディオを再生または録音できないようにします。 これは、キーボードフォーカスとは関係があります。これは、アプリケーションが、現在オーディオフォーカスを持っていないことと、要求できるさまざまな種類のオーディオフォーカスがあることが原因で、&ndash; にかかわらず再生されることを無視できることを &ndash; するためです。 たとえば、要求元が非常に短い時間だけオーディオを再生する場合、一時的なフォーカスを要求することがあります。

オーディオフォーカスはすぐに許可されるか、または最初に拒否され、後で付与されることがあります。 たとえば、通話中にアプリケーションがオーディオフォーカスを要求した場合、その通話は拒否されますが、通話が終了すると、フォーカスが付与される可能性があります。 この場合、オーディオフォーカスが離れると、それに応じて応答するようにリスナーが登録されます。 オーディオフォーカスの要求は、オーディオを再生または録音することができているかどうかを判断するために使用されます。

オーディオフォーカスの詳細については、「[オーディオフォーカスの管理](https://developer.android.com/training/managing-audio/audio-focus.html)」を参照してください。

#### <a name="registering-the-callback-for-audio-focus"></a>オーディオフォーカスのコールバックの登録

`IOnAudioChangeListener` からの `FocusChangeListener` コールバックの登録は、オーディオフォーカスの取得と解放の重要な部分です。 これは、オーディオフォーカスの許可が、後で遅延される可能性があるためです。 たとえば、アプリケーションでは、通話中に音楽を再生するように要求できます。 通話が終了するまで、オーディオフォーカスは付与されません。

このため、コールバックオブジェクトは、`AudioManager`の `GetAudioFocus` メソッドにパラメーターとして渡され、この呼び出しによってコールバックが登録されます。 オーディオフォーカスが最初に拒否されたが、後で許可された場合は、コールバックで `OnAudioFocusChange` を呼び出すことによって、アプリケーションに通知されます。 同じメソッドを使用して、オーディオフォーカスが奪われていることをアプリケーションに通知します。

アプリケーションでオーディオリソースの使用が完了すると、`AudioManager`の `AbandonFocus` メソッドが呼び出され、コールバックが再度渡されます。 これにより、コールバックが解除され、他のアプリケーションがオーディオフォーカスを取得できるように、オーディオリソースが解放されます。

#### <a name="requesting-audio-focus"></a>要求 (オーディオフォーカスを)

デバイスのオーディオリソースを要求するために必要な手順は次のとおりです。

1. `AudioManager` システムサービスへのハンドルを取得します。

2. コールバッククラスのインスタンスを作成します。

3. `AudioManager` で `RequestAudioFocus` メソッドを呼び出して、デバイスのオーディオリソースを要求します。 パラメーターは、コールバックオブジェクト、ストリームの種類 (音楽、音声通話、リングなど) と要求されているアクセス権の種類 (オーディオリソースは、一時的に要求することも、無期限に要求することもできます) です。

4. 要求が許可されると、`playMusic` メソッドが直ちに呼び出され、オーディオの再生が開始されます。

5. 要求が拒否された場合、それ以上の操作は行われません。 この場合、オーディオは、要求が後で許可された場合にのみ再生されます。

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

トラックの再生が完了すると、`AudioManager` の `AbandonFocus` メソッドが呼び出されます。 これにより、別のアプリケーションがデバイスのオーディオリソースを取得できるようになります。 他のアプリケーションは、独自のリスナーを登録している場合に、このオーディオフォーカス変更の通知を受け取ります。

## <a name="low-level-audio-api"></a>低レベルのオーディオ API

低レベルのオーディオ Api は、ファイルの Uri を使用する代わりにメモリバッファーと直接やり取りするため、オーディオの再生と記録をより細かく制御できます。 このアプローチが推奨されるシナリオがいくつかあります。 このようなシナリオを次に示します。

1. 暗号化されたオーディオファイルから再生する場合。

2. 短いクリップを連続再生する場合。

3. オーディオストリーミング。

### <a name="audiotrack-class"></a>AudioTrack クラス

[Audiotrack](xref:Android.Media.AudioTrack)クラスは、記録のために低レベルのオーディオ api を使用し、`MediaPlayer` クラスの下位レベルに相当します。

#### <a name="initializing-and-playing"></a>初期化と再生

オーディオを再生するには、`AudioTrack` の新しいインスタンスをインスタンス化する必要があります。 [コンストラクター](xref:Android.Media.AudioTrack)に渡された引数リストは、バッファーに格納されているオーディオサンプルの再生方法を指定します。 引数は次のとおりです。

1. 音声、着信音、音楽、システム、またはアラーム &ndash; ストリームの種類。

2. 頻度 &ndash;、Hz で表されたサンプリングレートです。

3. チャネル構成 &ndash; Mono またはステレオ。

4. オーディオ形式 &ndash; 8 ビットまたは16ビットエンコードです。

5. バッファーサイズ &ndash; バイト単位で格納されます。

6. バッファーモード &ndash; streaming または static です。

構築後、`AudioTrack` の[Play](xref:Android.Media.AudioTrack.Play)メソッドが呼び出され、再生を開始するように設定されます。 オーディオバッファーを `AudioTrack` に書き込むと、再生が開始されます。

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

#### <a name="cleanup"></a>清掃

`AudioTrack` が不要になった場合は、 [Release](xref:Android.Media.AudioTrack.Release)を呼び出してリソースを解放する必要があります。

```csharp
audioTrack.Release();
```

### <a name="the-audiorecord-class"></a>AudioRecord クラス

[AudioRecord](xref:Android.Media.AudioRecord)クラスは、記録側の `AudioTrack` に相当します。 `AudioTrack`と同様に、ファイルと Uri の代わりにメモリバッファーを直接使用します。 マニフェストで `RECORD_AUDIO` アクセス許可を設定する必要があります。

#### <a name="initializing-and-recording"></a>初期化と記録

最初の手順では、新しい[AudioRecord](xref:Android.Media.AudioRecord)オブジェクトを構築します。 [コンストラクター](xref:Android.Media.AudioRecord)に渡される引数リストは、記録に必要なすべての情報を提供します。 `AudioTrack`の場合とは異なり、引数が主に列挙型の場合、`AudioRecord` 内の等価の引数は整数になります。 次の設定があります。

1. マイクなどのハードウェアオーディオ入力ソース。

2. 音声、着信音、音楽、システム、またはアラーム &ndash; ストリームの種類。

3. 頻度 &ndash;、Hz で表されたサンプリングレートです。

4. チャネル構成 &ndash; Mono またはステレオ。

5. オーディオ形式 &ndash; 8 ビットまたは16ビットエンコードです。

6. バッファーサイズ (バイト単位)

`AudioRecord` が構築されると、その[Startrecording](xref:Android.Media.AudioRecord.StartRecording)メソッドが呼び出されます。 これで、記録を開始する準備ができました。 `AudioRecord` は、入力のためにオーディオバッファーを継続的に読み取り、この入力をオーディオファイルに書き込みます。

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

#### <a name="cleanup"></a>清掃

`AudioRecord` オブジェクトが不要になった場合、 [Release](xref:Android.Media.AudioRecord.Release)メソッドを呼び出すと、関連付けられているすべてのリソースが解放されます。

```csharp
audRecorder.Release();
```

## <a name="summary"></a>まとめ

Android OS は、オーディオを再生、記録、および管理するための強力なフレームワークを提供します。 この記事では、高レベルの `MediaPlayer` と `MediaRecorder` クラスを使用してオーディオを再生および録音する方法について説明します。 次に、オーディオ通知を使用して、デバイスのオーディオリソースを異なるアプリケーション間で共有する方法を説明します。 最後に、メモリバッファーと直接やり取りする低レベルの Api を使用してオーディオを再生し、録音する方法について説明します。

## <a name="related-links"></a>関連リンク

- [オーディオの操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/example-workingwithaudio)
- [Media Player](xref:Android.Media.MediaPlayer)
- [メディアレコーダー](xref:Android.Media.MediaRecorder)
- [オーディオマネージャー](xref:Android.Media.AudioManager)
- [オーディオトラック](xref:Android.Media.AudioTrack)
- [オーディオレコーダー](xref:Android.Media.AudioRecord)
