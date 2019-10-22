---
ms.openlocfilehash: d46968f9c53314abe561e7f4871cfbf6e07b7002
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "61047858"
---
## <a name="firewall-configuration"></a>ファイアウォールの構成

テストを Xamarin Test Cloud に送信するためには、テストを送信するコンピューターが Test Cloud サーバーと通信できる必要があります。 ファイアウォールは、ポート80および443の**testcloud.xamarin.com**にあるサーバーとの間のネットワークトラフィックを許可するように構成する必要があります。 このエンドポイントは DNS によって管理され、IP アドレスは変更される可能性があります。 

場合によっては、テスト (またはテストを実行しているデバイス) が、ファイアウォールによって保護されている web サーバーと通信する必要があります。 このシナリオでは、次の IP アドレスからのトラフィックを許可するようにファイアウォールを構成する必要があります。

* **195.249.159.238**
* **195.249.159.239**
