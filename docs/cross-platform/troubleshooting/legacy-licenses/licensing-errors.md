---
title: "いくつか特定のライセンスのエラー"
ms.topic: article
ms.prod: xamarin
ms.assetid: D26BDF2D-923B-4BC1-841A-74583155DB71
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 80006ce594db5baef5d295537f198181fe0b0fe1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="some-specific-licensing-errors"></a>いくつか特定のライセンスのエラー

> [!IMPORTANT]
> レガシ Xamarin のライセンスに固有の問題であるために、以下の情報は、MSDN のユーザーには適用されません。 表示される場合は、MSDN ユーザーもののようなエラー以下をお試しください[Xamarin の最新バージョンに更新](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)(& a) これを参照してください[ライセンス オプションのガイド](~/cross-platform/get-started/requirements.md)です。



## <a name="invalid-license"></a>「無効なライセンス」

> 無効なライセンスです。 Xamarin.Android (XA9999) ライセンス版を特定できませんを再アクティブ化してください。 (XA9010)

これらのメッセージは、いくつかの異なる状況で表示できます。

-   ディスク上の現在のライセンスで同期失われる可能性があります、コンピューター上の現在のユーザー情報とします。 [ライセンス ファイルを更新する](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)多くの場合は、この問題を解決します。

-   コンピューターには、競合している 2 つの重複するライセンス認証があります。 このタイプのライセンスは、Xamarin では使用されません。 表示してください。 このエラーに関連する問題が発生した場合[電子メール サポート](https://www.xamarin.com/support)です。

-   Xamarin アカウントがまだ招待できません、Xamarin ライセンスを。 関連項目:[チームのライセンス管理](~/cross-platform/troubleshooting/legacy-licenses/team-management.md)です。

## <a name="failed-to-load-android-entitlements"></a>「Android 権利の読み込みに失敗しました」

> Mono.VisualStudio.Shell.ShellPackage Error: 0 : Failed to load Android entitlements: Invalid license. Xamarin.Android (XA9999) Mono.VisualStudio.Shell.ShellPackage エラーを再アクティブ化してください: 0: ライセンスの更新に失敗しました: 無効なライセンスです。 Xamarin.Android (XA9999) を再アクティブ化してください。

これは、上から「無効なライセンス」の問題の古いバージョンです。 同じトラブルシューティングの手順が適用されます。

## <a name="this-version-was-released-after-your-subscription-expired"></a>「このバージョンは、お客様のサブスクリプションが期限切れの後にリリースされました」

> エラー XA9000: このバージョンは、サブスクリプションが期限切れの後にリリースされました (2014 年 11 月 11 日午後 5時 11分: 41)。 (XA9000)(Mobile.Android.Utilities) エラー XA9010: ライセンス版を決定できません。 (XA9010)(Mobile.Android.Utilities)

これらのメッセージは、2 つの状況によく表示されます。

-   ディスク上のライセンスは期限切れです。 [ライセンス ファイルを更新する](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)多くの場合は、この問題を解決します。

-   Xamarin サブスクリプションがアクティブでなくなったと、Xamarin の場合は、現在インストールされているバージョンは、サブスクリプションの有効期限よりも新しいです。 修正プログラムが、ここでは[ダウン グレード](http://kb.xamarin.com/customer/portal/articles/1699777)サブスクリプションが期限切れ前にリリースされた Xamarin のバージョンにします。 電子メールを送信する自由[ hello@xamarin.com ](mailto:hello@xamarin.com)をお客様のサブスクリプションの有効な最新のバージョンを要求します。 ダウン グレードした後もエラーが参照してください場合は、すぎるライセンス ファイルを更新してみてを必ずです。

## <a name="additional-references"></a>その他のリファレンス

-   [ライセンスを更新する方法](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)
-   [Xamarin をダウン グレードする方法](http://kb.xamarin.com/customer/portal/articles/1699777-downgrading)
-   [Xamarin.Android エラー コードの一覧](~/android/troubleshooting/errors.md)
-   [MonoTouch (Xamarin.iOS) エラー コードの一覧](~/ios/troubleshooting/mtouch-errors.md)

### <a name="next-steps"></a>次の手順
詳細については、問い合わせ先、または、上記の情報を使用した後もこの問題が残っている場合を参照してください[どのようなサポート オプションは、Xamarin を使用しますか?](~/cross-platform/troubleshooting/support-options.md)推奨事項は、の連絡先のオプションについて方法だけでなく必要な場合は、新しいバグをファイルです。
