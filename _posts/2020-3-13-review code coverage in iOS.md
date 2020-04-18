---
post: page
title: test code coverage in your Unit test case 
tag: [iOS, Unit test]
comment: true 
---
Test is a repeated task in daily development, but test can make your code strong and safe.
For developers, we should write the code based on TDD style, in a world test-driven development.
Code coverage refers to the percentage of code tested, using automated unit or functional tests.
The code coverage files can be generated directly using the Apple LLVM code generator, and you can set that option from within Xcode. There are two ways to do that — one allows report visualization from within Xcode and the other generates XML/HTML-based reports.
Integrated coverage report
Open the Xcode , do the command order as below:
Product ->Edit Scheme(shortcut :⌘ <) ->Test ->Options ->Code Coverage(select the target)

![code coverage](https://miro.medium.com/max/552/1*tXDI9psOrKUQldzW43iCDg.png)


And then you can choose test and then enter into the test, you can see the report.

![](https://miro.medium.com/max/552/1*knRBc5JV0pMyH2m0CwwsRA.png)

But it’s not smart to give other people the report to read.
The smarter way is that External coverage report.
You can also generate reports in XML or HTML formats. This is useful when you use continuous integration and tests run on a non-developer machine, or when you want to preserve the reports for later.
To generate the files that will contain coverage data, turn on the following flags:
Generate Debug Symbols — will emit the debug symbols in the compiled binary
Generate Test Coverage file — Will generate the binary files that contain coverage data
Instrument Program flow — will instrument the app as the test cases execute
It’s recommended to create a custom build configuration to isolate the coverage from regular builds, you can find the regular test
![](https://miro.medium.com/max/552/1*NEgccFBX9eDX6SH9wtEXrg.png)

This will generate the .gcno and .gcda files in the derived data folder of the project.
The .gcno file has details to reconstruct the basic block graphs and assign source line numbers to blocks.
The .gcda file contains the ARC transition count.
Next step: use these files to generate the report that can be exported to either XML or HTML format.
There are two tools are useful to make it happen:
1. lcov -let us collect coverage data from a file into a unified file known as the INFOFILE
2. genhtml- generates the HTML report using the INFOFILE generated by the lcov tool.
lcov is not installed on macOS, so firstly you must install it.

`brew install lcov`

To generate the code coverage report, you must use the script in the build


`lcov --directory "${OBJECT_FILE_DIR_normal}/${CURRENT_ARCH}"
    --capture
    --output-file "${PROJECT_DIR}/${PROJECT_NAME}.info"
genhtml --output-directory "${PROJECT_DIR}/${PROJECT_NAME}-coverage"
    "${PROJECT_DIR}/${PROJECT_NAME}.info"
`

and then you should see the coverage report in the folder named <Your_project_name>coverage in the project folder. If you open the main file, index.html, you should see a report, but you can export it to PDF