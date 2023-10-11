---
sidebar_position: 4
title: CLI
---
import Admonition from '@theme/Admonition';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Command Line Interface
To use the package as a command-line tool, run:
<Tabs>
  <TabItem value="dart" label="Dart" default>

```sh:
$ dart run dart_code_metrics:metrics <command> lib
```
</TabItem>
  <TabItem value="flutter" label="Flutter">

```sh
$ flutter pub run dart_code_metrics:metrics <command> lib
```
</TabItem>
</Tabs>


Alternatively, the package can be installed globally:

<Tabs>
  <TabItem value="dart" label="Dart" default>
  
```sh:
$ dart pub global activate dart_code_metrics
$ metrics <command> lib
```
</TabItem>
  <TabItem value="flutter" label="Flutter">

```sh
$ flutter pub global activate dart_code_metrics 
$ metrics <command> lib
```
</TabItem>
</Tabs>


It will produce a result in one of the supported formats:
- Console
- GitHub
- Checkstyle
- Codeclimate
- HTML
- JSON

<Admonition type="info" icon="" title="INFO">
  <p>
You need to configure `rules` entry in the `analysis_options.yaml` to have rules report included into the result.
  </p>
</Admonition>

## Available commands
| Command | Example of use | Short description |
|----------|----------|----------|
| analyze | dart run dart_code_linter:metrics analyze lib   | Reports code metrics, rules and anti-patterns violations. |
| check-unnecessary-nullable   | dart run dart_code_linter:metrics check-unnecessary-nullable lib   | Checks unnecessary nullable parameters.  
check-unused-files    | dart run dart_code_linter:metrics check-unused-files lib   | Checks unused *.dart files.   |
| check-unused-l10n    | dart run dart_code_linter:metrics check-unused-l10n lib   | Checks unused localization in *.dart files.   |
| check-unused-code    | dart run dart_code_linter:metrics check-unused-code lib   | Checks unused code in *.dart files.   |

For additional help on any of the commands:

<Tabs>
  <TabItem value="dart" label="Dart" default>
  
```sh:
$ dart run dart_code_metrics:metrics --help <command>
```
</TabItem>
  <TabItem value="flutter" label="Flutter">

```sh
$ flutter pub run dart_code_metrics:metrics --help <command>
```
</TabItem>
</Tabs>


## Multi-package repositories usage
If you run a command from the root of a multi-package repository (a.k.a. monorepo), it'll pick up `analysis_options.yaml` files correctly.

Additionally, if you use [Melos](https://web.archive.org/web/20230214151411/https://pub.dev/packages/melos), you can add custom command to the melos.yaml.

```yaml:
metrics:
  run: |
    melos exec -c 1 --ignore="*example*" -- \
      flutter pub run dart_code_metrics:metrics analyze lib
  description: |
    Run `dart_code_metrics` in all packages.
     - Note: you can also rely on your IDEs Dart Analysis / Issues window.
```

## Calling DCM CLI from your own package with the linter configuration
If you have a separate package with all the linter and DCM configurations which is used by your other packages and you want to call DCM transitively add a bin folder with a Dart file, for example

```dart:
import 'package:dart_code_metrics/cli_runner.dart';

Future<void> main(List<String> args) async {
  await CliRunner().run(args);
}
```

After that you will be able to run DCM by running your package executable.

