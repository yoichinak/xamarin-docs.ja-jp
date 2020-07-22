---
ms.topic: include
author: jamesmontemagno
ms.author: jamont
ms.date: 05/11/2020
ms.openlocfilehash: b5396061de657690fa3f4cdf68e8a94382918613
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149729"
---
この API では、Android の実行時のアクセス許可を使用します。 Xamarin. Essentials が完全に初期化されており、アクセス許可の処理がアプリで設定されていることを確認してください。

Android プロジェクトの `MainLauncher`、または起動されるすべての `Activity` では、`OnCreate` メソッド内で Xamarin.Essentials を初期化する必要があります。

```csharp
protected override void OnCreate(Bundle savedInstanceState) 
{
    //...
    base.OnCreate(savedInstanceState);
    Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code, it may also be called: bundle
    //...
}    
```

Android 上で実行時のアクセス許可を処理するには、Xamarin.Essentials がすべての `OnRequestPermissionsResult` を受け取る必要があります。 すべての `Activity` クラスに次のコードを追加します。

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Android.Content.PM.Permission[] grantResults)
{
    Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

    base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
}
```
