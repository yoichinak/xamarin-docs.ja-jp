---
title: Xamarin.iOS アプリの画面を起動します。
description: この記事では、任意の解像度と印刷の向き、1 つの統合のストーリー ボードを使用して、すべての iOS デバイス用アプリの起動画面を作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2018
ms.openlocfilehash: d5a267bfa8655a9b9c6d4dba9d8cf9d16624ba9b
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="launch-screens"></a>画面を起動します。

_この記事では、任意の解像度と印刷の向き、1 つの統合のストーリー ボードを使用して、すべての iOS デバイス用アプリの起動画面を作成する方法について説明します。_

IOS 8 の場合は、前に、iOS アプリの起動画面を作成するに、さまざまなデバイスのフォーム ファクターと、アプリが実行できなかった解像度の各イメージ資産を提供する開発者が必要です。 IOS 8 のリリース以降が経過しているが常に正しい起動の画面を作成する 1 つの統合のストーリー ボードを使用することです。

この簡単なチュートリアルでは、既定では、新しいプロジェクトで提供されるストーリーまたは既存のプロジェクトに手動で追加されたストーリー ボードの起動画面を作成する方法について説明します。 また、iOS デザイナーを使用して、それらのビューに制約を設定して、ストーリー ボードがさまざまなデバイスや向きが正しいことを確認するストーリー ボードにイメージ ビューとラベルを追加する方法を示します。

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>ストーリー ボードでの起動画面を管理します。

Ios 8 (およびそれ以降) では、開発者は、1 つ以上の静的な起動イメージを使用する代わりに起動画面を提供する特殊な Unified ストーリー ボードを作成できます。 Ios デザイナーを起動ストーリー ボードを作成する場合は、ディスプレイの別の環境ごとに異なるレイアウトを定義するサイズ クラスと自動レイアウトを使用します。 クラスのサイズと自動レイアウトを使用するは、開発者はすべてのデバイスでは良く見える 1 つの起動画面を作成し、環境を表示します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac 用 Visual Studio で新しいプロジェクトを作成 を選択して**ファイル > 新しいソリューション**し選択**1 つのアプリの表示**: 

    ![新しいプロジェクト ウィンドウで選択されている 1 つのアプリの表示](launch-screens-images/launch01.png)

    - 既定では、新しいプロジェクトが含まれます、 **LaunchScreen.storyboard**起動画面インターフェイスを定義するファイル。 
    - 代わりに既存のプロジェクトに起動画面ストーリー ボードを追加するでプロジェクト名を右クリックし、**ソリューション パッド**選択**追加 > 新しいファイル.** し、**起動画面**:

    ![IOS 選択起動画面で、新しいファイル ウィンドウ](launch-screens-images/launch01b.png)

    - ファイルの名前を付けます**LaunchScreen**または独自の別の名前。

2. 起動画面に適切なストーリー ボードを使用するプロジェクトを構成します。

    - ダブルクリックして、 **Info.plist**ファイルで、**ソリューション パッド**編集用に開きます。
    - **起動イメージ**セクションで、ことを確認して**起動画面**適切なストーリー ボードの名前に設定されています。

    ![Info.plist で、セレクターの起動画面](launch-screens-images/launch02.png)

    - 既定では、新しいプロジェクトが使用するよう構成**LaunchScreen.storyboard**起動画面として。

3. イメージの追加、 **Assets.xcassets**資産カタログを起動して画面上で利用できるようにします。 詳細については、次を参照してください。、[資産カタログ イメージ セットに追加するイメージ](~/ios/app-fundamentals/images-icons/displaying-an-image.md)のセクションで、[イメージを表示する](~/ios/app-fundamentals/images-icons/displaying-an-image.md)ガイドです。

4. 開いている**LaunchScreen.storyboard**でダブルクリックして編集するため、**ソリューション パッド**です。

