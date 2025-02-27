# Contributing test cases

Test cases in this folder can be generated programmatically from a scripting language, designed to minimize the maintainers effort.

If possible, contributions to the test suite should come as Pull request.

Each test should be composed of two parts:

1. An entry in the relevant [documentation file](scripts.md).
1. A minimised IFC file that should be verified agains the resulting IDS.

## Documentation script

A documentation script snippet looks something like the following:

```` text
### Test case title

An optional (but welcome) description of the rationale of the test.

``` ids attribute/<pass/fail/invalid>-<Test file name>.ids
Test case title
IFC4
Entity: ''IFCPRESENTATIONLAYERWITHSTYLE''
Requirements:
Attribute: ''LayerOn''
```
````

Each code block commencing with the ` ids ` sequence, followed by a local file name will be converted into an IDS.

The syntax of the script within the [triple backticks](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks#fenced-code-blocks) is as follows:

### Title

The first line is always interpreted as the title of the IDS and the name of the specification included, this is a required line

### Schemas

If the following line is a sequence of `IFC2X3`, `IFC4`, and `IFC4X3` tokens, in any order, it defines schemas for the specification.
This line is optional, when the schema is omitted, the default schemas of the IDS are `IFC2X3 IFC4`. Note that the capitalization of this line matters.

### Applicability cardinality

If the following line is one of the tokes `Optional`, `Required`, or `Prohibited`, it defines the cardinality for the applicability of the IDS.
This line is optional, when omitted, the default cardinality is set to `Required`. Note that the capitalization of this line matters.

### Applicability facets

Each subsequent line is interpreted as applicability facets, until the `Requirements:` token is encountered

### Requirements facets

Once the `Requirements:` token has been found, each subsequent line is interpreted as a requirement facets.

## Automation

It is possible to automate the conversion of an existing IDS to the scripting language.

To do so, you can write an IDS file in a subdirectory of the `testcases` folder and execute a script on your computer by launching one of the build targets.

The script execution requires the .NET 6.0 SDK installed on your computer, which is avaliable for Windows, MacOS and Linux.

Depending on your system you then launch the appropriate command in the root folder of the repository:

1. On windows power shell: `./build.ps1 CreateTestCases`
1. On windows command prompt: `build CreateTestCases`
1. On Mac terminal: `./build.sh CreateTestCases`
1. On linux terminal: `./build.sh CreateTestCases`

The resulting output should contain a section like the following:

``` text
╬════════════════════
║ CreateTestCases
╬═══════════

17:33:55 [INF] > "C:\Program Files\dotnet\dotnet.exe" run --configuration Release --project C:\Data\Dev\BuildingSmart\IDS\SchemaProject\SchemaProject.csproj
17:33:57 [DBG] Hello IDS!
17:33:57 [DBG] Process started in: C:\Data\Dev\BuildingSmart\IDS
17:33:57 [DBG] Testcase generation started in: C:\Data\Dev\BuildingSmart\IDS\Documentation\testcases
17:33:57 [DBG] Extra IDS report generated: C:\Data\Dev\BuildingSmart\IDS\Documentation\testcases\library\sample1.html
17:33:57 [DBG] Extra IDS report generated: C:\Data\Dev\BuildingSmart\IDS\Documentation\testcases\library\sample2.html
17:33:57 [DBG] Extra IFC: - C:\Data\Dev\BuildingSmart\IDS\Documentation\testcases\entity\fail-a_predefined_type_must_always_specify_a_meaningful_type__not_userdefined_itself.ifc
17:33:57 [DBG] Extra IFC: - C:\Data\Dev\BuildingSmart\IDS\Documentation\testcases\library\sample1.ifc
17:33:57 [DBG] Extra IFC: - C:\Data\Dev\BuildingSmart\IDS\Documentation\testcases\library\sample2.ifc
17:33:57 [DBG] All scripting IFC files found
17:33:57 [DBG]
17:33:57 [DBG] Done
```

The  `Extra IDS report generated:` text in there will indicate an HTML report containing:

1. the syntax of the converted scripting IDS
2. a differences between your original IDS and the one generated by the script

This information should help you produce a good PR. Adjust the script as needed and add it to the documentation following the advice above.
