---
title: 単体テスト
ms.prod: xamarin
ms.assetid: BD959779-3239-79B6-5289-3A9ECDFBD973
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 3129302cbb2fbe9e2757986317da0ec30601b492
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="unit-testing"></a>単体テスト

このドキュメントでは、Xamarin.iOS プロジェクト用の単体テストを作成する方法について説明します。
Xamarin.iOS での単体テストは、Touch.Unit フレームワークを使用して行います。このフレームワークには、iOS テスト ランナーと、単体テストの書き込みのために使い慣れた一連の API を提供する [Touch.Unit](https://github.com/xamarin/Touch.Unit) という NUnit の変更バージョンの両方が含まれます。

## <a name="setting-up-a-test-project"></a>テスト プロジェクトの設定

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

プロジェクトの単体テスト フレームワークを設定する場合、単に **iOS 単体テスト プロジェクト**という種類のプロジェクトをソリューションに追加するだけです。 その場合、ソリューションを右クリックし、**[追加]、[新しいプロジェクトの追加]** の順に選択します。 リストから、**[iOS]、[テスト]、[Unified API]、[iOS 単体テスト プロジェクト]** の順に選択します (C# または F# を選択できます)。

![](touch.unit-images/00.png "C# または F# を選択する")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

プロジェクトの単体テスト フレームワークを設定する場合、単に **iOS 単体テスト プロジェクト**という種類のプロジェクトをソリューションに追加するだけです。 その場合、ソリューションを右クリックし、**[追加]、[新しいプロジェクト...]** の順に選択します。リストから、**[Visual C#]、[iOS]、[単体テスト アプリ (iOS)]** の順に選択します。

![](touch.unit-images/00a.png "単体テスト アプリ iOS")

-----

これで基本的なプロジェクトが作成されます。このプロジェクトは基本的なランナー プログラムを含み、新しい MonoTouch.NUnitLite アセンブリを参照します。プロジェクトは次のようになります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](touch.unit-images/01.png "ソリューション エクスプローラーのプロジェクト")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](touch.unit-images/01a.png "ソリューション エクスプローラーのプロジェクト")

-----

`AppDelegate.cs` クラスにはテスト ランナーが含まれ、次のようになります。

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
        UIWindow window;
        TouchRunner runner;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
                // create a new window instance based on the screen size
                window = new UIWindow (UIScreen.MainScreen.Bounds);
                runner = new TouchRunner (window);

                // register every tests included in the main application/assembly
                runner.Add (System.Reflection.Assembly.GetExecutingAssembly ());

                window.RootViewController = new UINavigationController (runner.GetViewController ());

                // make the window visible
                window.MakeKeyAndVisible ();

                return true;
        }
}
```

## <a name="writing-some-tests"></a>一部のテストの書き込み

これで基本的なシェルの準備ができたので、最初の一連のテストを書き込む必要があります。

テストは、`[TestFixture]` 属性が適用されたクラスを作成することで書き込まれます。 各 TestFixture クラス内では、テスト ランナーで呼び出すすべてのメソッドに `[Test]` 属性を適用する必要があります。 実際のテスト フィクスチャはテスト プロジェクトのどのファイルにも存在できます。

作業をすばやく開始するには、**[追加]、[新規ファイルの追加]** を選択し、Xamarin.iOS グループの **[UnitTests]** を選択します。 これで、以下のように成功したテスト、失敗したテスト、および無視されたテストをそれぞれ 1 つずつ含むスケルトン ファイルが追加されます。

```csharp
using System;
using NUnit.Framework;

namespace Fixtures {

        [TestFixture]
        public class Tests {

                [Test]
                public void Pass ()
                {
                        Assert.True (true);
                }

                [Test]
                public void Fail ()
                {
                        Assert.False (true);
                }

                [Test]
                [Ignore ("another time")]
                public void Ignore ()
                {
                        Assert.True (false);
                }
        }
}
```

## <a name="running-your-tests"></a>テストの実行

ソリューション内でこのプロジェクトを実行するには、プロジェクトを右クリックし、**[Debug Item]\(アイテムのデバッグ\)** または **[Run Item]\(アイテムの実行\)** を選択します。

テスト ランナーでは、登録されるテストを確認し、実行可能なテストを個別に選択できます。

[![](touch.unit-images/02.png "登録済みのテストの一覧")](touch.unit-images/02.png#lightbox) 

[![](touch.unit-images/03.png "個々のテキスト")](touch.unit-images/03.png#lightbox) 

[![](touch.unit-images/04.png "実行結果")](touch.unit-images/04.png#lightbox)

個々のテスト フィクスチャを実行する場合は、入れ子ビューからテスト フィクスチャを選択します。あるいは、"すべて実行" ですべてのテストを実行することもできます。 既定のテストを実行する場合、成功したテスト、失敗したテストおよび無視されたテストが 1 つずつ含まれることが想定されます。 そのレポートは次のようになり、失敗したテストに直接ドリルダウンして、失敗に関する詳細情報を見つけることができます。

[![](touch.unit-images/05.png "サンプル レポート")](touch.unit-images/05.png#lightbox)[![](touch.unit-images/05.png "サンプル レポート")](touch.unit-images/05.png#lightbox) [![](touch.unit-images/05.png "サンプル レポート")](touch.unit-images/05.png#lightbox)

IDE のアプリケーション出力ウィンドウで、実行中のテストとその現在の状態を確認することもできます。

## <a name="writing-new-tests"></a>新しいテストの書き込み

NUnitLite は、[Touch.Unit](https://github.com/xamarin/Touch.Unit) プロジェクトという NUnit の変更バージョンです。 .NET 用の軽量なテスト フレームワークであり、[NUnit](http://nunit.com/) のアイデアに基づいており、その機能のサブセットを提供します。
最小限のリソースを使用し、埋め込みおよびモバイル開発で使用されるものなど、リソースに制限のあるプラットフォームで実行されます。 Xamarin.iOS で使用可能な [NUnitLite API を参照](https://developer.xamarin.com/api/namespace/NUnitLite/)できます。 単体テスト テンプレートで提供される基本的なスケルトンの場合、メイン エントリ ポイントは [Assert クラス](https://developer.xamarin.com/api/type/NUnit.Framework.Assert/) メソッドになります。

assert クラス メソッドに加え、単体テスト機能は、NUnitLite の一部である以下の名前空間に分けられます。

-   [NUnit.Framework](https://developer.xamarin.com/api/namespace/NUnit.Framework/)
-   [NUnit.Constraints](https://developer.xamarin.com/api/namespace/NUnit.Framework.Constraints/)
-   [NUnitLite](https://developer.xamarin.com/api/namespace/NUnitLite/)
-   [NUniteLite.Runner](https://developer.xamarin.com/api/namespace/NUnitLite.Runner/)


Xamarin.iOS に固有の単体テスト ランナーについては、以下に記載されています。

-   [NUnit.UI.TouchRunner](https://developer.xamarin.com/api/type/NUnit.UI.TouchRunner/)
