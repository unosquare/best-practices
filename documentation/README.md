# Open Source Projects Documentation guideline

This guideline contains a couple of recipies for document an Open Source Project. They are not mandatory to all Unosquare projects but they can give a clear explanation of what to do to handle the documentation.

## Table of Contents

* [README file](#readme-file)
  * [Overview](#overview)
  * [Table of Contents (TOC)](#table-of-contents-toc)
  * [Requirements](#requirements)
  * [Installation](#installation)
  * [Usage](#usage)
  * [Examples](#examples)
  * [Troubleshooting](#troubleshooting)
  * [Related Projects](#related-projects)
  * [Special Thanks](#special-thanks)
  * [License](#license)
* [Source code documentation](#source-code-documentation)
  * [.NET](#net)
  * [Javascript](#javascript)
  
## README file

Every Open Source Project should contain a README file in the root folder. This file may be a markdown or plain text. The README file will contain information about the project and it should be the initial place where an user can be brief. 

Depending of the nature of the project, the document may contain the following sections:

### Overview

The overview is one of the most important section in a README file. It should describe in short sentences what is the project. Any additional details should be explained in following topics. Don't sature the overview with examples or instructions. 

The overview may contain a header with badges to give a peak of the status of the project such as continuous integration health status, current release version, etc.

### Table of Contents (TOC)

A TOC is a helpful tool to structure the topics in the README file. The use of hyperlinks when the document is markdown is mandatory. The TOC should be show the topics indented properly. 

The order of the following sections are discutable but it may help you to build a proper TOC.

### Requirements

When the project is an application, you may need to list the minimal requirements for operation. Including what Operating System is required and the dependencies. If the project is a library, you may just need list what runtimes can be used. For example what target frameworks can be used.

### Installation

The installation is an important section where the user should be able to have clear instructions how to deploy the project or get a copy of the project if this is a library. For example you can illustrate with console commands:

```sh
> npm install mylibrary
```

### Usage

THe Usage section should enumerate the features of the project and provinding information of how to use each of them. This section should include subsections for each feature if they are complex. Incluing source code examples is also a good idea in this point. And you may not create an entire examples section.

Providing the Usage section with samples can deliver an guided experiencie to the reader, similar to a brief tutorial.

### Examples

If the Usage section is not required, and just a simple use case can be provided is not bad idea keep it simple. A documented source code with a specific example can provide a better explanation in many cases.

### Troubleshooting

An application project, that needs to be deployed or installed, may have some known issues or common pitfalls. The Troubleshooting section aims to reduce the duplicated issues in the repository, by giving a common issues base with solutions.

### Related Projects

An small section of related projects should help to the reader to find similar projects or the same library in other runtime. For example a React project with a counterpart in AngularJS may be listed here.

### Special Thanks

If the project contains or use any commercial product with open source license, here you must thanks to the company.

### License

Finally, if you need to add additional information about the licesing in the project, include a paragrah in the very end of the README file. You can also provide a LICENSE file in the root path.

## Source code documentation



### .NET

### Javascript
