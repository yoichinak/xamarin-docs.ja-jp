---
ms.openlocfilehash: b39d337c80a50fb6a3899b609eac1e73ed9bcb6d
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98690163"
---
この演習では、データ アクセス クラスを **LocalDatabaseTutorial** プロジェクト追加します。これは、人に関するデータをデータベースに保持するために使用されます。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー** の **[LocalDatabaseTutorial]** プロジェクトで、`Person` という名前の新しいクラスをプロジェクトに追加します。 次に、**Person.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    このコードは、各人に関するデータをアプリケーションに格納する `Person` クラスを定義します。 データベース内の各 `Person` インスタンスが SQLite.NET によって付与される一意の ID 持つようにするため、`ID` プロパティが `PrimaryKey` と `AutoIncrement` の属性によってマークされます。

1. **ソリューション エクスプローラー** の **[LocalDatabaseTutorial]** プロジェクトで、`Database` という名前の新しいクラスをプロジェクトに追加します。 次に、**Database.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    このクラスには、データを読み書きするデータベースを作成するためのコードが含まれます。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`Database` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスは次の演習で `App` クラスによって提供されます。

1. **ソリューション エクスプローラー** の **[LocalDatabaseTutorial]** プロジェクトで、 **[App.xaml]** を展開し、 **[App.xaml.cs]** をダブルクリックして開きます。 次に、**App.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    このコードは、新しい `Database` インスタンスをシングルトンとして作成する `Database` プロパティを定義します。 データベースを格納する場所を表すローカル ファイル パスとファイル名は、引数として `Database` クラス コンストラクターに渡されます。

    > [!IMPORTANT]
    > データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    シングルトン デザイン パターンの詳細については、「[デザイン パターン: シングルトン](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Design-Patterns-Singleton)」を参照してください。    

1. エラーがないようにソリューションを構築してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad** の **[LocalDatabaseTutorial]** プロジェクトで、`Person` という名前の新しいクラスをプロジェクトに追加します。 次に、**Person.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    このコードは、各人に関するデータをアプリケーションに格納する `Person` クラスを定義します。 データベース内の各 `Person` インスタンスが SQLite.NET によって付与される一意の ID 持つようにするため、`ID` プロパティが `PrimaryKey` と `AutoIncrement` の属性によってマークされます。

1. **Solution Pad** の **[LocalDatabaseTutorial]** プロジェクトで、`Database` という名前の新しいクラスをプロジェクトに追加します。 次に、**Database.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    このクラスには、データを読み書きするデータベースを作成するためのコードが含まれます。 このコードでは、データベース操作をバックグラウンドのスレッドに移動する非同期 SQLite.Net API が使用されています。 さらに、`Database` コンストラクターがデータベース ファイルのパスを引数として受け取ります。 このパスは次の演習で `App` クラスによって提供されます。

1. **Solution Pad** の **[LocalDatabaseTutorial]** プロジェクトで、**[App.xaml]** を展開し、**[App.xaml.cs]** をダブルクリックして開きます。 次に、**App.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    このコードは、新しい `Database` インスタンスをシングルトンとして作成する `Database` プロパティを定義します。 データベースを格納する場所を表すローカル ファイル パスとファイル名は、引数として `Database` クラス コンストラクターに渡されます。

    > [!IMPORTANT]
    > データベースをシングルトンとして公開することの利点は、アプリケーションが実行されている間、開いたままになる単一のデータベース接続が作成されることです。そのため、データベース操作が実行されるときにデータベース ファイルを毎回開いて閉じる労力を避けることができます。

    シングルトン デザイン パターンの詳細については、「[デザイン パターン: シングルトン](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Design-Patterns-Singleton)」を参照してください。

1. エラーがないようにソリューションを構築してください。
