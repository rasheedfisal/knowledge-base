### Check .Net version

`dotnet --version`

### Check Dotnet Project Templates

`dotnet new --list`
`dotnet --list-sdks`
`dotnet --version`

## create the appropriate global.json file

`dotnet new globaljson --sdk-version 2.2.105`

### Create new WebAPI Template

`dotnet new webapi -n [project name]`

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


### add ef core tool
`dotnet tool update --global dotnet-ef --prerelease`
with --prerelease flag to make sure we pull in the latest packages
