---
title: Xamarin iOS アプリの起動画面
description: この記事では、すべての iOS デバイスのアプリ起動画面を、1つの統合されたストーリーボードを使用して、あらゆる解像度と向きで作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/02/2018
ms.openlocfilehash: f4f24ee8bfc6bdde0becb9539ff9e2f532d06381
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91432026"
---
# <a name="launch-screens-for-xamarinios-apps"></a>Xamarin iOS アプリの起動画面

_この記事では、すべての iOS デバイスのアプリ起動画面を、1つの統合されたストーリーボードを使用して、あらゆる解像度と向きで作成する方法について説明します。_

IOS 8 より前では、iOS アプリの起動画面を作成するために、開発者は、アプリを実行できるさまざまなデバイスフォームの要因や解像度ごとにイメージ資産を提供する必要がありました。 ただし、iOS 8 のリリース以降、1つの統合されたストーリーボードを使用して、すべてのケースで適切に見える起動画面を作成できるようになりました。

この簡単なチュートリアルでは、新しいプロジェクトで既定で提供されるストーリーボードを使用するか、既存のプロジェクトに手動で追加されたストーリーボードを使用して起動画面を作成する方法について説明します。 次に、iOS Designer を使用して、イメージビューとラベルをストーリーボードに追加し、それらのビューに制約を設定し、さまざまなデバイスと向きに対してストーリーボードが正しく表示されることを確認する方法を示します。

<a name="storyboard"></a>

## <a name="managing-launch-screens-with-storyboards"></a>ストーリーボードを使用した起動画面の管理

