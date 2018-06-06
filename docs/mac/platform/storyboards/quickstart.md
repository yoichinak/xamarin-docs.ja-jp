---
title: 'Xamarin.Mac: クイック スタートのストーリー ボード'
description: このドキュメントでは、macOS Xamarin.Mac でストーリー ボードでのユーザー インターフェイスの構築にクイック スタートの概要を示します。 これには、segue を作成し、環境設定ウィンドウを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 2bf91a51a55583e2ba8ca1fc09eb3dcd0d9986cf
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792573"
---
# <a name="storyboards-in-xamarinmac--quick-start"></a>Xamarin.Mac: クイック スタートのストーリー ボード

ストーリー ボードを使用して Xamarin.Mac アプリのユーザー インターフェイスを定義する基礎として新しい Xamarin.Mac プロジェクトを開始しましょう。 **[Mac]** > **[アプリ]** > **[Cocoa アプリ]** を選択し、**[次へ]** ボタンをクリックします。

[![](quickstart-images/qs01.png "新しい Cocoa アプリを追加します。")](quickstart-images/qs01.png#lightbox)

使用して、**アプリ名**の`MacStoryboard` をクリックし、**次**ボタン。

[![](quickstart-images/qs02.png "アプリ名の設定")](quickstart-images/qs02.png#lightbox)

既定値を使用して**プロジェクト名**と**ソリューション名** をクリックし、**作成**ボタン。

[![](quickstart-images/qs03.png "プロジェクトとソリューションの名前")](quickstart-images/qs03.png#lightbox)

**ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`編集のため Xcode のインターフェイスのビルダーで開くファイル。

[![](quickstart-images/qs04.png "Xcode でストーリー ボードの編集")](quickstart-images/qs04.png#lightbox)

前述したように既定のストーリー ボードでビュー コント ローラーとビュー アプリのメニュー バーとそれにメイン ウィンドウの両方が定義します。 サンプル アプリのしようとして、メインの UI を作成すること_コンテンツ ビュー_一方の側と_インスペクター ビュー_の 1 秒間です。

これを行うは必要がありますまず既定ビュー コント ローラーを削除して、ストーリー ボードに付属しているビューには、選択インターフェイス ビルダーとキーを押して、**削除**キー。

[![](quickstart-images/qs05.png "既定のビュー コント ローラーを削除します。")](quickstart-images/qs05.png#lightbox)

次に、入力`split`に、**フィルター**領域が垂直方向の分割ビュー コント ローラーを選択し、ドラッグ、_デザイン サーフェイス_:

[![](quickstart-images/qs06.png "分割ビュー コント ローラーの検索")](quickstart-images/qs06.png#lightbox)

コント ローラーが 2 つの子コント ローラーの表示 (とその関連ビュー)、ワイヤード (有線) アップ分割ビューの左右に自動的に含まれることに注意してください。 親ウィンドウを分割ビューを妨害するキーを押して、**コントロール**キー、ウィンドウ コント ローラー (ウィンドウ コント ローラーのフレームに青い円形) をクリックし、分割ビュー コント ローラーに線をドラッグします。 選択**ウィンドウ コンテンツ**ポップアップから。

[![](quickstart-images/qs07.png "Windows を設定するには、コンテンツ ビュー")](quickstart-images/qs07.png#lightbox)

Segue を一緒に使用する 2 つのインターフェイス要素が連結されます。

[![](quickstart-images/qs08.png "ウィンドウとコンテンツの間 Segue")](quickstart-images/qs08.png#lightbox)

分割ビューの左側にテキスト ビューを配置して、それをウィンドウまたは分割ビュー サイズが変更されるときに、使用可能な領域が自動的に入力します。 テキスト ビューを分割ビューにアタッチされているビュー コント ローラー上にドラッグし、をクリックして、 **Pin**自動レイアウト制約 (デザイン画面の下部にある右から 2 つ目のアイコン)。

[![](quickstart-images/qs09.png "制約を構成します。")](quickstart-images/qs09.png#lightbox)

ここからは 4 つのすべてをクリックして、 **i ビーム**境界の周囲のアイコンが制約重なっての上部にあるボックスし、をクリックして、 **4 制約の追加**に必要な制約を追加する下部にあるボタンをクリックします。

Mac 用 Visual Studio に戻り、プロジェクトを実行すると、テキスト ビューが自動的にサイズ変更すると、入力ウィンドウ分割ビューの左側にあるか、分割のサイズが変更されることに注意してください。

[![](quickstart-images/qs10.png "実行されているアプリの例")](quickstart-images/qs10.png#lightbox)

分割ビューの右側にあるインスペクターの領域として使用するので、そのサイズを小さくして縮小することを許可するします。 Xcode に戻るし、デザイン画面で選択し、をクリックして、右側のビューの編集、**サイズ インスペクター**です。 ここでは、次を入力してください、**幅**の`250`:。

[![](quickstart-images/qs11.png "幅の設定")](quickstart-images/qs11.png#lightbox)

高く設定され、右辺を表す分割アイテムの 次へ を選択**を保持している優先度** をクリックし、**ユーザーできます折りたたみ** チェック ボックス。

[![](quickstart-images/qs12.png "保持している優先度の編集")](quickstart-images/qs12.png#lightbox)

Mac 用 Visual Studio に戻りおよびプロジェクトを今すぐ実行するには、右側にあるを保持することに注意してくださいが小さい場合は、サイズとウィンドウがサイズ変更します。

[![](quickstart-images/qs13.png "実行されているアプリの例")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>話題、表示を定義します。

しようとしてレイアウトの選択したテキストのプロパティ インスペクターとして機能する分割ビューの右側にあります。 下のビューにいくつかのコントロールをドラッグすると、インスペクターの UI のレイアウトにおします。 最後のコントロールのユーザーが次の 4 つのプリセットの文字スタイルから選択できる重なってを表示することができます。

ボタンと、デザイン サーフェイスにビュー コント ローラーを追加します。 サイズであるビュー コント ローラーに変更され、4 つのボタンを追加して、重なってします。 次にしましょう**コントロール**キー インスペクターのビューでボタンをクリックし、重なってを表すビュー コント ローラーにドラッグします。

[![](quickstart-images/qs14.png "ドラッグして新しい segue を作成するには")](quickstart-images/qs14.png#lightbox)

ポップアップ メニューから選択します**重なって**: 

[![](quickstart-images/qs15.png "Segue の種類の選択")](quickstart-images/qs15.png#lightbox)

最後を Segue デザイン画面内を選んで設定、**優先エッジ**に**左**です。 行をドラッグし、**アンカー ビュー**にアタッチされている重なっての対象がボタンに。

[![](quickstart-images/qs16.png "ドラッグして新しい segue を作成するには")](quickstart-images/qs16.png#lightbox)

Mac 用 Visual Studio に戻り場合、アプリを実行し、をクリックして、**なし**重なってインスペクターで、ボタンが表示されます。

[![](quickstart-images/qs17.png "実行している segue の例")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>アプリの環境設定を作成します。

ほとんどの標準の macOS アプリを提供、_基本設定 ダイアログ_ユーザー外観やユーザー アカウントなど、アプリのさまざまな側面を制御するいくつかのオプションを定義することができます。

標準の基本設定 ダイアログ ウィンドウを定義するのには、最初、デザイン画面にタブ ビュー コント ローラーをドラッグします。

[![](quickstart-images/qs18.png "Xcode でストーリー ボードの編集")](quickstart-images/qs18.png#lightbox)

ここでも、これは自動的に付属してコント ローラーの表示が接続されている 2 つの子。 たとえばために、追加のラベルがその内部中央各ビューに。

[![](quickstart-images/qs19.png "制約を設定")](quickstart-images/qs19.png#lightbox)

次に、ユーザーが選択したときに、環境設定ウィンドウを表示する、**設定しています.** メニュー項目。 メニュー バーから、設定メニュー項目を選択、**コントロール**キー をクリックし、タブのビュー コント ローラーに線をドラッグします。

[![](quickstart-images/qs20.png "ドラッグして、segue を作成するには")](quickstart-images/qs20.png#lightbox)

ここを選択、ポップアップから**モーダル**モーダル ダイアログ ボックスとしてこのウィンドウを表示します。

[![](quickstart-images/qs21.png "Segue の種類の選択")](quickstart-images/qs21.png#lightbox)

For Mac を Visual Studio に戻り、変更を保存する場合は、アプリの実行を選択して、**設定しています.** メニュー項目を新しい設定のダイアログが表示されます。

[![](quickstart-images/qs22.png "実行している segue の例")](quickstart-images/qs22.png#lightbox)

標準 macOS アプリ 基本設定 ダイアログ ウィンドウのように正しくことに注意してください可能性があります。 これを解決するには、Xamarin.Mac アプリの 2 つのイメージ ファイルを含める`Resources`内のフォルダー、**ソリューション エクスプ ローラー** Xcode のインターフェイスのビルダーに戻ります。

タブのビューのコント ローラーとスイッチを選択してその**スタイル**に**ツールバー**: 

[![](quickstart-images/qs23.png "タブ バー スタイルを設定します。")](quickstart-images/qs23.png#lightbox)

各タブを選択し、**ラベル**表現するために、イメージの 1 つを選択。

[![](quickstart-images/qs24.png "Xcode での各タブの構成")](quickstart-images/qs24.png#lightbox)

For Mac を Visual Studio に戻り、変更を保存する場合は、アプリの実行を選択して、**設定しています.** メニュー項目では、標準的な macOS アプリのように、ダイアログ ボックスが表示されます。

[![](quickstart-images/qs25.png "実行中の環境設定ウィンドウの例")](quickstart-images/qs25.png#lightbox)

詳細についてを参照してください、[イメージを操作](~/mac/app-fundamentals/image.md)、[メニュー](~/mac/user-interface/menu.md)、 [Windows](~/mac/user-interface/window.md)と[ダイアログ](~/mac/user-interface/dialog.md)ドキュメント。

## <a name="related-links"></a>関連リンク

- [MacStoryboard (サンプル)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ウィンドウの操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
