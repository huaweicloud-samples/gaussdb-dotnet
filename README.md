# GaussDB Driver

[![HuaweiCloud.GaussDB.Driver Latest Preview](https://img.shields.io/nuget/vpre/HuaweiCloud.GaussDB.Driver)](https://www.nuget.org/packages/HuaweiCloud.GaussDB.Driver/absoluteLatest)

[![PostgreSQL License](https://img.shields.io/badge/License-PostgreSQL-blue.svg)](https://opensource.org/licenses/PostgreSQL)

## What is GaussDB ?

GuassDB is an open-source database driver led by Huawei's open-source community and refactored based on npgsql. It is compatible with openGauss and Guass databases. [Open Source for Huawei](https://developer.huaweicloud.com/programs/opensource/contributing/) revolves around Huawei's technology ecosystems including Kunpeng, Ascend, HarmonyOS, and Huawei Cloud. Through collaboration with developers from enterprises, universities, and the open-source community, it enables adaptation development and solution validation for open-source software, ensuring smoother and more efficient operation on Huawei Cloud.  

Before getting started, developers can download the [Open Source for Huawei Wiki](https://gitcode.com/HuaweiCloudDeveloper/OpenSourceForHuaweiWiki) to access detailed development procedures, technical preparations, and various resources required throughout the development process. If you have any questions during use, please visit the [GaussDB forum](https://bbs.huaweicloud.com/forum/forum-1350-1.html) for discussion.

## Package naming

Use `HuaweiCloud.GaussDB.Driver` for the main ADO.NET driver package. The NuGet package IDs use the `HuaweiCloud.GaussDB.*` prefix, while the .NET namespaces and public APIs remain unchanged. Existing code such as `using HuaweiCloud.GaussDB;`, `GaussDBConnection`, and `GaussDBDataSourceBuilder` does not need to change.

If you used the previous package IDs, update `PackageReference` entries as follows and remove the old package references to avoid duplicate assembly references.

| Previous package ID | Current package ID |
| --- | --- |
| `HuaweiCloud.Driver.GaussDB` | `HuaweiCloud.GaussDB.Driver` |
| `HuaweiCloud.Driver.GaussDB.NodaTime` | `HuaweiCloud.GaussDB.Driver.NodaTime` |
| `HuaweiCloud.Driver.GaussDB.NetTopologySuite` | `HuaweiCloud.GaussDB.Driver.NetTopologySuite` |
| `HuaweiCloud.Driver.GaussDB.DependencyInjection` | `HuaweiCloud.GaussDB.Driver.DependencyInjection` |

## Quick start

Here's a basic code snippet to get you started:

```csharp
using HuaweiCloud.GaussDB;

var connString = "Host=myserver;Username=mylogin;Password=mypass;Database=mydatabase";

var dataSourceBuilder = new GaussDBDataSourceBuilder(connString);
var dataSource = dataSourceBuilder.Build();

var conn = await dataSource.OpenConnectionAsync();

await using (var cmd = new GaussDBCommand("INSERT INTO data (some_field) VALUES (@p)", conn))
{
    cmd.Parameters.AddWithValue("p", "Hello world");
    await cmd.ExecuteNonQueryAsync();
}

await using (var cmd = new GaussCommand("SELECT some_field FROM data", conn))
await using (var reader = await cmd.ExecuteReaderAsync())
{
    while (await reader.ReadAsync())
        Console.WriteLine(reader.GetString(0));
}
```

## License

This project is released under of the [PostgreSQL License](https://opensource.org/license/PostgreSQL).
