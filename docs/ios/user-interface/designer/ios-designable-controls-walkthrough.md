---
title: IOS Designer でカスタム コントロールの使用
description: このドキュメントでは、カスタム コントロールを作成し、iOS 用の Xamarin のデザイナーを使用する方法について説明します。 コントロールを iOS デザイナーのツールボックスで使用できるように、正しくレンダリングされるようにコントロールを実装および時刻、および詳細を設計する方法を示します。
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 0401c2c05677c719bbe4914cc7e008b650fdd198
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526242"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>IOS Designer でカスタム コントロールの使用

## <a name="requirements"></a>必要条件

IOS 用の Xamarin デザイナーは、Windows の場合は、Visual Studio for Mac と Visual Studio 2015 と 2017 の使用。

このガイドで説明した内容を熟知することを前提としています、[ガイド ファースト](~/ios/get-started/index.md)します。

## <a name="walkthrough"></a>チュートリアル

> [!IMPORTANT]
> Xamarin.Studio 5.5 以降、カスタム コントロールを作成する方法は、以前のバージョンと若干異なります。 か、カスタム コントロールを作成する、`IComponent`インターフェイスは、(関連する実装メソッド) を使用して必要またはクラスの注釈として付ける`[DesignTimeVisible(true)]`します。 後者の方法は、チュートリアルの次の例で使用されています。


1. 新しいソリューションを作成、 **iOS > アプリ > 単一ビュー アプリケーション > c#** テンプレート、という名前を付けます`ScratchTicket`、し、新しいプロジェクト ウィザードを続行します。

    [![](ios-designable-controls-walkthrough-images/01new.png "新しいソリューションを作成します。")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. という名前の新しい空のクラス ファイルを作成する`ScratchTicketView`:

    [![](ios-designable-controls-walkthrough-images/02new.png "新しい ScratchTicketView クラスを作成します。")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. 次のコードを追加`ScratchTicketView`クラス。

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;
    
            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }
    
            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }
    
            public ScratchTicketView()
            {
                Initialize();
            }
    
            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }
    
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }
    
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }
    
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }
    
            public override void Draw(CGRect rect)
            {
                base.Draw(rect);
    
                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);
    
                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();
    
                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }
    
                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);        
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```


1. 追加、 `FillTexture.png`、`FillTexture2.png`と`Monkey.png`ファイル (使用可能な[GitHub から](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) に、**リソース**フォルダー。
    
1. ダブルクリックして、`Main.storyboard`ファイルをデザイナーで開きます。

    [![](ios-designable-controls-walkthrough-images/03new.png "IOS Designer")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. ドラッグ アンド ドロップ、 **Image View**から、**ツールボックス**ストーリー ボードの表示にします。

    [![](ios-designable-controls-walkthrough-images/04new.png "イメージのビューがレイアウトに追加")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. 選択、 **Image View**変更とその**イメージ**プロパティを`Monkey.png`します。

    [![](ios-designable-controls-walkthrough-images/05new.png "Monkey.png にイメージのイメージの表示プロパティの設定")](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. サイズ クラスを使用しているようにこのイメージの表示を制限する必要があります。 制約モードにするには、2 回イメージをクリックします。 Center 固定ハンドルをクリックして、中央に制限して、垂直方向および水平方向に配置しましょう。

    [![](ios-designable-controls-walkthrough-images/06new.png "画像を中央揃え")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. 高さと幅を制限するには、サイズを固定ハンドル ('骨' 形のハンドル) をクリックしを幅と高さをそれぞれ選択します。

    [![](ios-designable-controls-walkthrough-images/07new.png "制約の追加")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. ツールバーの [更新] ボタンをクリックして、制約に基づくフレームを更新します。

    [![](ios-designable-controls-walkthrough-images/08new.png "制約ツールバー")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. 次に、プロジェクトをビルドできるように、**チケット ビューのスクラッチ**下に表示されます**カスタム コンポーネント**ツールボックスに。

    [![](ios-designable-controls-walkthrough-images/09new.png "カスタム コンポーネントのツールボックス")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. ドラッグ アンド ドロップ、**チケット ビューのスクラッチ**monkey 画像の上に表示されるようにします。 スクラッチ チケット ビューが完全に、次に示すよう、monkey をについて説明するため、ドラッグ ハンドルを調整します。

    [![](ios-designable-controls-walkthrough-images/10new.png "イメージ ビュー上のスクラッチ チケット ビュー")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. イメージの表示にスクラッチ チケット ビューを制限するには、両方のビューを選択する外接する四角形を描画します。 次に示すように制約に基づいて幅、高さ、Center、中間および更新プログラムのフレームに制限するオプションを選択します。

    [![](ios-designable-controls-walkthrough-images/11new.png "中央揃えと制約の追加")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. アプリケーションを実行し、monkey を表示するイメージをオフ"scratch"。

    [![](ios-designable-controls-walkthrough-images/10-app.png "サンプル アプリの実行")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>デザイン時プロパティを追加します。

デザイナーには、プロパティの数値型、列挙、文字列、bool、CGSize、示す UIColor、および UIImage のカスタム コントロールのデザイン時サポートも含まれています。 例として、プロパティを追加、 `ScratchTicketView` 「傷オフ」イメージを設定するには。

次のコードを追加、`ScratchTicketView`クラスのプロパティ。

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image 
{
    get { return image; }
    set { 
            image = value;
            SetNeedsDisplay ();
        }
}
```

Null チェックを追加したいことも、`Draw`メソッドでは、次のようにします。

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);
            
        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);       
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

など、`ExportAttribute`と`BrowsableAttribute`に設定する引数を持つ`true`デザイナーに表示されているプロパティで結果**プロパティ**パネル。 プロパティを変更するなど、プロジェクトに含まれているもう 1 つのイメージに`FillTexture2.png`、次に示すように、デザイン時にコントロールの更新中に結果します。

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "デザイン時プロパティの編集")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>まとめ

この記事には、iOS デザイナーを使用して iOS アプリケーションで使用するほか、カスタム コントロールを作成する方法を説明しました。 作成し、デザイナーのアプリケーションに使用できるようにするコントロールを構築する方法を説明しました**ツールボックス**します。 さらに、デザイン時およびランタイムの両方が正しく表示されるように、コントロールを実装する方法と、デザイナーでカスタム コントロールのプロパティを公開する方法を説明しました。



## <a name="related-links"></a>関連リンク

- [ScratchTicket (サンプル)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [必要なイメージ (サンプル)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
