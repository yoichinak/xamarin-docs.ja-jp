---
title: Xamarin. Mac のストーリーボード–クイックスタート
description: このドキュメントでは、Xamarin. Mac でストーリーボードを使用して macOS ユーザーインターフェイスを構築する方法について簡単に説明します。 ここでは、セグエを作成し、基本設定ウィンドウを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: 39e8e63c7612df95123e1e32edbfb3a8c28ffe37
ms.sourcegitcommit: 513feb0e07558766e3de4a898e53d56b27c20559
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697710"
---
# <a name="storyboards-in-xamarinmac--quick-start"></a>Xamarin. Mac のストーリーボード–クイックスタート

ストーリーボードを使用して Xamarin. Mac アプリのユーザーインターフェイスを定義する方法の概要については、新しい Xamarin. Mac プロジェクトを開始してみましょう。 [ **Mac**  >  **アプリ**  >  **cocoa アプリ**] を選択し、[**次へ**] ボタンをクリックします。

[![新しい Cocoa アプリを追加する](quickstart-images/qs01.png)](quickstart-images/qs01.png#lightbox)

の **アプリ名** を使用 `MacStoryboard` し、[ **次へ** ] ボタンをクリックします。

[![アプリ名の設定](quickstart-images/qs02.png)](quickstart-images/qs02.png#lightbox)

既定の **プロジェクト名** と **ソリューション名** を使用し、[ **作成** ] ボタンをクリックします。

[![プロジェクトとソリューションの名前](quickstart-images/qs03.png)](quickstart-images/qs03.png#lightbox)

**ソリューションエクスプローラー** で、ファイルをダブルクリックし `Main.storyboard` て、Xcode の Interface Builder で編集するために開きます。

[![Xcode Interface Builder でストーリーボードを編集しています。](quickstart-images/qs04.png)](quickstart-images/qs04.png#lightbox)

上の図に示すように、既定のストーリーボードでは、アプリのメニューバーとそのメインウィンドウの両方に、ビューコントローラーとビューが定義されています。 このサンプルアプリでは、ワンサイドにメイン _コンテンツビュー_ を持つ UI と、2番目の _インスペクタービュー_ を作成します。

これを行うには、ストーリーボードに付属している既定のビューコントローラーとビューを削除する必要があります。そのためには Interface Builder で選択し、 **del** キーを押します。

[![既定のビューコントローラーの削除](quickstart-images/qs05.png)](quickstart-images/qs05.png#lightbox)

次に、 `split` **フィルター** 領域に「」と入力し、垂直分割ビューコントローラーを選択して、 _デザインサーフェイス_ にドラッグします。

[![分割ビューコントローラーを検索しています](quickstart-images/qs06.png)](quickstart-images/qs06.png#lightbox)

コントローラーには、自動的に2つの子ビューコントローラー (および関連するビュー) が含まれており、分割ビューの左右左右に接続されています。 分割ビューを親ウィンドウと関連付けるには、 **Control** キーを押し、ウィンドウコントローラー (ウィンドウコントローラーのフレームの青い円) をクリックして、分割ビューコントローラーに線をドラッグします。 ポップアップから [ **ウィンドウコンテンツ** ] を選択します。

[![Windows コンテンツビューの設定](quickstart-images/qs07.png)](quickstart-images/qs07.png#lightbox)

これにより、セグエを使用して2つのインターフェイス要素が関連付けられます。

[![ウィンドウとコンテンツの間のセグエ](quickstart-images/qs08.png)](quickstart-images/qs08.png#lightbox)

分割ビューの左側にテキストビューを配置し、ウィンドウまたは分割ビューのサイズが変更されたときに、使用可能な領域を自動的に埋めるようにします。 分割ビューにアタッチされている一番上のビューコントローラーにテキストビューをドラッグし、 **ピン** 自動レイアウトの制約 (デザインサーフェイスの下部にある右側の2番目のアイコン) をクリックします。

[![制約の構成](quickstart-images/qs09.png)](quickstart-images/qs09.png#lightbox)

ここでは、制約 Segue の上部にある境界ボックスの周りにある4つすべての **I ビーム** アイコンをクリックし、下部にある [ **4 個の制約の追加** ] ボタンをクリックして、必要な制約を追加します。

Visual Studio for Mac に戻り、プロジェクトを実行した場合は、分割ビューの左側にウィンドウまたは分割のサイズが収まるようにテキストビューのサイズが自動的に変更されることに注意してください。

[![実行中のアプリの例。ウィンドウの左側のウィンドウにテキストを表示します。](quickstart-images/qs10.png)](quickstart-images/qs10.png#lightbox)

ここでは、分割ビューの右側をインスペクター領域として使用するため、サイズを小さくして、折りたたむことを許可します。 Xcode に戻り、右側のビューを編集します。これを行うには、デザインサーフェイスでそのビューを選択し、[ **サイズインスペクター**] をクリックします。 ここから、 **幅** を入力し `250` ます。

[![幅の設定](quickstart-images/qs11.png)](quickstart-images/qs11.png#lightbox)

次に、右側を表す分割項目を選択し、より高い **優先度** を設定し、[ **ユーザーが折りたたむ** ] チェックボックスをクリックします。

[![保持する優先度の編集](quickstart-images/qs12.png)](quickstart-images/qs12.png#lightbox)

Visual Studio for Mac に戻ってプロジェクトを実行した場合は、右側のサイズが小さくなり、ウィンドウのサイズが変更されていることに注意してください。

[![実行中のアプリの例。ウィンドウの大きな左ペインにテキストを表示します。](quickstart-images/qs13.png)](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue"></a>

## <a name="defining-a-presentation-segue"></a>プレゼンテーションセグエの定義

ここでは、選択したテキストのプロパティのインスペクターとして機能するように分割ビューの右側をレイアウトします。 コントロールを下のビューにドラッグして、インスペクターの UI をレイアウトします。 最後のコントロールでは、ユーザーが4つのプリセット文字スタイルから選択できるようにする segue を表示します。

ここでは、ボタンをインスペクターに追加し、ビューコントローラーをデザインサーフェイスに追加します。 ビューコントローラーのサイズを Segue するサイズに変更し、4つのボタンを追加します。 次に、インスペクタービューのボタンでキークリックを **制御** し、segue を表すビューコントローラーにドラッグします。

[![ドラッグして、ビューコントローラーに新しいセグエを作成します。](quickstart-images/qs14.png)](quickstart-images/qs14.png#lightbox)

ポップアップメニューから、[ **segue**] を選択します。 

[![ビューコントローラーから segue セグエ型を選択します。](quickstart-images/qs15.png)](quickstart-images/qs15.png#lightbox)

最後に、デザインサーフェイスでセグエを選択し、 **優先するエッジ** を **Left** に設定します。 次に、 **アンカービュー** から、segue をアタッチするボタンに線をドラッグします。

[![ドラッグして新しいセグエを作成するには、ボタンにアンカービューをアタッチします。](quickstart-images/qs16.png)](quickstart-images/qs16.png#lightbox)

Visual Studio for Mac に戻ると、アプリを実行し、インスペクターの [ **なし** ] ボタンをクリックすると、segue が表示されます。

[![実行中のセグエの例。 segue が表示されます。](quickstart-images/qs17.png)](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences"></a>

## <a name="creating-app-preferences"></a>アプリの基本設定を作成する

ほとんどの標準的な macOS アプリには、外観やユーザーアカウントなど、アプリのさまざまな側面を制御する複数のオプションを定義するためのユーザー _設定ダイアログ_ が用意されています。

標準の基本設定ダイアログウィンドウを定義するには、まず、タブビューコントローラーをデザインサーフェイスにドラッグします。

[![Xcode でストーリーボードを編集するには、まず、タブビューコントローラーをデザインサーフェイスにドラッグします。](quickstart-images/qs18.png)](quickstart-images/qs18.png#lightbox)

この場合も、2つの子ビューコントローラーが接続された状態になります。 たとえば、ラベルを中央に配置する各ビューに追加します。

[![制約の設定](quickstart-images/qs19.png)](quickstart-images/qs19.png#lightbox)

次に、ユーザーが [ユーザー **設定** ] メニュー項目を選択したときに [基本設定] ウィンドウを表示します。 メニューバーから [基本設定] メニュー項目を選択し、 **ctrl** キーをクリックして、タブビューコントローラーに線をドラッグします。

[![ドラッグしてセグエを作成する](quickstart-images/qs20.png)](quickstart-images/qs20.png#lightbox)

ポップアップ **からモーダルを選択し** て、このウィンドウをモーダルダイアログとして表示します。

[![[アクションのセグエ] メニューからモーダルセグエの種類を選択します。](quickstart-images/qs21.png)](quickstart-images/qs21.png#lightbox)

変更を保存して Visual Studio for Mac に戻り、アプリを実行して [ **基本設定.** ..] メニュー項目を選択すると、[新しいユーザー設定] ダイアログボックスが表示されます。

[![[新しいユーザー設定] ダイアログボックスを表示する、実行中のセグエの例。](quickstart-images/qs22.png)](quickstart-images/qs22.png#lightbox)

これは、標準の macOS アプリの基本設定ダイアログウィンドウのようには見えません。 この問題を解決するには、ソリューションエクスプローラーの Xamarin アプリのフォルダーに2つのイメージファイルを追加 `Resources` し、Xcode の Interface Builder に戻します。 

タブビューコントローラーを選択し、その **スタイル** を **ツールバー** に切り替えます。 

[![タブバーのスタイルの設定](quickstart-images/qs23.png)](quickstart-images/qs23.png#lightbox)

各タブを選択し、 **ラベル** を付けて、画像のいずれかを選択して表示します。

[![Xcode の各タブの構成](quickstart-images/qs24.png)](quickstart-images/qs24.png#lightbox)

変更を保存し、Visual Studio for Mac に戻り、アプリを実行して [ **基本設定.** ..] メニュー項目を選択すると、標準の macOS アプリのようにダイアログが表示されるようになります。

[![[実行中の設定] ウィンドウの例](quickstart-images/qs25.png)](quickstart-images/qs25.png#lightbox)

詳細については、「イメージ、[メニュー](~/mac/user-interface/menu.md)、[ウィンドウ](~/mac/user-interface/window.md)、および[ダイアログ](~/mac/user-interface/dialog.md)[の操作](~/mac/app-fundamentals/image.md)」のドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