IOS 8 (以降) では、開発者は、1つまたは複数の静的起動イメージを使用する代わりに、特別な統合されたストーリーボードを作成して起動画面を提供できます。 IOS デザイナーで起動ストーリーボードを作成する場合は、[サイズクラス] と [自動レイアウト] を使用して、さまざまな表示環境に対して異なるレイアウトを定義します。 開発者は、サイズクラスと自動レイアウトを使用することによって、すべてのデバイスと表示環境に適した1つの起動画面を作成できます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac で、[ **ファイル > 新しいソリューション** を選択し、[ **単一ビューアプリ**] を選択して、新しいプロジェクトを作成します。 

    ![単一ビューアプリが選択された [新しいプロジェクト] ウィンドウ](launch-screens-images/launch01.png)

    - 既定では、新しいプロジェクトには、起動画面インターフェイスを定義する **launchscreen. storyboard** ファイルが含まれています。 
    - 代わりに、起動画面のストーリーボードを既存のプロジェクトに追加するには、 **Solution Pad** でプロジェクト名を右クリックし、[ **> 新しいファイルの追加...** ] を選択して [ **起動画面**] を選択します。

    ![[新しいファイル] ウィンドウが開き、iOS の起動画面が選択されました](launch-screens-images/launch01b.png)

    - ファイルの名前を **Launchscreen** にするか、別の名前を選択します。

2. 起動画面に適切なストーリーボードを使用するようにプロジェクトを構成します。

    - **Solution Pad**の情報の**plist**ファイルをダブルクリックして、編集用に開きます。
    - [ **起動イメージ** ] セクションで、 **起動画面** が適切なストーリーボードの名前に設定されていることを確認します。

    ![情報 plist の起動画面セレクター](launch-screens-images/launch02.png)

    - 既定では、新しいプロジェクトは起動画面として **Launchscreen. storyboard** を使用するように構成されています。

3. アセットにイメージを追加 **します。 xcassets** は、起動画面で使用できるようにアセットカタログを設定します。 詳細については、「[イメージの表示](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」ガイドの「[アセットカタログイメージセットへのイメージの追加](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」セクションを参照してください。

4. [ **Solution Pad**でダブルクリックして編集するには、 **launchscreen を開きます。**

5. IOS Designer で起動画面のストーリーボードをプレビューするデバイスと向きを選択します。 下部のツールバーにある [デバイスの選択] パネルを開き、[ **IPhone 4S** and **縦長**] を選択します。

    ![[デバイスの選択] ツールバー](launch-screens-images/launch05.png)

    - デバイスと向きを選択すると、iOS デザイナーによるデザインのプレビュー方法が変更されることに注意してください。 ここで選択した内容に関係なく、新たに追加された制約は、[ **特徴の編集** ] ボタンを使用して指定しない限り、すべてのデバイスと方向に適用されます。 

6. ビューコントローラーのメインビューの **背景** 色を設定します。 ビューコントローラーの中央をクリックしてビューを選択し、 **Properties Pad**を使用して背景色を調整します。

    ![背景色が紫の単一ビュー](launch-screens-images/launch06.png)

7. 起動画面に **イメージビュー** を追加し、ソース **イメージ**を設定します。

    - [**ツールボックス] パッド**からビューの中央に**画像ビュー**をドラッグします。
    - **イメージビュー**を選択した状態で、の [**ウィジェット**] セクションで、[**イメージ**] プロパティを、アセットに既に追加されているイメージセットに設定**Properties Pad** **ます。 xcassets** Asset Catalog。 必要に応じて **イメージビュー** の位置を変更し、サイズを変更します。
    
    ![イメージのプロパティが設定されたイメージビュー](launch-screens-images/launch07.png)

8. **イメージビュー**の下に**ラベル**を追加し、 **Properties Pad**を使用してその属性を設定します。 

    ![テキストとカラーセットを含むラベル](launch-screens-images/launch08.png)

9. [ **制約] ツールバー**の右側のボタンを使用して、制約編集モードに切り替えます。
    
    ![[制約編集モード] ボタン](launch-screens-images/launch09.png)

10. **イメージビュー**に制約を追加し、高さと幅を設定し、水平方向と垂直方向に中央揃えにします。

    ![レイアウトの制約があるイメージビュー](launch-screens-images/launch10.png)

    - 制約を追加する方法の詳細については、「 [Xamarin Designer for iOS を使用した自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)」を参照してください。

11. **ラベル**に制約を追加し、水平方向に中央揃えにして、高さと幅を指定し、**イメージビュー**から垂直方向に固定の距離を配置します。

    ![レイアウトの制約があるラベル](launch-screens-images/launch11.png)

12. 他のデバイスと向きをテストして、設計がすべてのシナリオで意図したとおりに見えることを確認します。 特定のデバイスまたは向きに対して調整を行う必要がある場合は、[ **特徴の編集** ] ボタンを使用して、特定のサイズクラスの制約を追加します。

    ![横向きの向きを使用して iPhone X としてレンダリングされた起動画面](launch-screens-images/launch12.png)

13. 変更内容をストーリーボードに保存します。 アプリをシミュレーターまたはデバイスで実行すると、アプリの起動中に起動画面が表示されます。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. 新しいプロジェクトを作成します。 Visual Studio で、[ **ファイル > 新しい > プロジェクト] を選択し > VISUAL C# > iPhone & iPad > IOS アプリ (Xamarin)** を選択します。

    ![IOS アプリ (Xamarin) が選択された [新しいプロジェクト] ウィンドウ](launch-screens-images/launch01.w157.png)

    [ **単一ビューアプリ** ] テンプレートを選択し、[ **OK]** をクリックします。

    ![単一ビューアプリテンプレート](launch-screens-images/launch01-2.w157.png)

2. **リソース > LaunchScreen. xib**が**ソリューションエクスプローラー**に存在する場合は、ファイルを右クリックし、[**削除**] を選択して削除します。 このファイルは、次の手順でストーリーボードに置き換えられます。

3. 起動画面として使用するストーリーボードを作成します。 **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[追加]、[**新しい項目の > 追加**]、[**空のストーリーボード**] の順に選択します。 このストーリーボードの名前を「 **Launchscreen. storyboard** 」と指定し、[ **追加**] をクリックします。

    ![空のストーリーボードが選択された [新しい項目の追加] ウィンドウ](launch-screens-images/launch03.w157.png)

4. 起動画面のストーリーボードとして **Launchscreen** を使用するようにプロジェクトを構成します。

    - **ソリューション エクスプローラー**で **Info.plist** ファイルをダブルクリックし、編集用に開きます。 
    - [ **ビジュアルアセット** ] タブで、 **起動画面** を **launchscreen**に設定します。

    ![情報 plist の起動画面セレクター](launch-screens-images/launch04-vs.png)

5. プロジェクトのアセットカタログにイメージを追加して、起動画面で使用できるようにします。

    - **ソリューションエクスプローラー**で、[**資産カタログ**] を右クリックし、[**資産カタログの追加**] を選択します。 この新しいアセットカタログ **資産**に名前を指定します。

    ![アセットカタログが選択された [新しい項目の追加] ウィンドウ](launch-screens-images/launch05.w157.png)

    - 「[イメージの表示](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」ガイドの「[アセットカタログイメージセットへのイメージの追加](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」セクションの説明に従って、新しいイメージセットを**Assets**資産カタログに追加します。

6. [**ソリューションエクスプローラー**でダブルクリックして編集するには、 **launchscreen を開きます。**

    - ストーリーボードファイルを編集するには、Visual Studio に Mac ビルドホストへのアクティブな接続が必要です。 詳細については、「 [Mac への接続](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 」ガイドを参照してください。

7. IOS Designer で起動画面のストーリーボードをプレビューするデバイスと向きを選択します。 下部のツールバーの [デバイスの選択] パネルを開き、[ **IPhone 4S** and **縦長**] を選択します。 

    ![[デバイスの選択] ツールバー](launch-screens-images/launch07-vs.png)

    - デバイスと向きを選択すると、iOS デザイナーによるデザインのプレビュー方法が変更されることに注意してください。 ここで選択した内容に関係なく、新たに追加された制約は、[ **特徴の編集** ] ボタンを使用して指定しない限り、すべてのデバイスと方向に適用されます。 

8. ビューコントローラーを [**ツールボックス**] からデザインサーフェイスにドラッグして、**ビューコントローラー**をストーリーボードに追加します。 

    ![デザインサーフェイスに追加された空のビューコントローラー](launch-screens-images/launch08-vs.png)

9. ビューコントローラーのメインビューの **背景** 色を設定します。 ビューコントローラーの中央をクリックしてビューを選択し、[ **プロパティ] ウィンドウ**を使用して背景色を調整します。
    
    ![背景色が紫の単一ビュー](launch-screens-images/launch09-vs.png)

10. 起動画面に **イメージビュー** を追加し、ソース **イメージ**を設定します。

    - **画像ビュー**を**ツールボックス**からビューの中央にドラッグします。
    - **イメージビュー**がまだ選択されている状態で、[**プロパティ] ウィンドウ**の [**ウィジェット**] セクションで、[**イメージ**] プロパティを [**アセット**資産カタログ] に既に追加されているイメージセットに設定します。 必要に応じて **イメージビュー** の位置を変更し、サイズを変更します。
    
    ![イメージのプロパティが設定されたイメージビュー](launch-screens-images/launch10-vs.png)

11. **画像ビュー**の下に**ラベル**を追加します。

    - **ツールボックス**からデザイン画面に**ラベル**をドラッグして、**イメージビュー**の下に配置します。
    - [**プロパティ] ウィンドウ**を使用して、**ラベル**の属性を設定します。

    ![テキストとカラーセットを含むラベル](launch-screens-images/launch11-vs.png) 

12. [ **制約] ツールバー**の右側のボタンを使用して、制約編集モードに切り替えます。
    
    ![[制約編集モード] ボタン](launch-screens-images/launch12-vs.png) 

13. **イメージビュー**に制約を追加し、高さと幅を設定し、水平方向と垂直方向に中央揃えにします。

    ![レイアウトの制約があるイメージビュー](launch-screens-images/launch13-vs.png) 

    - 制約を追加する方法の詳細については、「 [Xamarin Designer for iOS を使用した自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)」を参照してください。

14. **ラベル**に制約を追加し、水平方向に中央揃えにして、高さと幅を指定し、**イメージビュー**から垂直方向に固定の距離を配置します。
    
    ![レイアウトの制約があるラベル](launch-screens-images/launch14-vs.png) 

15. 他のデバイスと向きをテストして、設計がすべてのシナリオで意図したとおりに見えることを確認します。 特定のデバイスまたは向きに対して調整を行う必要がある場合は、[ **特徴の編集** ] ボタンを使用して、特定のサイズクラスの制約を追加します。

    ![横向きの向きを使用して iPhone X としてレンダリングされた起動画面](launch-screens-images/launch15-vs.png) 

16. 変更内容をストーリーボードに保存します。 アプリをシミュレーターまたはデバイスで実行すると、アプリの起動中に起動画面が表示されます。

-----

> [!NOTE]
> 起動画面として使用されるストーリーボードには、単純な組み込み UI 要素のみが含まれて _いる必要があり_ 、計算を実行したり、カスタムクラスから派生したりする **ことはできません** 。

統合されたストーリーボードを使用して起動画面を作成する方法の詳細については、統合された[ストーリー](~/ios/user-interface/storyboards/unified-storyboards.md)ボードガイドの[動的起動画面](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens)に関するセクションを参照してください。

## <a name="migrating-to-launch-screen-storyboards"></a>起動画面のストーリーボードへの移行

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

既存のアプリを更新してその起動画面にストーリーボードを使用する場合は、**ソリューションエクスプローラー**で**プロジェクト名**を右クリックし、[ **Add**  >  **新しいファイル**の追加] を選択します。[ **IOS**  >  の**起動画面**] を選択し、[**新規**] ボタンをクリックします。

![IOS の起動画面を選択する](launch-screens-images/storyboard02.png)

次に、ソリューションエクスプローラー内のファイルをダブルクリックし `Info.plist` て、編集用に**Solution Explorer**開きます。 [ **起動画面**] で、上で作成した新しいストーリーボードファイルを選択します。

![上で作成した新しいストーリーボードファイルを選択します](launch-screens-images/storyboard09.png)

新しいストーリーボードを起動画面として使用するには、次の手順を実行します。

1. ソリューションエクスプローラー内のファイルをダブルクリックし `Info.plist` て、編集用に**Solution Explorer**開きます。
2. エディターの [ **ユニバーサル起動イメージ** ] セクションまでスクロールし、[ **起動画面]** ドロップダウンを開いて、上で作成したストーリーボードの名前を選択します。 

    ![ストーリーボードへの起動画面の設定](launch-screens-images/storyboard08.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**でプロジェクト名を右クリックし、[ **Add**  >  **新しいファイル**の追加] を選択します。 

    ![新しいファイルの追加](launch-screens-images/image012.png)
2. 起動画面の名前を入力し、[ **追加** ] ボタンをクリックします。 

    ![起動画面の名前を入力してください](launch-screens-images/image013.png)
3. **ソリューションエクスプローラー**で、新しく作成したストーリーボードファイルをダブルクリックして開き、編集します。
4. **Size クラス**が Any と**表示****され**ていることを確認します **。** 

    ![Size クラスが any に設定されていることを確認します。](launch-screens-images/image016.png)
5. 起動画面は、サイズクラス、単純な UI 要素 (など `UIImageView` )、およびアプリケーションのバンドルに含まれているイメージからアセンブリします。 

    ![IOS デザイナーで起動画面をアセンブリする](launch-screens-images/image017.png)
6. 変更内容をストーリーボードに保存します。

-----

## <a name="related-links"></a>関連リンク

- [動的な起動画面 (サンプル)](/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen)
- [統合ストーリーボード](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS Designer の基本](~/ios/user-interface/designer/index.md)
- [アセットカタログイメージセットへのイメージの追加](~/ios/app-fundamentals/images-icons/displaying-an-image.md#adding-images-to-an-asset-catalog-image-set)
- [Xamarin Designer for iOS を使用した自動レイアウト](~/ios/user-interface/designer/designer-auto-layout.md)
- [ヒューマンインターフェイスのガイドライン: 起動画面](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)