5. デバイスと iOS デザイナーで 起動画面ストーリー ボードをプレビューする方向を選択します。 下部のツールバーで [デバイスの選択] パネルを開き、選択**iPhone 4 秒**と**縦**です。

    ![デバイスの選択 ツールバー](launch-screens-images/launch05.png)

    - デバイスと印刷の向きを選択するだけ変更されている iOS デザイナーがデザインをプレビューする方法に注意してください。 ここで行った選択に関係なく新しく追加された場合を除き、すべてのデバイスや向き間の制約が適用されます、**編集 Traits**  ボタンを指定しない場合に使用されています。 

6. 設定、**背景**ビュー コント ローラーのメイン ビューの色。 ビュー コント ローラーの途中でクリックして、ビューを選択し、背景色を使用して、調整、**プロパティ パッド**:

    ![紫の背景色で 1 つのビュー](launch-screens-images/launch06.png)

7. 追加、**イメージ ビュー**起動画面をそのソースを設定および**イメージ**:

    - ドラッグ、**イメージ ビュー**から、**ツールボックス パッド**ビューの中心にします。
    - **イメージ ビュー**オンにした場合、**ウィジェット**のセクションで、**プロパティ パッド**設定、**イメージ**イメージ セットに既にプロパティ追加された、 **Assets.xcassets**アセット カタログ。 位置の変更およびサイズ、**イメージ ビュー**必要に応じて。
    
    ![そのイメージのプロパティ設定とイメージのビュー](launch-screens-images/launch07.png)

8. 追加、**ラベル**下、**イメージ ビュー**を使用して、**プロパティ パッド**その属性を設定します。 

    ![そのテキストと色のセットを持つラベル](launch-screens-images/launch08.png)

9. 右側のボタンを使用して制約を編集モードに切り替える、**制約ツールバー**:
    
    ![制約を編集モード ボタン](launch-screens-images/launch09.png)

10. 制約を追加、**イメージ ビュー**の高さと幅を設定し、水平方向および垂直方向に中央します。

    ![レイアウトの制約を含むイメージのビュー](launch-screens-images/launch10.png)

    - 制約を追加する方法の詳細については、次を参照してください。 [iOS 用の Xamarin デザイナーに自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)です。

11. 制約を追加、**ラベル**、水平方向に中央固定に配置して、高さと幅が、それにから垂直距離、**イメージ ビュー**:

    ![レイアウトの制約付きのラベル](launch-screens-images/launch11.png)

12. その他のデバイスと設計がすべてのシナリオで意図したとおりになっていることを確認する方向をテストします。 調整が特定のデバイスや印刷の向きにする必要がある場合、使用、**特徴 (traits) 編集**クラスの特定のサイズの制約を追加する。

    ![横方向を使用して X iPhone としてレンダリング起動画面](launch-screens-images/launch12.png)

13. ストーリー ボードに、変更を保存します。 シミュレーターまたはデバイスでアプリを実行して、アプリを起動すると、起動画面が表示されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 新しいプロジェクトを作成します。 Visual Studio で、次のように選択します**ファイル > 新規 > プロジェクト > Visual c# > iPhone と iPad > iOS アプリ (Xamarin)**:。

    ![新しいプロジェクト ウィンドウ、iOS アプリ (Xamarin) で選択されています。](launch-screens-images/launch01.w157.png)

    選択、 **1 つのアプリの表示**テンプレート、およびクリック**OK**:

    ![1 つのビューのアプリケーション テンプレート](launch-screens-images/launch01-2.w157.png)

2. 場合**リソース > LaunchScreen.xib**内に存在する、**ソリューション エクスプ ローラー**、ファイルを右クリックして を選択して削除**削除**です。 このファイルは次の手順でストーリー ボードに置き換えられます。

3. 起動画面として使用するストーリー ボードを作成します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加 > 新しい項目の追加.** 続く**空ストーリー ボード**です。 このストーリー ボードの名前を付けます**LaunchScreen.storyboard**  をクリック**追加**:

    ![[新しい項目の追加] ウィンドウの空のストーリー ボードが選択されています。](launch-screens-images/launch03.w157.png)

