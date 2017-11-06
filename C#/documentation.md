# Documentation

This is a quick guide to document your code.

## Ghost Doc

[Ghost Doc](https://submain.com/products/ghostdoc.aspx?utm_campaign=ghostdoc&utm_medium=listing&utm_source=visualstudiogallery) is a Visual Studio extension that automatically generates XML documentation comments for methods and properties based on their type, parameters, name, and other contextual information.

To use it, you need to [download](https://marketplace.visualstudio.com/items?itemName=sergeb.GhostDoc) or install the extension from Visual Studio 2017

In Visual Studio 2017 go to 
* Tools > Extension and Updates > Online 

then search for GhostDoc Community for VS2017 and installed.

In your code when you want to document something you need to select your class or method, rigth click and with the extension activited select
* GhostDoc > Document This

Another way to do that is selecting your class or method and with the keyboard command 
* Ctrl + Shift + D

## DocFx

[DocFX](https://dotnet.github.io/docfx/) is a documentation generation tool for API reference and Markdown files!. It generates Documentation directly from source code (.NET, RESTful API, JavaScript, Java, etc...) and Markdown files.

To use it you need to [download](https://github.com/dotnet/docfx/releases) it and set the path enviroment to use docfx to the console

### Set the path

To set the path enviroment to docfx you need to do as fallow

* Open Control Panel > System > Advance System Configuration
* Click on Environment Variables
* Select Path and Edit
* Put the path where you extract the files of DocFx, for example
    * C:\DocFx

To check if docfx is working

```
C:\Users\user> docfx -v
docfx 2.26.4.0
Copyright (C) 2017 Copyright © Microsoft docfx 2015-2017
This is open-source software under MIT License.
```

### Configure docfx

To more information check the [documentation](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html?tabs=tabid-1%2Ctabid-a).

Here a quick guide. There are some files that are required.
* `index.md`
* `docfx.json`
* `toc.yml`

#### Set the `index.md`

It is just a markdown file, it will be the home/index of you application.

#### Set the `docfx.json`

```json
{
  "metadata": [
    {
      "src": [
        { 
            // Here are the project that you convert into the documentation       
          "files": [ "**/*.sln", "src/DotnetNew/*.csproj" ],
          "exclude": [ "**/bin/**", "**/obj/**" ],
          "cwd": "src"
        }
      ],
      "dest": "obj/api"
    }
  ],
  "build": {
      // You can use the default template and modify or leave this in blank and docfx would use the default
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

Here some usefull comands:

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

#### Set the `toc.yml`

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