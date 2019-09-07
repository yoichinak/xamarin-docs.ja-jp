---
title: iOS Designer でのカスタム コントロールの使用
description: このドキュメントでは、カスタムコントロールを作成し、Xamarin Designer for iOS と共に使用する方法について説明します。 IOS デザイナーのツールボックスでコントロールを使用できるようにする方法、コントロールを実装して、正しくレンダリングされるようにする方法などについて説明します。
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: 651df247e3b5616a3baa38e85159c6a6d5247807
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768803"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>iOS Designer でのカスタム コントロールの使用

## <a name="requirements"></a>必要条件

Xamarin Designer for iOS は Visual Studio for Mac と Visual Studio 2017 以降の Windows で使用できます。

このガイドでは、[はじめにガイド](~/ios/get-started/index.md)で説明されている内容を理解していることを前提としています。

## <a name="walkthrough"></a>チュートリアル

> [!IMPORTANT]
> Xamarin. Studio 5.5 以降では、カスタムコントロールの作成方法は、以前のバージョンと少し異なります。 カスタムコントロール`IComponent`を作成するには、インターフェイスが (関連付けられている実装メソッドを使用して) 必要で`[DesignTimeVisible(true)]`あるか、またはクラスに注釈を付けることができます。 後者の方法は、次のチュートリアルの例で使用されています。

1. IOS > アプリから新しいソリューションを作成し`ScratchTicket`、[**シングルビューアプリケーションC# ] > テンプレート >** して、名前を指定し、新しいプロジェクトウィザードを続行します。

    [![](ios-designable-controls-walkthrough-images/01new.png "新しいソリューションを作成する")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. という名前`ScratchTicketView`の新しい空のクラスファイルを作成します。

    [![](ios-designable-controls-walkthrough-images/02new.png "新しい ScratchTicketView クラスを作成する")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. クラスの`ScratchTicketView`次のコードを追加します。

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

1. `FillTexture.png`、`FillTexture2.png`、および`Monkey.png` ([GitHub から](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)入手できる) ファイルを **Resources** フォルダーに追加します。

1. `Main.storyboard`ファイルをダブルクリックして、デザイナーで開きます。

    [![](ios-designable-controls-walkthrough-images/03new.png "IOS デザイナー")](ios-designable-controls-walkthrough-images/03new.png#lightbox)

1. **[ツールボックス]** から**イメージビュー**をストーリーボードのビューにドラッグアンドドロップします。

    [![](ios-designable-controls-walkthrough-images/04new.png "レイアウトに追加されたイメージビュー")](ios-designable-controls-walkthrough-images/04new.png#lightbox)

1. **イメージビュー**を選択し、その**イメージ**プロパティを`Monkey.png`に変更します。

    [![](ios-designable-controls-walkthrough-images/05new.png "画像表示イメージのプロパティを「サル .png」に設定します。")](ios-designable-controls-walkthrough-images/05new.png#lightbox)

1. サイズクラスを使用しているため、このイメージビューを制限する必要があります。 イメージを2回クリックして、制約モードにします。 中央にピン留めするハンドルをクリックし、垂直方向と水平方向に並べて配置します。

    [![](ios-designable-controls-walkthrough-images/06new.png "イメージの中央揃え")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. 高さと幅を制限するには、サイズ固定ハンドル ("ボーン" の形のハンドル) をクリックし、[幅] と [高さ] をそれぞれ選択します。

    [![](ios-designable-controls-walkthrough-images/07new.png "制約の追加")](ios-designable-controls-walkthrough-images/07new.png#lightbox)

1. ツールバーの [更新] ボタンをクリックして、制約に基づいてフレームを更新します。

    [![](ios-designable-controls-walkthrough-images/08new.png "[制約] ツールバー")](ios-designable-controls-walkthrough-images/08new.png#lightbox)

1. 次に、プロジェクトをビルドして、ツールボックス の **カスタムコンポーネント** の下に **スクラッチチケット ビュー**が表示されるようにします。

    [![](ios-designable-controls-walkthrough-images/09new.png "カスタムコンポーネントツールボックス")](ios-designable-controls-walkthrough-images/09new.png#lightbox)

1. **スクラッチチケットビュー**をドラッグアンドドロップして、サル画像の上に表示されるようにします。 次に示すように、スクラッチチケットビューがサルを完全にカバーするように、ドラッグハンドルを調整します。

    [![](ios-designable-controls-walkthrough-images/10new.png "イメージビューに対するスクラッチチケットビュー")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. 両方のビューを選択するには、外接する四角形を描画して、スクラッチチケットビューをイメージビューに制限します。 次に示すように、制約に基づいて、幅、高さ、中心、中央に制限するオプションを選択し、制約に基づいてフレームを更新します。

    [![](ios-designable-controls-walkthrough-images/11new.png "制約の中心と追加")](ios-designable-controls-walkthrough-images/11new.png#lightbox)

1. アプリケーションを実行し、イメージを "ゼロオフ" にして、サルを表示します。

    [![](ios-designable-controls-walkthrough-images/10-app.png "サンプルアプリの実行")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>デザイン時プロパティの追加

このデザイナーには、numeric、enumeration、string、bool、CGSize、UIColor、および UIImage の各プロパティ型のカスタムコントロールのデザイン時サポートも含まれています。 例として、に`ScratchTicketView`プロパティを追加して、"傷のあるイメージ" を設定してみましょう。

プロパティの`ScratchTicketView`クラスに次のコードを追加します。

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

次のように、 `Draw`メソッドに null チェックを追加することもできます。

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

引数をに`BrowsableAttribute` 設定`true`したおよびを含めると、デザイナーの [プロパティ] パネルにプロパティが表示されます`ExportAttribute` 。 次に示すように、プロパティをプロジェクトに含まれる別`FillTexture2.png`のイメージ (など) に変更すると、デザイン時にコントロールが更新されます。

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "デザイン時のプロパティの編集")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>まとめ

この記事では、iOS デザイナーを使用して、カスタムコントロールを作成し、iOS アプリケーションで使用する方法について説明します。 デザイナーの**ツールボックス**で、コントロールを作成してビルドし、アプリケーションで使用できるようにする方法を説明しました。 また、デザイン時と実行時の両方で適切にレンダリングされるようにコントロールを実装する方法についても説明しました。また、デザイナーでカスタムコントロールのプロパティを公開する方法についても説明しました。

## <a name="related-links"></a>関連リンク

- [ScratchTicket (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/scratchticket)
- [必要なイメージ (サンプル)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
