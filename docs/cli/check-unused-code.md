import Admonition from '@theme/Admonition';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

### Check Unused Code

Checks unused classes, functions, top level variables, extensions, enums, mixins and type aliases.

<Admonition type="info" icon="" title="INFO">
  <p>
This implementation doesn't check for particular class members (fields / properties / methods) usage. If you need this functionality, refer to check-unused-code command for Teams version.
  </p>
</Admonition>

To execute the command, run:

<Tabs>
  <TabItem value="dart" label="Dart" default>

```sh:
$ dart run dart_code_metrics:metrics check-unused-code lib
```
</TabItem>
  <TabItem value="flutter" label="Flutter">

```sh
$ flutter pub run dart_code_metrics:metrics check-unused-code lib
```
</TabItem>
</Tabs>

Full command description:

```sh:
Usage: metrics check-unused-code [arguments] <directories>
-h, --help                                        Print this usage information.


-r, --reporter=<console>                          The format of the output of the analysis.
                                                  [console (default), json]
    --report-to-file=<path/to/report.json>        The path, where a JSON file with the analysis result will be placed (only for the JSON reporter).

-c, --print-config                                Print resolved config.


    --root-folder=<./>                            Root folder.
                                                  (defaults to current directory)
    --sdk-path=<directory-path>                   Dart SDK directory path.
                                                  Should be provided only when you run the application as compiled executable(https://dart.dev/tools/dart-compile#exe) and automatic Dart SDK path detection fails.
    --exclude=<{/**.g.dart,/**.freezed.dart}>    File paths in Glob syntax to be exclude.
                                                  (defaults to "{/**.g.dart,/**.freezed.dart}")


    --no-congratulate                             Don't show output even when there are no issues.


    --[no-]monorepo                               Treat all exported code as unused by default.


    --[no-]fatal-unused                           Treat find unused file as fatal.
```

### Suppressing the command
In order to suppress the command add the `ignore: unused-code` comment. To suppress for an entire file add `ignore_for_file: unused-code` to the beginning of a file.

### Monorepo support
By default the command treats all code that is exported from the package as used. To disable this behavior use `--monorepo` flag. This might be useful when all the packages in your repository are only used within the repository and are not published to the pub.

### Output example

#### Console
Use --reporter=console to enable this format.

![HTML Report](/static/img/unused-code/unused-code-console-report.png)
#### JSON

The reporter prints a single JSON object containing meta information and the unused code file paths. Use `--reporter=json` to enable this format.

The root object fields are
- `formatVersion` - an integer representing the format version (will be incremented each time the serialization format changes)
- `timestamp` - a creation time of the report in YYYY-MM-DD HH:MM:SS format
- `unusedCode` - an array of [unused code issues](/docs/cli/check-unused-code.md#the-unusedcode-object-fields-are)


```json:
{
  "formatVersion": 2,
  "timestamp": "2021-04-11 14:44:42",
  "unusedCode": [
    {
      ...
    },
    {
      ...
    },
    {
      ...
    }
  ]
}
```
##### The unusedCode object fields are
- path - a relative path of the unused file
- issues - an array of [issues](/docs/cli/check-unused-code.md#the-issue-object-fields-are) detected in the target file
```json:
{
  "path": "lib/src/some/file.dart",
  "issues": [
    ...
  ],
}
```
##### The issue object fields are
- `declarationType` - unused declaration type
- `declarationName` - unused declaration name
- `offset` - a zero-based offset of the class member location in the source
- `line` - a zero-based line of the class member location in the source
- `column` - a zero-based column of class member the location in the source

```json:
{
  "declarationType": "extension",
  "declarationName": "StringX",
  "offset": 156,
  "line": 7,
  "column": 1
}
```
