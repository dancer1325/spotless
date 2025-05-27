![](_images/spotless_logo.png)


[![Gradle Plugin](https://img.shields.io/gradle-plugin-portal/v/com.diffplug.spotless?color=blue&label=gradle%20plugin)](plugin-gradle)
[![Maven Plugin](https://img.shields.io/maven-central/v/com.diffplug.spotless/spotless-maven-plugin?color=blue&label=maven%20plugin)](plugin-maven)
[![SBT Plugin](https://img.shields.io/badge/sbt%20plugin-0.1.3-blue)](https://github.com/moznion/sbt-spotless)

* allows
  * formatting
* support
  * antlr
  * c
  * c#
  * c++
  * css
  * flow
  * graphql
  * groovy
  * html
  * java
  * javascript
  * json
  * jsx
  * kotlin
  * less
  * license headers
  * markdown
  * objective-c
  * protobuf
  * python
  * scala
  * scss
  * shell
  * sql
  * typeScript
  * vue
  * yaml
  * gradle
  * maven
  * sbt

## [❇️ Spotless for Gradle](plugin-gradle) (with integrations for [VS Code](https://marketplace.visualstudio.com/items?itemName=richardwillis.vscode-spotless-gradle) and [IntelliJ](https://plugins.jetbrains.com/plugin/18321-spotless-gradle))

```console
user@machine repo % ./gradlew build
:spotlessJavaCheck FAILED
  The following files had format violations:
  src\main\java\com\diffplug\gradle\spotless\FormatExtension.java
    -\t\t····if·(targets.length·==·0)·{
    +\t\tif·(targets.length·==·0)·{
  Run './gradlew spotlessApply' to fix these violations.
user@machine repo % ./gradlew spotlessApply
:spotlessApply
BUILD SUCCESSFUL
user@machine repo % ./gradlew build
BUILD SUCCESSFUL
```

## [❇️ Spotless for Maven](plugin-maven)

```console
user@machine repo % mvn spotless:check
[ERROR]  > The following files had format violations:
[ERROR]  src\main\java\com\diffplug\gradle\spotless\FormatExtension.java
[ERROR]    -\t\t····if·(targets.length·==·0)·{
[ERROR]    +\t\tif·(targets.length·==·0)·{
[ERROR]  Run 'mvn spotless:apply' to fix these violations.
user@machine repo % mvn spotless:apply
[INFO] BUILD SUCCESS
user@machine repo % mvn spotless:check
[INFO] BUILD SUCCESS
```

## [❇️ Spotless for SBT (external for now)](https://github.com/moznion/sbt-spotless)
## [Other build systems](CONTRIBUTING.md#how-to-add-a-new-plugin-for-a-build-system)

## How it works (for potential contributors)

* code formatter
  * == tool / 
    * find formatting errors
    * fix formatting errors
  * == 👀`Function<String, String>` 👀/
    * FROM potentially UNFORMATTED input -> returns a FORMATTED version
    * OUR FOCUS -- to -- build
    * lots of integration work -- done by Spotless --
      * [newlines](https://github.com/diffplug/spotless/tree/main/plugin-gradle#line-endings-and-encodings-invisible-stuff)
      * character [encodings](https://github.com/diffplug/spotless/blob/08340a11566cdf56ecf50dbd4d557ed84a70a502/testlib/src/test/java/com/diffplug/spotless/EncodingErrorMsgTest.java#L34-L38)
      * [idempotency](https://github.com/diffplug/spotless/blob/main/PADDEDCELL.md)
      * git [ratcheting](https://github.com/diffplug/spotless/tree/main/plugin-gradle#ratchet)
      * build-system integration
       `Function<String, String>`

## Current feature matrix

* TODO:
<!---freshmark matrix
function lib(className)   { return '| [`' + className + '`](lib/src/main/java/com/diffplug/spotless/' + className.replaceAll('\\.', '/') + '.java) | ' }
function extra(className) { return '| [`' + className + '`](lib-extra/src/main/java/com/diffplug/spotless/extra/' + className.replaceAll('\\.', '/') + '.java) | ' }

//                                               | GRADLE        | MAVEN        | SBT          | (new)   |
output = [
'| Feature / FormatterStep                       | [gradle](plugin-gradle/README.md) | [maven](plugin-maven/README.md) | [sbt](https://github.com/moznion/sbt-spotless) | [(Your build tool here)](CONTRIBUTING.md#how-to-add-a-new-plugin-for-a-build-system) |',
'| --------------------------------------------- | ------------- | ------------ | ------------ | --------|',
'| Automatic [idempotency safeguard](PADDEDCELL.md) | {{yes}}    | {{yes}}      | {{yes}}      | {{no}}  |',
'| Misconfigured [encoding safeguard](https://github.com/diffplug/spotless/blob/08340a11566cdf56ecf50dbd4d557ed84a70a502/testlib/src/test/java/com/diffplug/spotless/EncodingErrorMsgTest.java#L34-L38) | {{yes}}    | {{yes}}      | {{yes}}       | {{no}}  |',
'| Toggle with [`spotless:off` and `spotless:on`](plugin-gradle/#spotlessoff-and-spotlesson) | {{yes}}    | {{yes}}      | {{no}}       | {{no}}  |',
'| [Ratchet from](https://github.com/diffplug/spotless/tree/main/plugin-gradle#ratchet) `origin/main` or other git ref | {{yes}}    | {{yes}}      | {{no}}       | {{no}}  |',
'| Define [line endings using git](https://github.com/diffplug/spotless/tree/main/plugin-gradle#line-endings-and-encodings-invisible-stuff) | {{yes}}    | {{yes}}      | {{yes}}       | {{no}}  |',
'| Fast incremental format and up-to-date check   | {{yes}}      | {{yes}}      | {{no}}       | {{no}}  |',
'| Fast format on fresh checkout using buildcache | {{yes}}      | {{no}}       | {{no}}       | {{no}}  |',
lib('generic.EndWithNewlineStep')                +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('generic.IndentStep')                        +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('generic.Jsr223Step')                        +'{{no}}        | {{yes}}      | {{no}}       | {{no}}  |',
lib('generic.LicenseHeaderStep')                 +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
lib('generic.NativeCmdStep')                     +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('generic.ReplaceRegexStep')                  +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('generic.ReplaceStep')                       +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('generic.TrimTrailingWhitespaceStep')        +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('antlr4.Antlr4FormatterStep')                +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('biome.BiomeStep')                           +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('cpp.ClangFormatStep')                       +'{{yes}}       | {{no}}       | {{no}}       | {{no}}  |',
extra('cpp.EclipseFormatterStep')                +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
lib('go.GofmtFormatStep')                        +'{{yes}}       | {{no}}       | {{no}}       | {{no}}  |',
lib('gherkin.GherkinUtilsStep')                  +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
extra('groovy.GrEclipseFormatterStep')           +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
lib('java.GoogleJavaFormatStep')                 +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
lib('java.ImportOrderStep')                      +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
lib('java.PalantirJavaFormatStep')               +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('java.RemoveUnusedImportsStep')              +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
extra('java.EclipseJdtFormatterStep')            +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
lib('java.FormatAnnotationsStep')                +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('java.CleanthatJavaStep')                    +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('json.gson.GsonStep')                        +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('json.JacksonJsonStep')                      +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('json.JsonSimpleStep')                       +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('json.JsonPatchStep')                        +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('kotlin.KtLintStep')                         +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
lib('kotlin.KtfmtStep')                          +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('kotlin.DiktatStep')                         +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('markdown.FreshMarkStep')                    +'{{yes}}       | {{no}}       | {{no}}       | {{no}}  |',
lib('markdown.FlexmarkStep')                     +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('npm.EslintFormatterStep')                   +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('npm.PrettierFormatterStep')                 +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('npm.TsFmtFormatterStep')                    +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('pom.SortPomStep')                           +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('protobuf.BufStep')                          +'{{yes}}       | {{no}}       | {{no}}       | {{no}}  |',
lib('python.BlackStep')                          +'{{yes}}       | {{no}}       | {{no}}       | {{no}}  |',
lib('rdf.RdfFormatterStep')                      +'{{no}}        | {{yes}}      | {{no}}       | {{no}}  |',
lib('scala.ScalaFmtStep')                        +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
lib('shell.ShfmtStep')                           +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('sql.DBeaverSQLFormatterStep')               +'{{yes}}       | {{yes}}      | {{yes}}      | {{no}}  |',
extra('wtp.EclipseWtpFormatterStep')             +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
lib('yaml.JacksonYamlStep')                      +'{{yes}}       | {{yes}}      | {{no}}       | {{no}}  |',
'| [(Your FormatterStep here)](CONTRIBUTING.md#how-to-add-a-new-formatterstep) | {{no}} | {{no}} | {{no}} | {{no}} |',
].join('\n');
-->
| Feature / FormatterStep                       | [gradle](plugin-gradle/README.md) | [maven](plugin-maven/README.md) | [sbt](https://github.com/moznion/sbt-spotless) | [(Your build tool here)](CONTRIBUTING.md#how-to-add-a-new-plugin-for-a-build-system) |
| --------------------------------------------- | ------------- | ------------ | ------------ | --------|
| Automatic [idempotency safeguard](PADDEDCELL.md) | :+1:    | :+1:      | :+1:      | :white_large_square:  |
| Misconfigured [encoding safeguard](https://github.com/diffplug/spotless/blob/08340a11566cdf56ecf50dbd4d557ed84a70a502/testlib/src/test/java/com/diffplug/spotless/EncodingErrorMsgTest.java#L34-L38) | :+1:    | :+1:      | :+1:       | :white_large_square:  |
| Toggle with [`spotless:off` and `spotless:on`](plugin-gradle/#spotlessoff-and-spotlesson) | :+1:    | :+1:      | :white_large_square:       | :white_large_square:  |
| [Ratchet from](https://github.com/diffplug/spotless/tree/main/plugin-gradle#ratchet) `origin/main` or other git ref | :+1:    | :+1:      | :white_large_square:       | :white_large_square:  |
| Define [line endings using git](https://github.com/diffplug/spotless/tree/main/plugin-gradle#line-endings-and-encodings-invisible-stuff) | :+1:    | :+1:      | :+1:       | :white_large_square:  |
| Fast incremental format and up-to-date check   | :+1:      | :+1:      | :white_large_square:       | :white_large_square:  |
| Fast format on fresh checkout using buildcache | :+1:      | :white_large_square:       | :white_large_square:       | :white_large_square:  |
| [`generic.EndWithNewlineStep`](lib/src/main/java/com/diffplug/spotless/generic/EndWithNewlineStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`generic.IndentStep`](lib/src/main/java/com/diffplug/spotless/generic/IndentStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`generic.Jsr223Step`](lib/src/main/java/com/diffplug/spotless/generic/Jsr223Step.java) | :white_large_square:        | :+1:      | :white_large_square:       | :white_large_square:  |
| [`generic.LicenseHeaderStep`](lib/src/main/java/com/diffplug/spotless/generic/LicenseHeaderStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`generic.NativeCmdStep`](lib/src/main/java/com/diffplug/spotless/generic/NativeCmdStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`generic.ReplaceRegexStep`](lib/src/main/java/com/diffplug/spotless/generic/ReplaceRegexStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`generic.ReplaceStep`](lib/src/main/java/com/diffplug/spotless/generic/ReplaceStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`generic.TrimTrailingWhitespaceStep`](lib/src/main/java/com/diffplug/spotless/generic/TrimTrailingWhitespaceStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`antlr4.Antlr4FormatterStep`](lib/src/main/java/com/diffplug/spotless/antlr4/Antlr4FormatterStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`biome.BiomeStep`](lib/src/main/java/com/diffplug/spotless/biome/BiomeStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`cpp.ClangFormatStep`](lib/src/main/java/com/diffplug/spotless/cpp/ClangFormatStep.java) | :+1:       | :white_large_square:       | :white_large_square:       | :white_large_square:  |
| [`cpp.EclipseFormatterStep`](lib-extra/src/main/java/com/diffplug/spotless/extra/cpp/EclipseFormatterStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`go.GofmtFormatStep`](lib/src/main/java/com/diffplug/spotless/go/GofmtFormatStep.java) | :+1:       | :white_large_square:       | :white_large_square:       | :white_large_square:  |
| [`gherkin.GherkinUtilsStep`](lib/src/main/java/com/diffplug/spotless/gherkin/GherkinUtilsStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`groovy.GrEclipseFormatterStep`](lib-extra/src/main/java/com/diffplug/spotless/extra/groovy/GrEclipseFormatterStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`java.GoogleJavaFormatStep`](lib/src/main/java/com/diffplug/spotless/java/GoogleJavaFormatStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`java.ImportOrderStep`](lib/src/main/java/com/diffplug/spotless/java/ImportOrderStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`java.PalantirJavaFormatStep`](lib/src/main/java/com/diffplug/spotless/java/PalantirJavaFormatStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`java.RemoveUnusedImportsStep`](lib/src/main/java/com/diffplug/spotless/java/RemoveUnusedImportsStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`java.EclipseJdtFormatterStep`](lib-extra/src/main/java/com/diffplug/spotless/extra/java/EclipseJdtFormatterStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`java.FormatAnnotationsStep`](lib/src/main/java/com/diffplug/spotless/java/FormatAnnotationsStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`java.CleanthatJavaStep`](lib/src/main/java/com/diffplug/spotless/java/CleanthatJavaStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`json.gson.GsonStep`](lib/src/main/java/com/diffplug/spotless/json/gson/GsonStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`json.JacksonJsonStep`](lib/src/main/java/com/diffplug/spotless/json/JacksonJsonStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`json.JsonSimpleStep`](lib/src/main/java/com/diffplug/spotless/json/JsonSimpleStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`json.JsonPatchStep`](lib/src/main/java/com/diffplug/spotless/json/JsonPatchStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`kotlin.KtLintStep`](lib/src/main/java/com/diffplug/spotless/kotlin/KtLintStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`kotlin.KtfmtStep`](lib/src/main/java/com/diffplug/spotless/kotlin/KtfmtStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`kotlin.DiktatStep`](lib/src/main/java/com/diffplug/spotless/kotlin/DiktatStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`markdown.FreshMarkStep`](lib/src/main/java/com/diffplug/spotless/markdown/FreshMarkStep.java) | :+1:       | :white_large_square:       | :white_large_square:       | :white_large_square:  |
| [`markdown.FlexmarkStep`](lib/src/main/java/com/diffplug/spotless/markdown/FlexmarkStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`npm.EslintFormatterStep`](lib/src/main/java/com/diffplug/spotless/npm/EslintFormatterStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`npm.PrettierFormatterStep`](lib/src/main/java/com/diffplug/spotless/npm/PrettierFormatterStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`npm.TsFmtFormatterStep`](lib/src/main/java/com/diffplug/spotless/npm/TsFmtFormatterStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`pom.SortPomStep`](lib/src/main/java/com/diffplug/spotless/pom/SortPomStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`protobuf.BufStep`](lib/src/main/java/com/diffplug/spotless/protobuf/BufStep.java) | :+1:       | :white_large_square:       | :white_large_square:       | :white_large_square:  |
| [`python.BlackStep`](lib/src/main/java/com/diffplug/spotless/python/BlackStep.java) | :+1:       | :white_large_square:       | :white_large_square:       | :white_large_square:  |
| [`rdf.RdfFormatterStep`](lib/src/main/java/com/diffplug/spotless/rdf/RdfFormatterStep.java) | :white_large_square:        | :+1:      | :white_large_square:       | :white_large_square:  |
| [`scala.ScalaFmtStep`](lib/src/main/java/com/diffplug/spotless/scala/ScalaFmtStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`shell.ShfmtStep`](lib/src/main/java/com/diffplug/spotless/shell/ShfmtStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`sql.DBeaverSQLFormatterStep`](lib/src/main/java/com/diffplug/spotless/sql/DBeaverSQLFormatterStep.java) | :+1:       | :+1:      | :+1:      | :white_large_square:  |
| [`wtp.EclipseWtpFormatterStep`](lib-extra/src/main/java/com/diffplug/spotless/extra/wtp/EclipseWtpFormatterStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [`yaml.JacksonYamlStep`](lib/src/main/java/com/diffplug/spotless/yaml/JacksonYamlStep.java) | :+1:       | :+1:      | :white_large_square:       | :white_large_square:  |
| [(Your FormatterStep here)](CONTRIBUTING.md#how-to-add-a-new-formatterstep) | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
<!---freshmark /matrix -->

### Why are there empty squares?

Many projects get harder to work on as they get bigger. Spotless is easier to work on than ever, and one of the reasons why is that we don't require contributors to "fill the matrix". If you want to [add Bazel support](https://github.com/diffplug/spotless/issues/76), we'd happily accept the PR even if it only supports the one formatter you use. And if you want to add FooFormatter support, we'll happily accept the PR even if it only supports the one build system you use.

Once someone has filled in one square of the formatter/build system matrix, it's easy for interested parties to fill in any empty squares, since you'll now have a working example for every piece needed.
