---
title: IOS デザイナーでカスタム コントロールの使用
description: このドキュメントでは、カスタム コントロールを作成し、iOS 用の Xamarin デザイナーで使用する方法について説明します。 コントロールを iOS デザイナーのツールボックスで使用できるように、正しく表示されるようにコントロールを実装および時刻、および詳細を設計する方法を示します。
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: dae675d65cb2be93ac828a1aebe560354630ab54
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790166"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>IOS デザイナーでカスタム コントロールの使用

## <a name="requirements"></a>必要条件

IOS 用 Xamarin デザイナーは、Windows 上の Mac と Visual Studio 2015 と 2017 の Visual Studio で使用します。

このガイドで説明した内容に関する知識を前提と、[概要ガイド](~/ios/get-started/index.md)です。

## <a name="walkthrough"></a>チュートリアル

> [!IMPORTANT]
> Xamarin.Studio 5.5 以降、カスタム コントロールを作成する方法は以前のバージョンには若干異なります。 か、カスタム コントロールを作成する、`IComponent`インターフェイスが (、関連付けられた実装手段) またはできるクラスで注釈を付ける`[DesignTimeVisible(true)]`です。 後者の方法は、次のチュートリアルの例で使用されています。


1. 新しいソリューションを作成、 **iOS > アプリ > ビューの 1 つのアプリケーション > c#** テンプレート、という名前を付けます`ScratchTicket`、し、新しいプロジェクト ウィザードを続行します。

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


1. 追加、 `FillTexture.png`、`FillTexture2.png`と`Monkey.png`ファイル (使用可能な[GitHub から](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) に、**リソース**フォルダーです。
    
1. ダブルクリックして、`Main.storyboard`デザイナーで開くファイル。

    [![](ios-designable-controls-walkthrough-images/03new.png "IOS デザイナー")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. ドラッグ アンド ドロップ、**イメージ ビュー**から、**ツールボックス**ストーリー ボードの表示にします。

    [![](ios-designable-controls-walkthrough-images/04new.png "レイアウトに追加のイメージ ビュー")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. 選択、**イメージ ビュー**変更とその**イメージ**プロパティを`Monkey.png`です。

    [!(ios-デザインのコントロールのチュートリアル-イメージ/05new.png"イメージの表示設定イメージ プロパティ Monkey.png を)](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. サイズのクラスを使用して、このイメージの表示を制限する必要になります。 制約のモードにするには、2 回イメージをクリックします。 Center 固定ハンドルをクリックして、中央に制限して、垂直および水平方向の両方を配置してみましょう。

    [![](ios-designable-controls-walkthrough-images/06new.png "画像を中央揃え")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. 高さと幅を制限、サイズ固定ハンドル ('骨' 形のハンドル) をクリックしを幅と高さをそれぞれ選択します。

    [![](ios-designable-controls-walkthrough-images/07new.png "制約の追加")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. ツールバーの [更新] ボタンをクリックして、制約に基づくフレームを更新します。

    [![](ios-designable-controls-walkthrough-images/08new.png "制約のツールバー")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. 次に、プロジェクトをビルドできるように、**チケット ビューのスクラッチ**下に表示されます**カスタム コンポーネント**ツールボックスで。

    [![](ios-designable-controls-walkthrough-images/09new.png "カスタム コンポーネントのツールボックス")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. ドラッグ アンド ドロップ、**チケット ビューのスクラッチ**サル画像の上に表示されるようにします。 スクラッチ チケット ビュー、サルを次のように、完全にカバーするために、ドラッグ ハンドルを調整します。

    [![](ios-designable-controls-walkthrough-images/10new.png "全体のイメージ表示スクラッチ チケット ビュー")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. スクラッチ チケット ビューに、画像表示を制限するには、両方のビューを選択する外接する四角形を描画します。 次に示すように制約に基づいて幅、高さ、中央および中央および更新プログラムのフレームに制限するオプションを選択します。

    [![](ios-designable-controls-walkthrough-images/11new.png "中央に配置し、制約を追加します。")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. アプリケーションを実行し、サルを表示するイメージをオフ"scratch"です。

    [![](ios-designable-controls-walkthrough-images/10-app.png "実行のサンプル アプリ")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>デザイン時のプロパティを追加します。

デザイナーには、プロパティの数値型、列挙型、文字列、bool、CGSize、UIColor、および UIImage のカスタム コントロールのデザイン時サポートも含まれています。 をデモンストレーションするためにプロパティを追加してみましょう。、 `ScratchTicketView` 「傷オフ。」イメージを設定するには

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

Null チェックを追加したいも、`Draw`メソッドを次のようにします。

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

含む、`ExportAttribute`と`BrowsableAttribute`に設定する引数を持つ`true`デザイナーに表示されているプロパティで結果**プロパティ**パネルです。 プロパティを変更するなど、プロジェクトに含まれている別のイメージを`FillTexture2.png`、次に示すように、コントロールを更新中に、デザイン時に結果します。

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "デザイン時プロパティの編集")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>まとめ

この記事の内容おとおしをカスタム コントロールを作成できるだけでなく、iOS デザイナーを使用して iOS アプリケーションで使用する方法です。 作成し、デザイナーでのアプリケーションに使用できるようにするコントロールを構築する方法を説明しました**ツールボックス**です。 さらに、デザイン時と、実行時の両方で正しく表示されるようにコントロールを実装する方法と、デザイナーでカスタム コントロールのプロパティを公開する方法を説明しました。



## <a name="related-links"></a>関連リンク

- [ScratchTicket (サンプル)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [必要なイメージ (サンプル)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
