---
title: Xamarin. Mac のストーリーボード–クイックスタート
description: このドキュメントでは、Xamarin. Mac でストーリーボードを使用して macOS ユーザーインターフェイスを構築する方法について簡単に説明します。 ここでは、セグエを作成し、基本設定ウィンドウを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: 2b8fede37354fd8a899a14c0710bf46e5a82b86a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026205"
---
# <a name="storyboards-in-xamarinmac-quick-start"></a>Xamarin. Mac のストーリーボード–クイックスタート

ストーリーボードを使用して Xamarin. Mac アプリのユーザーインターフェイスを定義する方法の概要については、新しい Xamarin. Mac プロジェクトを開始してみましょう。 **[Mac]**  >  **[アプリ]**  >  **[Cocoa アプリ]** を選択し、 **[次へ]** ボタンをクリックします。

[![](quickstart-images/qs01.png "Adding a new Cocoa App")](quickstart-images/qs01.png#lightbox)

`MacStoryboard` の**アプリ名**を使用し、 **[次へ]** ボタンをクリックします。

[![](quickstart-images/qs02.png "Setting the App Name")](quickstart-images/qs02.png#lightbox)

既定の**プロジェクト名**と**ソリューション名**を使用し、 **[作成]** ボタンをクリックします。

[![](quickstart-images/qs03.png "The project and solution names")](quickstart-images/qs03.png#lightbox)

**ソリューションエクスプローラー**で、`Main.storyboard` ファイルをダブルクリックして、Xcode の Interface Builder で編集するために開きます。

[![](quickstart-images/qs04.png "Editing the storyboard in Xcode")](quickstart-images/qs04.png#lightbox)

上の図に示すように、既定のストーリーボードでは、アプリのメニューバーとそのメインウィンドウの両方に、ビューコントローラーとビューが定義されています。 このサンプルアプリでは、ワンサイドにメイン_コンテンツビュー_を持つ UI と、2番目の_インスペクタービュー_を作成します。

これを行うには、ストーリーボードに付属している既定のビューコントローラーとビューを削除する必要があります。そのためには Interface Builder で選択し、 **del**キーを押します。

[![](quickstart-images/qs05.png "Removing the default view controller")](quickstart-images/qs05.png#lightbox)

次に、**フィルター**領域に `split` 入力し、垂直分割ビューコントローラーを選択して、_デザインサーフェイス_にドラッグします。

[![](quickstart-images/qs06.png "Searching for the split view controller")](quickstart-images/qs06.png#lightbox)

コントローラーには、自動的に2つの子ビューコントローラー (および関連するビュー) が含まれており、分割ビューの左右左右に接続されています。 分割ビューを親ウィンドウと関連付けるには、 **Control**キーを押し、ウィンドウコントローラー (ウィンドウコントローラーのフレームの青い円) をクリックして、分割ビューコントローラーに線をドラッグします。 ポップアップから **[ウィンドウコンテンツ]** を選択します。

[![](quickstart-images/qs07.png "Setting the windows content view")](quickstart-images/qs07.png#lightbox)

これにより、セグエを使用して2つのインターフェイス要素が関連付けられます。

[![](quickstart-images/qs08.png "The Segue between the window and the content")](quickstart-images/qs08.png#lightbox)

分割ビューの左側にテキストビューを配置し、ウィンドウまたは分割ビューのサイズが変更されたときに、使用可能な領域を自動的に埋めるようにします。 分割ビューにアタッチされている一番上のビューコントローラーにテキストビューをドラッグし、**ピン**自動レイアウトの制約 (デザインサーフェイスの下部にある右側の2番目のアイコン) をクリックします。

[![](quickstart-images/qs09.png "Configuring the constraints")](quickstart-images/qs09.png#lightbox)

ここでは、制約 Segue の上部にある境界ボックスの周りにある4つすべての**I ビーム**アイコンをクリックし、下部にある **[4 個の制約の追加]** ボタンをクリックして、必要な制約を追加します。

Visual Studio for Mac に戻り、プロジェクトを実行した場合は、分割ビューの左側にウィンドウまたは分割のサイズが収まるようにテキストビューのサイズが自動的に変更されることに注意してください。

[![](quickstart-images/qs10.png "An example of the app running")](quickstart-images/qs10.png#lightbox)

ここでは、分割ビューの右側をインスペクター領域として使用するため、サイズを小さくして、折りたたむことを許可します。 Xcode に戻り、右側のビューを編集します。これを行うには、デザインサーフェイスでそのビューを選択し、 **[サイズインスペクター]** をクリックします。 ここから、`250`の**幅**を入力します。

[![](quickstart-images/qs11.png "Setting the width")](quickstart-images/qs11.png#lightbox)

次に、右側を表す分割項目を選択し、より高い**優先度**を設定し、 **[ユーザーが折りたたむ]** チェックボックスをクリックします。

[![](quickstart-images/qs12.png "Editing the holding priority")](quickstart-images/qs12.png#lightbox)

Visual Studio for Mac に戻ってプロジェクトを実行した場合は、右側のサイズが小さくなり、ウィンドウのサイズが変更されていることに注意してください。

[![](quickstart-images/qs13.png "An example of the app running")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>プレゼンテーションセグエの定義

ここでは、選択したテキストのプロパティのインスペクターとして機能するように分割ビューの右側をレイアウトします。 コントロールを下のビューにドラッグして、インスペクターの UI をレイアウトします。 最後のコントロールでは、ユーザーが4つのプリセット文字スタイルから選択できるようにする segue を表示します。

ここでは、ボタンをインスペクターに追加し、ビューコントローラーをデザインサーフェイスに追加します。 ビューコントローラーのサイズを Segue するサイズに変更し、4つのボタンを追加します。 次に、インスペクタービューのボタンでキークリックを**制御**し、segue を表すビューコントローラーにドラッグします。

[![](quickstart-images/qs14.png "Dragging to create a new segue")](quickstart-images/qs14.png#lightbox)

ポップアップメニューから、 **[segue]** を選択します。 

[![](quickstart-images/qs15.png "Selecting the segue type")](quickstart-images/qs15.png#lightbox)

最後に、デザインサーフェイスでセグエを選択し、**優先するエッジ**を**Left**に設定します。 次に、**アンカービュー**から、segue をアタッチするボタンに線をドラッグします。

[![](quickstart-images/qs16.png "Dragging to create a new segue")](quickstart-images/qs16.png#lightbox)

Visual Studio for Mac に戻ると、アプリを実行し、インスペクターの **[なし]** ボタンをクリックすると、segue が表示されます。

[![](quickstart-images/qs17.png "An example of the segue running")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>アプリの基本設定を作成する

ほとんどの標準的な macOS アプリには、外観やユーザーアカウントなど、アプリのさまざまな側面を制御する複数のオプションを定義するためのユーザー_設定ダイアログ_が用意されています。

標準の基本設定ダイアログウィンドウを定義するには、まず、タブビューコントローラーをデザインサーフェイスにドラッグします。

[![](quickstart-images/qs18.png "Editing the storyboard in Xcode")](quickstart-images/qs18.png#lightbox)

この場合も、2つの子ビューコントローラーが接続された状態になります。 たとえば、ラベルを中央に配置する各ビューに追加します。

[![](quickstart-images/qs19.png "Setting the constraints")](quickstart-images/qs19.png#lightbox)

次に、ユーザーが [ユーザー**設定**] メニュー項目を選択したときに [基本設定] ウィンドウを表示します。 メニューバーから [基本設定] メニュー項目を選択し、 **ctrl**キーをクリックして、タブビューコントローラーに線をドラッグします。

[![](quickstart-images/qs20.png "Dragging to create a segue")](quickstart-images/qs20.png#lightbox)

ポップアップ**からモーダルを選択し**て、このウィンドウをモーダルダイアログとして表示します。

[![](quickstart-images/qs21.png "Selecting the segue type")](quickstart-images/qs21.png#lightbox)

変更を保存して Visual Studio for Mac に戻り、アプリを実行して [**基本設定.** ..] メニュー項目を選択すると、[新しいユーザー設定] ダイアログボックスが表示されます。

[![](quickstart-images/qs22.png "An example of the segue running")](quickstart-images/qs22.png#lightbox)

これは、標準の macOS アプリの基本設定ダイアログウィンドウのようには見えません。 この問題を解決するには、2つのイメージファイルを**ソリューションエクスプローラー**の Xcode アプリの [`Resources`] フォルダーに追加し、の Interface Builder に戻ります。

タブビューコントローラーを選択し、その**スタイル**を**ツールバー**に切り替えます。 

[![](quickstart-images/qs23.png "Setting the tab bar style")](quickstart-images/qs23.png#lightbox)

各タブを選択し、**ラベル**を付けて、画像のいずれかを選択して表示します。

[![](quickstart-images/qs24.png "Configuring each tab in Xcode")](quickstart-images/qs24.png#lightbox)

変更を保存し、Visual Studio for Mac に戻り、アプリを実行して [**基本設定.** ..] メニュー項目を選択すると、標準の macOS アプリのようにダイアログが表示されるようになります。

[![](quickstart-images/qs25.png "An example of the running preferences window")](quickstart-images/qs25.png#lightbox)

詳細については、「イメージ、[メニュー](~/mac/user-interface/menu.md)、[ウィンドウ](~/mac/user-interface/window.md)、および[ダイアログ](~/mac/user-interface/dialog.md)[の操作](~/mac/app-fundamentals/image.md)」のドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
