# StyleCop

[StyleCop](https://github.com/DotNetAnalyzers/StyleCopAnalyzers) is a tool to analyze C# source code and detect possible issues and improve code quality.

## How to use it

* Install the [StyleCop.Analyzers](https://www.nuget.org/packages/StyleCop.Analyzers/) nuget in each project in your solution.
* Copy the file *StyleCop.Analyzers.ruleset* in the your solution's root folder.
* Reference the ruleset using the following XML node in each CSPROJ file:

```
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        ...
        <CodeAnalysisRuleSet>..\..\StyleCop.Analyzers.ruleset</CodeAnalysisRuleSet>
    </PropertyGroup>
    ...
</Project>
```