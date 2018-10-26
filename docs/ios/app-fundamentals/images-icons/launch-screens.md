---
title: Xamarin.iOS アプリの起動画面
description: この記事では、任意の解像度と 1 つ Unified ストーリー ボードを使用して、向きで、すべての iOS デバイス用アプリの起動画面を作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2018
ms.openlocfilehash: 0ec1defa29a4fe85c4ae3e809d8733e68cc268ac
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116929"
---
# <a name="launch-screens-for-xamarinios-apps"></a>Xamarin.iOS アプリの起動画面

_この記事では、任意の解像度と 1 つ Unified ストーリー ボードを使用して、向きで、すべての iOS デバイス用アプリの起動画面を作成する方法について説明します。_

IOS 8 の場合は、前に iOS アプリの起動画面を作成すると、さまざまなデバイス フォーム ファクターと、アプリが実行される解像度の各イメージ資産を提供する、開発者が必要です。 IOS 8 のリリースでは、ただし、それ以来単一 Unified ストーリー ボードを使用して、次のすべてのケースで適切な起動画面を作成することです。

この簡単なチュートリアルでは、既定では、新しいプロジェクトで提供されるストーリーまたはストーリー ボードを既存のプロジェクトに手動で追加の起動画面を作成する方法について説明します。 IOS Designer を使用して、これらのビューに制約を設定して、ストーリー ボードがさまざまなデバイスや向きの正しいことを確認するストーリー ボードにイメージのビューとラベルを追加する方法を紹介します。

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>ストーリー ボードの起動画面を管理します。

Ios 8 (以降) で、開発者は、1 つまたは複数の静的な起動イメージを使用する代わりに起動画面を提供する特別な Unified ストーリー ボードを作成できます。 IOS Designer の起動のストーリー ボードを作成するときに、さまざまなディスプレイ環境ごとに異なるレイアウトを定義するのにクラスのサイズと自動レイアウトを使用します。 サイズ クラスおよび自動レイアウトを使用して、開発者はすべてのデバイス上で優れた外観を 1 つの起動画面を作成し、環境を表示できます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual studio for Mac では、選択して新しいプロジェクトを作成**ファイル > 新しいソリューション**し**単一ビュー アプリ**: 

    ![新しいプロジェクト] ウィンドウの [単一ビュー アプリが選択されていると、](launch-screens-images/launch01.png)

    - 既定では、新しいプロジェクトが含まれます、 **LaunchScreen.storyboard**起動画面のインターフェイスを定義するファイル。 
    - 代わりに起動画面ストーリー ボードを既存のプロジェクトに追加するでプロジェクト名を右クリックし、 **Solution Pad**選択**追加 > 新しいファイル.** 選び**起動画面**:

    ![IOS の起動画面が選択されていると、新しいファイル ウィンドウ](launch-screens-images/launch01b.png)

    - ファイルに名前を**LaunchScreen**または任意の別の名前。

2. 起動画面に適切なストーリー ボードを使用するプロジェクトを構成します。

    - ダブルクリックして、 **Info.plist**ファイル、 **Solution Pad**編集用に開きます。
    - **起動イメージ**セクションで、ことを確認します**起動画面**適切なストーリー ボードの名前に設定されます。

    ![Info.plist で起動画面セレクター](launch-screens-images/launch02.png)

    - 既定では、新しいプロジェクトが使用するよう構成**LaunchScreen.storyboard**起動画面として。

3. イメージの追加、 **Assets.xcassets**資産カタログの起動画面で使用できるようにします。 詳細については、次を参照してください。、[資産カタログ イメージ セットに追加するイメージ](~/ios/app-fundamentals/images-icons/displaying-an-image.md)のセクション、[イメージを表示する](~/ios/app-fundamentals/images-icons/displaying-an-image.md)ガイド。

4. 開いている**LaunchScreen.storyboard**でダブルクリックして編集するため、 **Solution Pad**します。

