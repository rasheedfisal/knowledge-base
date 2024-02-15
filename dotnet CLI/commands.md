### Check .Net version

`dotnet --version`

### Check Dotnet Project Templates

`dotnet new list`
`dotnet new list -h`
`dotnet --list-sdks`
`dotnet --version`

## create the appropriate global.json file

`dotnet new globaljson --sdk-version 2.2.105`

### Create new WebAPI Template

`dotnet new webapi -n [project name]`

### Add projects to solution (you can add multiple in one command or one).
`
dotnet sln MyApiApp.sln add .\MyApiApp.ConsoleApp\MyApiApp.ConsoleApp.csproj .\MyApiApp.WebApi\MyApiApp.WebApi.csproj .\MyApiApp.Repository\MyApiApp.Repository.csproj .\MyApiApp.Tests\MyApiApp.Tests.csproj`


### Reference Library Project to Web API Project
`dotnet add SampleDotNet.Api/SampleDotNet.Api.csproj reference SampleDotNet.Services/SampleDotNet.Services.csproj`

### Adding Packages in .Net

`dotnet add package [package name]`
or
`dotnet add package [package name] ----prerelease`
--prerelease flag to make sure we pull in the latest packages

### some of the most used packages

| Package Name                                        |
| --------------------------------------------------- |
| AutoMapper.Extensions.Microsoft.DependencyInjection |
| Microsoft.EntityFrameworkCore                       |
| Microsoft.EntityFrameworkCore.Design                |
| Microsoft.EntityFrameworkCore.InMemory              |
| Microsoft.EntityFrameworkCore.SqlServer             |
| Z.EntityFramework.Extensions.EFCore            	  |


### add ef core tool
`dotnet tool update --global dotnet-ef --prerelease`
with --prerelease flag to make sure we pull in the latest packages

### The correct format to add a new migration is:
`dotnet ef migrations add yourMigrationName`
`dotnet ef database update`

### to Undo Migration
`dotnet ef migrations remove`

### Remove all files from the migrations folder.
`dotnet ef database drop -f -v`

### List All migrations.
`dotnet ef migrations list`