4. 使用するプロジェクト構成**LaunchScreen.storyboard**としてその起動画面ストーリー ボード。

    - **ソリューション エクスプローラー**で **Info.plist** ファイルをダブルクリックし、編集用に開きます。 
    - **のビジュアル アセット** タブで、設定**起動画面**に**LaunchScreen**です。

    ![Info.plist で、セレクターの起動画面](launch-screens-images/launch04-vs.png)

5. 起動画面上で利用できるように、プロジェクト内のアセット カタログにイメージを追加します。

    - **ソリューション エクスプ ローラー**を右クリックして**資産カタログ**選択と**アセット カタログの追加**です。 この新しいアセット カタログの名前を付けます**資産**:

    ![[新しい項目の追加] ウィンドウの選択した資産カタログを](launch-screens-images/launch05.w157.png)

    - 新しいイメージ セットを追加、**資産**資産カタログを」の説明に従って、[資産カタログ イメージ セットに追加するイメージ](~/ios/app-fundamentals/images-icons/displaying-an-image.md)のセクション、[イメージを表示する](~/ios/app-fundamentals/images-icons/displaying-an-image.md)ガイドです。

6. 開いている**LaunchScreen.storyboard**でダブルクリックして編集するため、**ソリューション エクスプ ローラー**です。

    - ストーリー ボード ファイルを編集するには、Visual Studio には、Mac ビルド ホストをアクティブな接続が必要があります。 参照してください、 [Mac に接続する](~/ios/get-started/installation/windows/connecting-to-mac/index.md)についてガイドします。

7. デバイスと iOS デザイナーで 起動画面ストーリー ボードをプレビューする方向を選択します。 下部のツールバーで [デバイスの選択] パネルを開き、選択**iPhone 4 秒**と**縦**: 
 
    ![デバイスの選択 ツールバー](launch-screens-images/launch07-vs.png)

    - デバイスと印刷の向きを選択するだけ変更されている iOS デザイナーがデザインをプレビューする方法に注意してください。 ここで行った選択に関係なく新しく追加された場合を除き、すべてのデバイスや向き間の制約が適用されます、**編集 Traits**  ボタンを指定しない場合に使用されています。 

8. 追加、**ビュー コント ローラー**から 1 つをドラッグしてストーリー ボードに、**ツールボックス**デザイン サーフェイスに。 

    ![デザイン画面に、空ビュー コント ローラーの追加](launch-screens-images/launch08-vs.png)

9. 設定、**背景**ビュー コント ローラーのメイン ビューの色。 ビュー コント ローラーの途中でクリックして、ビューを選択し、背景色を使用して、調整、**プロパティ ウィンドウ**:
    
    ![紫の背景色で 1 つのビュー](launch-screens-images/launch09-vs.png)

10. 追加、**イメージ ビュー**起動画面をそのソースを設定および**イメージ**:

    - ドラッグ、**イメージ ビュー**から、**ツールボックス**ビューの中心にします。
    - **イメージ ビュー**オンにした場合も、**ウィジェット**のセクションで、**プロパティ ウィンドウ**設定、**イメージ**イメージを設定するプロパティ既に追加されて、**資産**アセット カタログ。 位置の変更およびサイズ、**イメージ ビュー**必要に応じて。
    
    ![そのイメージのプロパティ設定とイメージのビュー](launch-screens-images/launch10-vs.png)

11. 追加、**ラベル**下、**ビューのイメージ**:

    - ドラッグ、**ラベル**から、**ツールボックス**デザイン サーフェイスに配置して下、**イメージ ビュー**です。
    - 属性を設定、**ラベル**を使用して、**プロパティ ウィンドウ**:

    ![そのテキストと色のセットを持つラベル](launch-screens-images/launch11-vs.png) 

12. 右側のボタンを使用して制約を編集モードに切り替える、**制約ツールバー**:
    
    ![制約を編集モード ボタン](launch-screens-images/launch12-vs.png) 