5. デバイスと iOS Designer の 起動画面ストーリー ボードをプレビューする方向を選択します。 下部にあるツールバーのデバイスの選択パネルを開き、選択**iPhone 4 s**と**縦**します。

    ![デバイスの選択 ツールバー](launch-screens-images/launch05.png)

    - デバイスを選択することに注意してください。 し、印刷の向きがのみ iOS Designer で設計をプレビューする方法を変更します。 ここで行った選択に関係なく新しく追加された場合を除き、すべてのデバイスや向きの制約が適用されます、**編集 Traits**  ボタンを指定しない場合に使用されています。 

6. 設定、**バック グラウンド**ビュー コント ローラーのメイン ビューの色。 ビュー コント ローラーの中心をクリックして、ビューを選択し、調整、背景色を使用して、 **Properties Pad**:

    ![紫の背景色を持つ 1 つのビュー](launch-screens-images/launch06.png)

7. 追加、 **Image View**起動画面にそのソースの設定と**イメージ**:

    - ドラッグ、 **Image View**から、**ツールボックス パッド**ビューの中央にします。
    - **Image View**選択したら、**ウィジェット**のセクション、 **Properties Pad**設定、**イメージ**イメージ セットに既にプロパティ追加、 **Assets.xcassets**アセット カタログ。 位置を変更し、サイズ、 **Image View**必要に応じて。
    
    ![そのイメージのプロパティ セットでのイメージ ビュー](launch-screens-images/launch07.png)

8. 追加、**ラベル**下、 **Image View**を使用して、 **Properties Pad**その属性を設定します。 

    ![そのテキストと色のセットを持つラベル](launch-screens-images/launch08.png)

9. 右側のボタンを使用して、制約の編集モードに切り替えて、**制約ツールバー**:
    
    ![制約の編集モード ボタン](launch-screens-images/launch09.png)

10. 制約を追加、 **Image View**の高さと幅の設定および水平方向および垂直方向に中央揃えにします。

    ![レイアウトの制約でのイメージ ビュー](launch-screens-images/launch10.png)

    - 制約を追加する方法の詳細については、次を参照してください。 [iOS 用の Xamarin のデザイナーを使用した自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)します。

11. 制約を追加、**ラベル**、水平方向に中央固定に配置して、高さと幅を付けることから、垂直方向に距離、 **Image View**:

    ![レイアウトの制約付きのラベル](launch-screens-images/launch11.png)

12. その他のデバイスと、デザインがすべてのシナリオで意図したとおりになっていることを確認する方向をテストします。 調整が特定のデバイスや印刷の向きにする必要がある場合、使用、**編集 Traits**した特定のサイズ クラスを追加する。

    ![IPhone 横向きを使用して X として表示される起動画面](launch-screens-images/launch12.png)

13. ストーリー ボードに変更を保存します。 シミュレーターまたはデバイスでアプリを実行して、アプリが起動すると、起動画面が表示されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 新しいプロジェクトを作成します。 Visual Studio で、次のように選択します**ファイル > 新規 > プロジェクト > Visual c# > iPhone と iPad > iOS アプリ (Xamarin)**:。

    ![新しいプロジェクト ウィンドウ、iOS アプリ (Xamarin) で選択されています。](launch-screens-images/launch01.w157.png)

    選択、**単一ビュー アプリ**テンプレート、およびクリック**OK**:

    ![単一ビュー アプリ テンプレート](launch-screens-images/launch01-2.w157.png)

2. 場合**リソース > LaunchScreen.xib**内に存在する、**ソリューション エクスプ ローラー**、ファイルを右クリックし、選択して削除**削除**します。 このファイルは次の手順でストーリー ボードに置き換えられます。

3. 起動画面として使用するストーリー ボードを作成します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加 > 新しい項目.** 続けて**の空の Storyboard**します。 このストーリー ボードの名前を付けます**LaunchScreen.storyboard**クリック**追加**:

    ![選択されている空のストーリー ボードで、新しい項目の追加ウィンドウ](launch-screens-images/launch03.w157.png)

