---
title: Xamarin.Mac – クイック スタートでストーリー ボード
description: このドキュメントでは、macOS ユーザー インターフェイスを Xamarin.Mac でストーリー ボードを構築するためのクイック スタートの概要を示します。 これには、セグエを作成し、[preferences] ウィンドウを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: 7f7d23a01a3c3c6567d6bab45d0abbfb078fb512
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61033601"
---
# <a name="storyboards-in-xamarinmac-quick-start"></a>Xamarin.Mac – クイック スタートでストーリー ボード

ストーリー ボードを使用して Xamarin.Mac アプリのユーザー インターフェイスを定義する基礎として新しい Xamarin.Mac プロジェクトを開始しましょう。 **[Mac]**  >  **[アプリ]**  >  **[Cocoa アプリ]** を選択し、 **[次へ]** ボタンをクリックします。

[![](quickstart-images/qs01.png "新しい Cocoa アプリを追加します。")](quickstart-images/qs01.png#lightbox)

使用して、**アプリ名**の`MacStoryboard` をクリックし、**次**ボタン。

[![](quickstart-images/qs02.png "アプリ名を設定します。")](quickstart-images/qs02.png#lightbox)

既定値を使用して、**プロジェクト名**と**ソリューション名** をクリックし、**作成**ボタン。

[![](quickstart-images/qs03.png "プロジェクトとソリューション名")](quickstart-images/qs03.png#lightbox)

**ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルを開き、Xcode の Interface Builder での編集します。

[![](quickstart-images/qs04.png "Xcode でストーリー ボードの編集")](quickstart-images/qs04.png#lightbox)

上記をご覧のとおり、既定のストーリー ボードでビュー コント ローラーとビュー、アプリのメニュー バーとそれにメイン ウィンドウの両方が定義します。 サンプル アプリでは、ここでは、main を含む UI を作成する_コンテンツ ビュー_一方の側でと_Inspector View_の 1 秒間です。

これを行うには必要がありますまず既定のビュー コント ローラーを削除して、ビューでストーリー ボードが付属しているキーを押すとインターフェイス ビルダーで選択、**削除**キー。

[![](quickstart-images/qs05.png "既定のビュー コント ローラーを削除します。")](quickstart-images/qs05.png#lightbox)

次に、入力`split`に、**フィルター**領域で、垂直分割ビュー コント ローラーを選択し、上にドラッグ、_デザイン サーフェイス_:

[![](quickstart-images/qs06.png "分割ビュー コント ローラーの検索")](quickstart-images/qs06.png#lightbox)

コント ローラーは 2 つの子ビュー コント ローラー (とその関連するビュー)、ワイヤード (有線) を分割ビューの左側および右側に自動的に含まれることに注意してください。 親ウィンドウに分割ビューを関連付けるには、キーを押して、**コントロール**キー ウィンドウ コント ローラー (ウィンドウ コント ローラーのフレームに青い円) をクリックして、分割ビュー コント ローラーに線をドラッグします。 選択**ウィンドウ コンテンツ**ポップアップから。

[![](quickstart-images/qs07.png "コンテンツ ビューを設定するには、ウィンドウに、")](quickstart-images/qs07.png#lightbox)

セグエを使用して 2 つのインターフェイス要素が連結されます。

[![](quickstart-images/qs08.png "ウィンドウとコンテンツの間のセグエ")](quickstart-images/qs08.png#lightbox)

分割ビューの左側にあるテキスト ビューに配置し、ウィンドウまたは分割ビューのいずれかがサイズ変更時に使用可能な領域を自動的に入力します。 分割ビューにアタッチされているビュー コント ローラー上にテキスト ビューをドラッグし、をクリックして、 **Pin**自動レイアウトの制約 (デザイン画面の下部にある右から 2 つ目のアイコン)。

[![](quickstart-images/qs09.png "制約を構成します。")](quickstart-images/qs09.png#lightbox)

ここで 4 つのすべてをクリックしてしましたが、 **i ビーム**境界の周囲のアイコンが制約ポップ オーバーの上部にあるボックスし、をクリックして、 **4 制約の追加**必要な制約を追加する下部にあるボタン。

Visual Studio for Mac 返され、プロジェクトを実行する場合は、ウィンドウと分割ビューの左側にあるを入力するテキスト ビューが自動的にサイズ変更または分割のサイズが変更されることに注意してください。

[![](quickstart-images/qs10.png "実行されているアプリの例")](quickstart-images/qs10.png#lightbox)

分割ビューの右側にある Inspector 領域として使用するため、サイズは小さくして折りたたむことができるようにします。 Xcode に戻り、デザイン画面で選択してクリックすると、右側のビューの編集、**サイズ インスペクター**します。 ここから入力を**幅**の`250`:

[![](quickstart-images/qs11.png "幅の設定")](quickstart-images/qs11.png#lightbox)

次へ を表す、右側にある分割アイテムを選択しますが、大きい値に設定**を保持している優先度** をクリックし、**ユーザーできます折りたたみ**チェック ボックスをオン。

[![](quickstart-images/qs12.png "保持している優先度の編集")](quickstart-images/qs12.png#lightbox)

戻りましょう。 Visual Studio for Mac と、プロジェクトを今すぐ実行するには、右側に保持する通知が小さい場合は、サイズとウィンドウがサイズ変更します。

[![](quickstart-images/qs13.png "実行されているアプリの例")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>セグエ プレゼンテーションを定義します。

ここのレイアウトを選択したテキストのプロパティについて、インスペクターとして動作する分割ビューの右側にあります。 下のビューに一部のコントロールをドラッグすると、インスペクターの UI のレイアウトにします。 最後のコントロールのユーザーが事前設定された文字の 4 つのスタイルから選択できるポップ オーバーを表示します。

ボタンと、デザイン サーフェイスにビュー コント ローラーを追加します。 ビュー コント ローラーをサイズ変更されます、ポップ オーバーを 4 つのボタンを追加することです。 次に、**コントロール**キー、Inspector View でボタンをクリックし、ポップ オーバーを表すビュー コント ローラーにドラッグします。

[![](quickstart-images/qs14.png "ドラッグして新しいセグエを作成するには")](quickstart-images/qs14.png#lightbox)

選択、ポップアップ メニューから**ポップ オーバー**: 

[![](quickstart-images/qs15.png "セグエの種類の選択")](quickstart-images/qs15.png#lightbox)

最後に、デザイン画面で、セグエを選択しますが、設定と、**優先 Edge**に**左**します。 行をドラッグし、**アンカー ビュー**ポップ オーバーに接続すると考えているボタンに。

[![](quickstart-images/qs16.png "ドラッグして新しいセグエを作成するには")](quickstart-images/qs16.png#lightbox)

Visual studio for Mac を返す場合、アプリを実行して、をクリックして、 **None** Inspector、ポップ オーバー ボタンが表示されます。

[![](quickstart-images/qs17.png "実行している、セグエの例")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>アプリ設定を作成します。

ほとんどの標準の macOS アプリの提供、_基本設定 ダイアログ_外観やユーザー アカウントなど、アプリのさまざまな側面を制御するいくつかのオプションを定義するユーザーをできるようにします。

標準の基本設定 ダイアログ ウィンドウを定義するには、最初に、デザイン サーフェイスにタブ ビュー コント ローラーをドラッグします。

[![](quickstart-images/qs18.png "Xcode でストーリー ボードの編集")](quickstart-images/qs18.png#lightbox)

ここでも、これは自動的に付属してビュー コント ローラーに接続されている 2 つの子。 たとえばために、私たちを追加しますラベルがその内部に中央のそれぞれのビュー。

[![](quickstart-images/qs19.png "制約の設定")](quickstart-images/qs19.png#lightbox)

次に、ユーザーが選択したときに、[preferences] ウィンドウを表示する、**設定しています.** メニュー項目。 メニュー バーから、基本設定 メニュー項目を選択します。**コントロール**キーをクリックし、タブのビュー コント ローラーに線をドラッグします。

[![](quickstart-images/qs20.png "ドラッグしてセグエを作成するには")](quickstart-images/qs20.png#lightbox)

選択、ポップアップから**モーダル**このウィンドウをモーダル ダイアログを表示します。

[![](quickstart-images/qs21.png "セグエの種類の選択")](quickstart-images/qs21.png#lightbox)

アプリを実行し、選択 Visual Studio for Mac に戻り、変更を保存する場合、**設定しています.**  メニュー、ダイアログが表示され、新しい設定。

[![](quickstart-images/qs22.png "実行している、セグエの例")](quickstart-images/qs22.png#lightbox)

標準の macOS アプリの基本設定 ダイアログ ウィンドウはないようだことに注意してください可能性があります。 これを解決するには、Xamarin.Mac アプリの 2 つのイメージ ファイルを含める`Resources`フォルダーで、**ソリューション エクスプ ローラー**し、Xcode の Interface Builder に戻ります。

タブのビュー コント ローラーとスイッチを選択しますその**スタイル**に**ツールバー**:。 

[![](quickstart-images/qs23.png "タブ バー スタイルの設定")](quickstart-images/qs23.png#lightbox)

各タブを選択し、**ラベル**表現するためのイメージのいずれかを選択します。

[![](quickstart-images/qs24.png "Xcode での各タブの構成")](quickstart-images/qs24.png#lightbox)

アプリを実行し、選択 Visual Studio for Mac に戻り、変更を保存する場合、**設定しています.** メニュー項目では、標準の macOS アプリのようなダイアログ ボックスが表示されます。

[![](quickstart-images/qs25.png "実行中の環境設定ウィンドウの例")](quickstart-images/qs25.png#lightbox)

詳細についてを参照してください、[イメージを操作](~/mac/app-fundamentals/image.md)、[メニュー](~/mac/user-interface/menu.md)、 [Windows](~/mac/user-interface/window.md)と[ダイアログ](~/mac/user-interface/dialog.md)ドキュメント。

## <a name="related-links"></a>関連リンク

- [MacStoryboard (サンプル)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
