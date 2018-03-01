---
title: "MacOS の概要"
ms.topic: article
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 5e2f14e7b29f85e838563914089743f56239d7bb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-macos"></a>MacOS の概要


## <a name="what-you-will-need"></a>必要があります。

* 指示に従って、 [Objective C の概要](~/tools/dotnet-embedding/get-started/objective-c/index.md)ガイドです。

* 使用する .NET アセンブリ**Embeddinator 4000**です。

* MacOS Cocoa アプリケーション

実行した後の手順を続行してください、 [Objective C の概要](~/tools/dotnet-embedding/get-started/objective-c/index.md)ガイドです。 .NET アセンブリが既にある場合に直接省略できます**Embeddinator 4000 を使用して**セクションです。

## <a name="creating-a-net-assembly"></a>.NET アセンブリを作成します。

開く必要があります .NET アセンブリをビルドする[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)され、新しい作成**.NET ライブラリ プロジェクト**行う*ファイル > 新しいソリューション > その他の > .NET > ライブラリ*. 次へ をクリックし、*気象*として、*プロジェクト名*、 をクリック*作成*です。

次の手順に従います。

1. 削除、**カスタム**ファイルおよび**プロパティ**フォルダーです。

2. 右クリックして*気象プロジェクト > 追加 > 新しいファイル。*

3. 選択*空のクラス*して**XAMWeatherFetcher**名として [新規] をクリックします。

4. 内容を置き換える*XAMWeatherFetcher.cs*を次のコード。

```csharp
using System;
using System.Json;
using System.Net;

public class XAMWeatherFetcher {

    static string urlTemplate = @"https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22{0}%2C%20{1}%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys";
    public string City { get; private set; }
    public string State { get; private set; }

    public XAMWeatherFetcher (string city, string state)
    {
        City = city;
        State = state;
    }

    public XAMWeatherResult GetWeather ()
    {
        try {
            using (var wc = new WebClient ()) {
                var url = string.Format (urlTemplate, City, State);
                var str = wc.DownloadString (url);
                var json = JsonValue.Parse (str)["query"]["results"]["channel"]["item"]["condition"];
                var result = new XAMWeatherResult (json["temp"], json["text"]);
                return result;
            }
        }
        catch (Exception ex) {
            // Log some of the exception messages
            Console.WriteLine (ex.Message);
            Console.WriteLine (ex.InnerException?.Message);
            Console.WriteLine (ex.InnerException?.InnerException?.Message);

            return null;
        }

    }
}

public class XAMWeatherResult {
    public string Temp { get; private set; }
    public string Text { get; private set; }

    public XAMWeatherResult (string temp, string text)
    {
        Temp = temp;
        Text = text;
    }
}
```

わかります`Using System.Json;`; 以下を実行する必要があります。 この問題を解決すると、エラーを示します。

1. ダブルクリックして、**参照**フォルダーです。

2. をクリックして、**パッケージ**タブです。

3. 確認**System.Json**です。

4. Click **Ok**.

終わったら、上記の必要なことを行うには、ビルド、.NET アセンブリ をクリックして*ビルド メニュー > すべてビルド*⌘ + b のまたはします。 試みなかったメッセージは最上位のステータス バーに表示を作成します。

これを右クリックして*気象*プロジェクト ノードを選択*Finder で明らか*です。 Finder でに移動、 *Bin/debug*フォルダー以外の内側に表示されます**Weather.dll です。**

## <a name="using-embeddinator-4000"></a>Embeddinator 4000 を使用します。

Embeddinator 4000 パッケージ インストーラーを使用してインストールし、インストールした後の新しいターミナル セッションを開始、ことができますを使用する場合、 **objcgen**コマンド (それ以外の場合、その絶対パスを使用できます: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`) です。**objcgen** .NET アセンブリからネイティブ ライブラリを生成する必要なツールです。

開いているターミナルは、 `cd` Weather.dll を含むフォルダーにし、実行**objcgen**次に示す引数を使用。

```shell
cd /Users/Alex/Projects/Weather/Weather/bin/Debug

objcgen --debug --outdir=output -c Weather.dll
```

必要がありますに配置するすべてのもの、**出力**横にディレクトリ*Weather.dll*です。 .NET アセンブリがある場合は置換*Weather.dll*ことと同じ手順に従ってください。

## <a name="using-the-generated-output-in-an-xcode-project"></a>Xcode プロジェクトで生成された出力の使用

Xcode を開き、新しい作成**macOS Cocoa アプリケーション**し名前を付けます**MyWeather**です。 右クリックして、 *MyWeather プロジェクト ノード* *"MyWeather"にファイルを追加*に移動し、**出力**によって作成されたディレクトリ*Embeddinator 4000*、し、次のファイルを追加します。

* bindings.h
* embeddinator.h
* glib.h
* mono-support.h
* mono_embeddinator.h
* objc-support.h
* libWeather.dylib
* Weather.dll

確認してください**ために必要な場合は、項目をコピー**がファイル ダイアログの [オプション] パネルでオンになっています。

確認していく必要があります**libWeather.dylib**と**Weather.dll**アプリ バンドルを取得します。

* をクリックして*MyWeather プロジェクト ノード*です。
* 選択*ビルド フェーズ*タブです。
* 新しい*ファイルのコピー フェーズ*です。
* *先*選択**フレームワーク**を追加および**libWeather.dylib**です。
* 新しい*ファイルのコピー フェーズ*です。
* *先*選択**実行可能ファイル**、追加**Weather.dll**ことを確認し、*コピーでコード署名*がオンになってです。

これで開く**ViewController.m**とその内容を置き換えます。

```objective-c
#import "ViewController.h"
#import "bindings.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    XAMWeatherFetcher * fetcher = [[XAMWeatherFetcher alloc] initWithCity:@"Boston" state:@"MA"];
    XAMWeatherResult * weather = [fetcher getWeather];

    NSString * result;
    if (weather)
        result = [NSString stringWithFormat:@"%@ °F - %@", weather.temp, weather.text];
    else
        result = @"An error occured";

    NSTextField * textField = [NSTextField labelWithString:result];
    [self.view addSubview:textField];
}

@end
```

最後に、Xcode プロジェクトを実行し、次のように表示されます。

![実行されている MyWeather サンプル](macos-images/weather-from-csharp-macos.png)

詳細とより探し求めているサンプルは[ここ](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather)です。
