---
title: 指紋認証
description: このガイドでは、Xamarin.Android アプリケーションを Android 6.0 で導入された、指紋認証を追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 70e12abdf61a6a0bfb36d281bcaa6214199e567d
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667335"
---
# <a name="fingerprint-authentication"></a>指紋認証

_このガイドでは、Xamarin.Android アプリケーションを Android 6.0 で導入された、指紋認証を追加する方法について説明します。_


## <a name="fingerprint-authentication-overview"></a>指紋認証の概要

Android デバイスで指紋スキャナーの到着は、ユーザー認証の代わりに、従来のユーザー名/パスワード メソッドにアプリケーションを提供します。 ユーザーを認証する指紋の使用によって、ユーザー名とパスワードよりも煩雑セキュリティを組み込むアプリケーションになります。

FingerprintManager Api 指紋スキャナーでデバイスを対象にして、API レベル 23 (Android 6.0) を実行しているまたはそれ以降。 Api がある、`Android.Hardware.Fingerprints`名前空間。 Android サポート ライブラリ v4 は、指紋の古いバージョンの Android 用の Api のバージョンを提供します。 互換性 Api にあるは、`Android.Support.v4.Hardware.Fingerprint`名前空間内での配布、 [Xamarin.Android.Support.v4 NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)。

[FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (とそのサポート ライブラリの対応する[FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) のハードウェア スキャン、指紋を使用して、主要なクラスです。 このクラスは、ハードウェア自体との対話を管理するシステム レベルのサービスを Android SDK でラッパーです。 スキャナーからのフィードバックに応答したり、指紋スキャナーを起動するためになります。 このクラスには 3 つのみのメンバーで非常に簡単ですインターフェイス。

* **`Authenticate`** &ndash; このメソッドは、ハードウェア スキャナーを初期化し、ユーザーが、指紋をスキャンするために待機しているバック グラウンドでサービスを開始します。
* **`EnrolledFingerprints`** &ndash; このプロパティは返します`true`ユーザーが 1 つまたは複数の指紋をデバイスに登録されている場合。
* **`HardwareDetected`** &ndash; このプロパティは、デバイスが指紋をスキャンをサポートしているかどうかに使用されます。

`FingerprintManager.Authenticate`メソッドは、指紋スキャナーを開始する Android アプリケーションによって使用されます。 次のスニペットでは、サポート ライブラリの互換性の Api を使用して呼び出す方法の例を示します。

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

このガイドは、使用する方法を説明する、`FingerprintManager`指紋認証を使用した Android アプリケーションを強化する Api。 インスタンスを作成し、作成する方法を取り上げますが、`CryptoObject`指紋スキャナーからの結果を保護するためにします。 サブクラスをアプリケーションがどのように検証`FingerprintManager.AuthenticationCallback`指紋スキャナーからのフィードバックに応答しているとします。 最後に、使用する方法と、Android デバイスまたはエミュレーターにフィンガー プリントを登録する方法が表示されます**adb**指紋のスキャンをシミュレートします。

## <a name="requirements"></a>必要条件

指紋認証には、Android 6.0 (API レベル 23) が必要となるまたは以上および指紋スキャナーでデバイス。 

指紋が既に認証されている各ユーザーのデバイスに登録されている必要があります。 パスワード、PIN、スワイプのパターンまたは顔認識を使用して画面のロックを設定します。 Android のエミュレーターで指紋認証機能の一部をシミュレートすることになります。  これら 2 つのトピックの詳細についてを参照してください、[指紋の登録](enrolling-fingerprint.md)セクション。 






## <a name="related-links"></a>関連リンク

- [指紋のガイド サンプル アプリ](https://developer.xamarin.com/samples/monodroid/FingerprintGuide/)
- [指紋ダイアログのサンプル](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [実行時にアクセス許可の要求](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [指紋と支払いの API (ビデオ)](https://youtu.be/VOn7VrTRlA4)
