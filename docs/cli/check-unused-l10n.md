import Admonition from '@theme/Admonition';
# Check Unused L10n
Checks unused Dart class members, that encapsulates the app's localized values.

An example of such class:

```dart:
class ClassWithLocalization {
  String get title {
    return Intl.message(
      'Hello World',
      name: 'title',
      desc: 'Title for the Demo application',
      locale: localeName,
    );
  }
}
```

Read more about this localization approach [in the Flutter docs](https://web.archive.org/web/20230314204503/https://flutter.dev/docs/development/accessibility-and-localization/internationalization#defining-a-class-for-the-apps-localized-resources).

<Admonition type="info" icon="" title="INFO">
  <p>
By default the command searches for classes that end with `I18n`, but you can override this behavior with `--class-pattern` argument.
  </p>
</Admonition>

To execute the command, run:


<Tabs>
  <TabItem value="dart" label="Dart" default>

```sh:
$ dart run dart_code_metrics:metrics check-unused-l10n lib
```
</TabItem>
  <TabItem value="flutter" label="Flutter">

```sh
$ flutter pub run dart_code_metrics:metrics check-unused-l10n lib
```
</TabItem>
</Tabs>



Full command description:

## Output example
### Console
Use `--reporter=console` to enable this format.

![HTML Report](/static/img/unused-files/unused-files-console-report.png)

### JSON
The reporter prints a single JSON object containing meta information and the unused file paths. Use `--reporter=json` to enable this format.

##### The root object fields are
- `formatVersion` - an integer representing the format version (will be incremented each time the serialization format changes)
- `timestamp` - a creation time of the report in YYYY-MM-DD HH:MM:SS format
- `unusedLocalizations` - an array of [unused files](/docs/cli/check-unused-l10n.md#the-unusedlocalizations-object-fields-are)

```json:
{
  "formatVersion": 2,
  "timestamp": "2021-04-11 14:44:42",
  "unusedLocalizations": [
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

##### The unusedLocalizations object fields are
- `path` - a relative path of the unused file
- `className` - a name of the class that has unused members
- `issues` - an array of [issues](/docs/cli/check-unused-l10n.md#the-issue-object-fields-are) detected in the -target class

```json:
{
  "path": "lib/src/some/class.dart",
  "className": "class",
  "issues": [
    ...
  ],
}
```

##### The issue object fields are
- `memberName` - unused class member name
- `offset` - a zero-based offset of the class member location in the source
- `line` - a zero-based line of the class member location in the source
- `column` - a zero-based column of class member the location in the source

```json:
{
  "memberName": "someGetter",
  "offset": 156,
  "line": 7,
  "column": 1
}
```
