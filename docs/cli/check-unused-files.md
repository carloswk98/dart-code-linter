import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Check Unused Files
Checks unused `*.dart` files. To execute the command, run:

<Tabs>
  <TabItem value="dart" label="Dart" default>

```sh:
$ dart run dart_code_metrics:metrics check-unused-files lib
```
</TabItem>
  <TabItem value="flutter" label="Flutter">

```sh
$ flutter pub run dart_code_metrics:metrics check-unused-files lib
```
</TabItem>
</Tabs>

Full command description:

```sh:
Usage: metrics check-unused-files [arguments...] <directories>

-h, --help                                        Print this usage information.


-r, --reporter=<console>                          The format of the output of the analysis.
                                                  [console (default), json]

-c, --print-config                                Print resolved config.


    --root-folder=<./>                            Root folder.
                                                  (defaults to current directory)
    --sdk-path=<directory-path>                   Dart SDK directory path.
                                                  Should be provided only when you run the application as compiled executable(https://dart.dev/tools/dart-compile#exe) and automatic Dart SDK path detection fails.
    --exclude=<{/**.g.dart,/**.freezed.dart}>    File paths in Glob syntax to be exclude.
                                                  (defaults to "{/**.g.dart,/**.freezed.dart}")


    --no-congratulate                             Don't show output even when there are no issues.


    --[no-]fatal-unused                           Treat find unused file as fatal.

-d, --[no-]delete-files                           Delete all unused files.
```
## Suppressing the command
In order to suppress the command add `ignore_for_file: unused-files` to the beginning of a file.

## Monorepo support
By default the command treats all files that are exported from the package as used. To disable this behavior use `--monorepo` flag. This might be useful when all the packages in your repository are only used within the repository and are not published to the pub.

## Output example
### Console
Use `--reporter=console` to enable this format.
![HTML Report](/static/img/unused-files/unused-files-console-report.png)
### JSON
The reporter prints a single JSON object containing meta information and the unused file paths. Use `--reporter=json` to enable this format.
##### The root object fields are
- `formatVersion` - an integer representing the format version (will be incremented each time the serialization - format changes)
- `timestamp` - a creation time of the report in YYYY-MM-DD HH:MM:SS format
- `unusedFiles` - an array of [unused files](/docs/cli/check-unused-files.md#the-unusedfiles-object-fields-are)
- `automaticallyDeleted` - an indication of unused files being automatically deleted
```json:
{
  "formatVersion": 2,
  "timestamp": "2021-04-11 14:44:42",
  "unusedFiles": [
    {
      ...
    },
    {
      ...
    },
    {
      ...
    }
  ],
  "automaticallyDeleted": false
}
```
##### The unusedFiles object fields are
- `path` - a relative path of the unused file
```json:
{
  "path": "lib/src/some/file.dart",
}
```
