# The C# Unosquare Labs Best Practices

Welcome to the C# Best Practices and Style Guides. We provide you some C# standards and tools that we use in our OSS projects.

Enjoy it!

## Table of Contents

- Coding
  - [StyleCop](#stylecop)
  - [Unit Testing](#unit-testing)
- Documentation
  - [Ghost Doc](#ghost-doc)
  - [DocFx](#docfx)

# Coding

## StyleCop

[StyleCop](https://github.com/DotNetAnalyzers/StyleCopAnalyzers) is a tool to analyze C# source code and detect possible issues and improve code quality.

### How to use it

* Install the [StyleCop.Analyzers](https://www.nuget.org/packages/StyleCop.Analyzers/) nuget in each project in your solution.
* Copy the file [StyleCop.Analyzers.ruleset](https://github.com/unosquare/best-practices/blob/master/C%23/StyleCop.Analyzers.ruleset) in the your solution's root folder.
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

## Unit Testing

It's **mandatory** to test all your code against different scenarios to prevent some failures when the project is under production.

You don't want to hear complaints from your clients or bosses, so you need to make tests for every possible scenario that will crash, or destroy, your code. There are a lot of testing tools in the market to do this job.

In Unosquare Labs we have used an standard to organize our test classes and methods that has helped us to make our tests more readable and understandable. Remember, your code could be read by other programmers and testers (also clients). So please, think first in them.

### How to name unit tests

Follow the next steps:

* Create a abstract class for the class to be tested (`YourAwesomeClass`, for example). The name of the abstract class  should suffix Test. This class will only have shared members and `SetUp` and `TearDown` methods. This step is optional, if you target class doesn't require any initialization or shared members, you can skip the base class creation.
* The classes with the name of the methods to be tested (`Methods`). These classes will inherit from the base class, so you don't need to reinvent the wheel initializing variables, constants, etc.
* The name of the class must be descriptive and clear to understand. 
* Inside of the class create the methods with the name of the possible scenarios that will occur. What are your `InitialConditions` and what will be your `ExpectedResults`?
* Optional 1: Use [mocks](https://stackoverflow.com/questions/2665812/what-is-mocking) for your tests.
* Optional 2: Change the name of the `namespace `(it could be, for example, `namespace YourWorkspace.Test.ClassTests`, in plural)
* Note 1: If there aren't anything to setup, omit the abstract class.

Here is the pattern that we are proposing: 
```csharp
using NUnit.Framework;

namespace YourWorkspace.Test.YourAwesomeClassTest
{
        // Use an abstract class as long as you need any setup. Omit it if not
	public abstract class YourAwesomeClassTest
	{
		//Your setup
	}

	[TestFixture]
	public class SomeMethodToTest : YourAwesomeClassTest
	{
		[Test]
		public void OnePossibleScenarioOfTheMethod_ExpectedResult()
		{
			//Your code
		}

		//Other methods
	}

	//Other classes
}
```
Let's suppose that you have a Calculator class, with basic functionalities (addition, multiplication, substraction and division), and to verify if your basic calcultor works you need to test the methods.

Using the pattern proposed, your test class should look like this:
```csharp
using NUnit.Framework;

namespace YourWorkspace.Test.CalculatorTests
{
    public abstract class CalculatorTest
    {
        protected double x = 234.12, y = 134;
        protected string xString = "Hi", yString = "Something";
    }

    [TestFixture]
    public class Addition : CalculatorTest
    {
        [Test]
        public void SumTwoNumbers_ReturnsTrue()
        {
            var calc = new Calculator();

            Assert.AreEqual(calc.Addition(x, y), 368.12);
        }

        [Test]
        public void SumTwoNumbers_ReturnsFalse()
        {
            var calc = new Calculator();

            Assert.AreEqual(calc.Addition(x, y), 5);
        }

        [Test]
        public void SumTwoRandomVariables_ThrowsException()
        {
            var calc = new Calculator();

            Assert.Throws<Exception>(() =>
            {
                calc.Addition(xString, y);
            });
        }

        //Other possible scenarios
    }

    //Your other classes to test
}
```
Following this convention makes clearer and easy to understand the code that you're writing, and as stated before: your code could be read by other programmers and testers (also clients). So please, think first on them.

Advantages of this convention:

* Code tests well organized
* Method names more descriptives
* Easy to maintain

Disadvantages of this convention:

* Nothing

[Check](https://github.com/unosquare/swan/blob/master/test/Unosquare.Swan.Test/ObjectComparerTest.cs) how our test classes are structured in one of our OSS projects.

We are proudly to share with you this standard, so feel free to use this in your projects. Enjoy it!

# Documentation

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