4. 使用するプロジェクト構成**LaunchScreen.storyboard**としてその起動画面ストーリー ボード。

    - **ソリューション エクスプローラー**で **Info.plist** ファイルをダブルクリックし、編集用に開きます。 
    - **のビジュアル アセット**タブで、設定**起動画面**に**LaunchScreen**します。

    ![Info.plist で起動画面セレクター](launch-screens-images/launch04-vs.png)

5. 起動画面で使用できるように、プロジェクトのアセット カタログにイメージを追加します。

    - **ソリューション エクスプ ローラー**を右クリックして**資産カタログ**選択**資産カタログの追加**します。 この新しいアセット カタログの名前を付けます**資産**:

    ![資産カタログが選択されていると、新しい項目の追加ウィンドウ](launch-screens-images/launch05.w157.png)

    - 新しいイメージ セットを追加、**資産**資産カタログ、」の説明に従って、[資産カタログ イメージ セットに追加するイメージ](~/ios/app-fundamentals/images-icons/displaying-an-image.md)のセクション、[イメージを表示する](~/ios/app-fundamentals/images-icons/displaying-an-image.md)ガイド。

6. 開いている**LaunchScreen.storyboard**でダブルクリックして編集するため、**ソリューション エクスプ ローラー**します。

    - ストーリー ボード ファイルを編集するには、Visual Studio には、Mac ビルド ホストへのアクティブな接続が必要があります。 参照してください、 [Mac への接続](~/ios/get-started/installation/windows/connecting-to-mac/index.md)についてガイドします。

7. デバイスと iOS Designer の 起動画面ストーリー ボードをプレビューする方向を選択します。 下部にあるツールバーのデバイスの選択パネルを開き、選択**iPhone 4 s**と**縦**: 
 
    ![デバイスの選択 ツールバー](launch-screens-images/launch07-vs.png)

    - デバイスを選択することに注意してください。 し、印刷の向きがのみ iOS Designer で設計をプレビューする方法を変更します。 ここで行った選択に関係なく新しく追加された場合を除き、すべてのデバイスや向きの制約が適用されます、**編集 Traits**  ボタンを指定しない場合に使用されています。 

8. 追加、**ビュー コント ローラー**から 1 つをドラッグしてストーリー ボードに、**ツールボックス**デザイン サーフェイスに。 

    ![デザイン画面に、空のビュー コント ローラーの追加](launch-screens-images/launch08-vs.png)

9. 設定、**バック グラウンド**ビュー コント ローラーのメイン ビューの色。 ビュー コント ローラーの中心をクリックして、ビューを選択し、調整、背景色を使用して、**プロパティ ウィンドウ**:
    
    ![紫の背景色を持つ 1 つのビュー](launch-screens-images/launch09-vs.png)

10. 追加、 **Image View**起動画面にそのソースの設定と**イメージ**:

    - ドラッグ、 **Image View**から、**ツールボックス**ビューの中央にします。
    - **Image View**でオンのまま、**ウィジェット**のセクション、**プロパティ ウィンドウ**設定、**イメージ**イメージを設定するプロパティ既に追加されて、**資産**アセット カタログ。 位置を変更し、サイズ、 **Image View**必要に応じて。
    
    ![そのイメージのプロパティ セットでのイメージ ビュー](launch-screens-images/launch10-vs.png)

11. 追加、**ラベル**下、**イメージ ビュー**:

    - ドラッグ、**ラベル**から、**ツールボックス**をデザイン サーフェイスに配置することが以下、 **Image View**します。
    - 属性を設定、**ラベル**を使用して、**プロパティ ウィンドウ**:

    ![そのテキストと色のセットを持つラベル](launch-screens-images/launch11-vs.png) 

12. 右側のボタンを使用して、制約の編集モードに切り替えて、**制約ツールバー**:
    
    ![制約の編集モード ボタン](launch-screens-images/launch12-vs.png) 

