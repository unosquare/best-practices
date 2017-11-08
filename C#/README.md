# C# Best Practices

## StyleCop

[StyleCop](https://github.com/DotNetAnalyzers/StyleCopAnalyzers) is a tool to analyze C# source code and detect possible issues and improve code quality.

### How to use it

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
* Build your solution. The StyleCop rules create warning from your code.

# C# Documentation

## Ghost Doc

[Ghost Doc](https://submain.com/products/ghostdoc.aspx?utm_campaign=ghostdoc&utm_medium=listing&utm_source=visualstudiogallery) is a Visual Studio extension that automatically generates XML documentation comments for classes and memebrs based on their type, parameters, name, and other contextual information.

To use it, you need to [download](https://marketplace.visualstudio.com/items?itemName=sergeb.GhostDoc) or install the extension from Visual Studio 2017

In Visual Studio 2017 go to:

* Tools > Extension and Updates > Online 

then search for GhostDoc Community for VS2017 and installed.

In your code when you want to document something you need to select your class or method, rigth click and with the extension activited select:

* GhostDoc > Document This

Another way to do that is selecting your class or method and with the keyboard command:

* Ctrl + Shift + D

## DocFx

[DocFX](https://dotnet.github.io/docfx/) is a documentation generation tool for API reference and Markdown files! To use it you need to [download](https://github.com/dotnet/docfx/releases) it. We recommend using [Chocolatey](https://chocolatey.org) to install DocFx.

### How to use it

You need to setup your solution with a two config files (`docfx.json` and `toc.yml`) in order to build a documentation web site. For more information check the [documentation](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html?tabs=tabid-1%2Ctabid-a).

#### `docfx.json` file

The `src` property is where you specified the c # project and solution that docfx is going to use to make the documentation.

The `template` property is optional, docfx is going to use the default template, but you can modify this by exporting the template `docfx template export default` and then add to the docfx.json the `template` property and the path where the template was saved.

```json
{
  "metadata": [
    {
      "src": [
        {  
          "files": [ "**/*.sln", "src/DotnetNew/*.csproj" ],
          "exclude": [ "**/bin/**", "**/obj/**" ],
          "cwd": "src"
        }
      ],
      "dest": "obj/api"
    }
  ],
  "build": {
    "template": [
      "templates/default"
    ],
    "content": [
      {
        "files": [ "**/*.yml" ],
        "cwd": "obj/api",
        "dest": "api"
      },
      {
        "files": [ "*.md", "toc.yml", "restapi/**" ]
      }
    ],
    "resource": [
      {
        "files": [ "resources/**"],
        "exclude": [ "obj/**", "_site/**" ]
      }
    ],
    "overwrite": "specs/*.md",
    "globalMetadata": {
      "_appTitle": "docfx seed website",
      "_enableSearch": true,
      "_docLogo": "resources/images/doc_logo.svg",
      "_appLogoPath": "resources/images/logo.png"
    },
    "dest": "_site"
  }
}

```
#### `toc.yml` file

```yml
# This are the names that appear on the navigation bar
- name: API Documentation
# This is the reference
  href: obj/api/
# This are the names that appear on the navigation bar
- name: REST API
# This is the reference
  href: restapi/
```

Here some useful comands:

```
// To export the default template and make modifications
> docfx template export default

// To build the documentation and the site
> docfx docfx.json

// To build and run the site
> docfx docfx.json --serve

// To build with other template
> docfx -t templates\custom
```
