import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Check Unnecessary Nullable Parameters

Checks unnecessary nullable parameters in functions, methods, constructors. Removing unnecessary nullables can help reduce amount of checks in the code.

To execute the command, run:

<Tabs>
  <TabItem value="dart" label="Dart" default>

```sh:
$ dart run dart_code_metrics:metrics check-unnecessary-nullable lib
```
</TabItem>
  <TabItem value="flutter" label="Flutter">

```sh
$ flutter pub run dart_code_metrics:metrics check-unnecessary-nullable lib

```
</TabItem>
</Tabs>

Full command description:

```sh:
Usage: metrics check-unnecessary-nullable [arguments] <directories>
-h, --help                                        Print this usage information.


-r, --reporter=<console>                          The format of the output of the analysis.
                                                  [console (default), json]

-c, --print-config                                Print resolved config.


    --root-folder=<./>                            Root folder.
                                                  (defaults to current directory)
    --sdk-path=<directory-path>                   Dart SDK directory path. Should be provided only when you run the application as compiled executable(https://dart.dev/tools/dart-compile#exe) and automatic Dart SDK path detection fails.
    --exclude=<{/**.g.dart,/**.freezed.dart}>    File paths in Glob syntax to be exclude.
                                                  (defaults to "{/**.g.dart,/**.freezed.dart}")


    --no-congratulate                             Don't show output even when there are no issues.


    --[no-]monorepo                               Treat all exported code with parameters as non-nullable by default.


    --[no-]fatal-found                            Treat found unnecessary nullable parameters as fatal.
```

## Suppressing the command
In order to suppress the command add the `ignore: unnecessary-nullable` comment. To suppress for an entire file add `ignore_for_file: unnecessary-nullable` to the beginning of a file.

## Monorepo support
By default the command treats all code that is exported from the package as used. To disable this behavior use `--monorepo` flag. This might be useful when all the packages in your repository are only used within the repository and are not published to the pub.

## Output example

### Console
Use `--reporter=console` to enable this format.

![HTML Single Report](/static/img/unncesessary-nullable/unnecessary-nullable-console-report.png)

### JSON

The reporter prints a single JSON object containing meta information and the unnecessary nullable parameters. Use `--reporter=json` to enable this format.

##### The root object fields are
- `formatVersion` - an integer representing the format version (will be incremented each time the serialization format changes)
- `timestamp` - a creation time of the report in YYYY-MM-DD HH:MM:SS format
- `unnecessaryNullable` - an array of [unnecessary nullable issues](/docs/cli/check-unnecessary-nullable.md#the-unnecessarynullable-object-fields-are)

```json:
{
  "formatVersion": 2,
  "timestamp": "2021-04-11 14:44:42",
  "unnecessaryNullable": [
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

##### The unnecessaryNullable object fields are
- `path` - a relative path of the file with unnecessary nullable parameters declaration
- `issues` - an array of [issues](/docs/cli/check-unnecessary-nullable.md#the-issue-object-fields-are) detected in the target class

```json:
{
  "path": "lib/src/some/class.dart",
  "issues": [
    ...
  ],
}
```

##### The issue object fields are
- `declarationName` - the name of a declaration with unnecessary nullable parameters
- `declarationType` - the type of a declaration with unnecessary nullable parameters (function, method or constructor)
- `parameters` - an array of strings representing parameters that are marked as nullable
- `offset` - a zero-based offset of the class member location in the source
- `line` - a zero-based line of the class member location in the source
- `column` - a zero-based column of class member the location in the source

```json:
{
  "declarationName": "someFunction",
  "declarationType": "function",
  "parameters": "[String? value]",
  "offset": 156,
  "line": 7,
  "column": 1
}
```