13. 制約を追加、 **Image View**の高さと幅の設定および水平方向および垂直方向に中央揃えにします。

    ![レイアウトの制約でのイメージ ビュー](launch-screens-images/launch13-vs.png) 

    - 制約を追加する方法の詳細については、次を参照してください。 [iOS 用の Xamarin のデザイナーを使用した自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)します。

14. 制約を追加、**ラベル**、水平方向に中央固定に配置して、高さと幅を付けることから、垂直方向に距離、 **Image View**:
    
    ![レイアウトの制約付きのラベル](launch-screens-images/launch14-vs.png) 

15. その他のデバイスと、デザインがすべてのシナリオで意図したとおりになっていることを確認する方向をテストします。 調整が特定のデバイスや印刷の向きにする必要がある場合、使用、**編集 Traits**した特定のサイズ クラスを追加する。

    ![IPhone 横向きを使用して X として表示される起動画面](launch-screens-images/launch15-vs.png) 

16. ストーリー ボードに変更を保存します。 シミュレーターまたはデバイスでアプリを実行して、アプリが起動すると、起動画面が表示されます。

-----

> [!NOTE]
> ストーリー ボードが起動画面として使用_する必要があります_だけの単純な組み込みの UI 要素を含めると**ことはできません**計算を行うか、カスタム クラスから派生します。

Unified ストーリー ボードの起動画面の作成の詳細についてを参照してください、[動的起動画面](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens)のセクション、 [Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)ガイド。

## <a name="migrating-to-launch-screen-storyboards"></a>画面のストーリー ボードの起動に移行します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

その起動画面のストーリー ボードを使用する既存のアプリを更新するときに右クリックして、**プロジェクト名**で、**ソリューション エクスプ ローラー**選択**追加** > **新しいファイル.**.選択**iOS** > **起動画面** をクリックし、**新規**ボタン。

![](launch-screens-images/storyboard02.png "IOS の起動画面を選択します。")

次をダブルクリック、`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。 **起動画面**、上記で作成した新しいストーリー ボード ファイルを選択します。

![](launch-screens-images/storyboard09.png "上記で作成した新しいストーリー ボード ファイルを選択します。")


新しいストーリー ボードを起動画面として使用するには、次の操作を行います。

1. ダブルクリックして、`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
2. までスクロールし、**ユニバーサル起動イメージ**開く、エディターのセクション、**起動画面**上、ストーリー ボードの名前で作成し、ドロップダウンを選択します。 

    ![](launch-screens-images/storyboard08.png "ストーリー ボードを起動画面の設定")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. プロジェクト名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいファイル.**: 

    ![](launch-screens-images/image012.png "新しいファイルを追加します。")
2. 起動画面の名前を入力し、クリックして、**追加**ボタンをクリックします。 

    ![](launch-screens-images/image013.png "起動画面の名前を入力します。")
3. **ソリューション エクスプ ローラー**を開き、編集を新しく作成されたストーリー ボード ファイルをダブルクリックします。
4. いることを確認、**サイズ クラス**に設定されている **、:** と**ビューとして**は**ジェネリック**: 

    ![](launch-screens-images/image016.png "サイズ クラスは、いずれかに設定されていることを確認: 任意のビューとしては、ジェネリックと")
5. アセンブリ、単純な UI 要素のサイズ クラスからの起動画面 (など`UIImageView`) と、アプリケーションのバンドルに含めるイメージ。 

    ![](launch-screens-images/image017.png "アセンブリ iOS Designer の起動画面")
6. ストーリー ボードに変更を保存します。

-----

## <a name="related-links"></a>関連リンク

- [動的な起動画面 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [統合ストーリーボード](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS Designer の基本](~/ios/user-interface/designer/index.md)
- [資産カタログ イメージに追加するイメージを設定します。](~/ios/app-fundamentals/images-icons/displaying-an-image.md#adding-images-to-an-asset-catalog-image-set)
- [IOS 用の Xamarin のデザイナーを使用した自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)
- [起動画面のヒューマン インターフェイス ガイドライン:](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