13. 制約を追加、**イメージ ビュー**の高さと幅を設定し、水平方向および垂直方向に中央します。

    ![レイアウトの制約を含むイメージのビュー](launch-screens-images/launch13-vs.png) 

    - 制約を追加する方法については、次を参照してください。 [iOS 用の Xamarin デザイナーに自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)です。

14. 制約を追加、**ラベル**、水平方向に中央固定に配置して、高さと幅が、それにから垂直距離、**イメージ ビュー**:
    
    ![レイアウトの制約付きのラベル](launch-screens-images/launch14-vs.png) 

15. その他のデバイスと設計がすべてのシナリオで意図したとおりになっていることを確認する方向をテストします。 調整が特定のデバイスや印刷の向きにする必要がある場合、使用、**特徴 (traits) 編集**クラスの特定のサイズの制約を追加する。

    ![横方向を使用して X iPhone としてレンダリング起動画面](launch-screens-images/launch15-vs.png) 

16. ストーリー ボードに、変更を保存します。 シミュレーターまたはデバイスでアプリを実行して、アプリを起動すると、起動画面が表示されます。

-----

> [!NOTE]
> 起動画面として使用されるストーリー ボード_必要があります_だけシンプルで組み込みの UI 要素を含めると**ことはできません**計算を行うか、カスタムのクラスから派生します。

Unified ストーリー ボードの起動画面の作成の詳細についてを参照してください、[動的な起動画面](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens)のセクションで、 [Unified ストーリー ボード](~/ios/user-interface/storyboards/unified-storyboards.md)ガイドです。

## <a name="migrating-to-launch-screen-storyboards"></a>画面のストーリー ボードの起動に移行します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

その起動画面のストーリー ボードを使用する既存のアプリを更新するときに右クリックして、**プロジェクト名**で、**ソリューション エクスプ ローラー**選択**追加** > **新しいファイル.**.選択**iOS** > **起動画面** をクリックし、**新規**ボタン。

![](launch-screens-images/storyboard02.png "IOS の起動画面を選択します。")

次をダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。 **起動画面**、上記で作成した新しいストーリー ボード ファイルを選択します。

![](launch-screens-images/storyboard09.png "上記で作成した新しいストーリー ボード ファイルを選択します。")


起動画面として新しいストーリー ボードを使用するには、次の操作を行います。

1. ダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
2. スクロールして、**ユニバーサル起動イメージ**エディターを開くのセクション、**起動画面**上記ストーリー ボードの名前で作成したドロップダウン リストを選択。 

    ![](launch-screens-images/storyboard08.png "ストーリー ボードに起動画面の設定")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. プロジェクト名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいファイル.**: 

    ![](launch-screens-images/image012.png "新しいファイルを追加します。")
2. 起動画面の名前を入力し、クリックして、**追加**ボタンをクリックします。 

    ![](launch-screens-images/image013.png "起動画面の名前を入力します。")
3. **ソリューション エクスプ ローラー**、ファイルを開いて編集する、新しく作成されたストーリー ボード ファイルをダブルクリックします。
4. いることを確認、**サイズ クラス**に設定されている**、:** と**ビューとして**は**ジェネリック**: 

    ![](launch-screens-images/image016.png "サイズのクラスがいずれかに設定されていることを確認してください: 任意であり、ビューとしてジェネリック")
5. アセンブリ、単純な UI 要素のサイズ クラスからの起動画面 (など`UIImageView`) と、アプリケーションのバンドルに含まれているイメージ。 

    ![](launch-screens-images/image017.png "アセンブリ iOS デザイナーでの起動画面")
6. ストーリー ボードに、変更を保存します。

-----

## <a name="related-links"></a>関連リンク

- [動的な起動画面 (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [統合ストーリーボード](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS Designer の基本](~/ios/user-interface/designer/index.md)
- [資産カタログ イメージに追加するイメージを設定します。](~/ios/app-fundamentals/images-icons/displaying-an-image.md#asset-catalogs)
- [IOS 用の Xamarin デザイナーに自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)
- [ヒューマン インターフェイス ガイドライン: 起動画面](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
