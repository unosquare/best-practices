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

You need to setup your solution with two config files (`docfx.json` and `toc.yml`) and one `index.md` (usually is the same as `README.md`) in order to build a documentation web site. For more information check the [documentation](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html?tabs=tabid-1%2Ctabid-a).

#### `docfx.json` file

The `src` property is where you specified the solution and C# project that docfx is going to use to make the documentation.

The `template` property is optional, docfx is going to use the default template, but you can modify this by exporting the template `docfx template export default` and then add to the `docfx.json` the `template` property and the path where the template was saved.

To build docfx.
> docfx docfx.json

Build and serve.
> docfx docfx.json --serve

If you have several custom templates, use this to build with another template.
> docfx -t templates\custom

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

These are references to the navigation bar that docfx is going to create, it will appear with the name and have a link to the href that are specified in this file.

```yml
- name: API Documentation
  href: obj/api/

- name: REST API
  href: restapi/
```

### Configuration of docfx in `appveyor.yml` for another project

Since we have the template in this repository, you only need to add the docfx.json and the toc.yml to the root of your project, making the necessary modifications to adapt it to the project (generally only adding the routes to the `src` property and changing the `_docLogo` to the logo of your project), and then adding these lines after configuring the git credentials and cloning your main project.

````
git clone -b documentation https://github.com/unosquare/best-practices.git -q
docfx docfx.json --logLevel Error
````

And then these lines to have an index inside the destination folder `_site`

````
CD _site
Copy-Item README.html index.html -force
````
