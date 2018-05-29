# gulp-bootlint-customization
[![npm version](https://badge.fury.io/js/gulp-bootlint.svg)](https://badge.fury.io/js/gulp-bootlint)
[![Build Status](https://travis-ci.org/tschortsch/gulp-bootlint.svg?branch=master)](https://travis-ci.org/tschortsch/gulp-bootlint)
[![dependencies Status](https://david-dm.org/tschortsch/gulp-bootlint/status.svg)](https://david-dm.org/tschortsch/gulp-bootlint)
[![devDependencies Status](https://david-dm.org/tschortsch/gulp-bootlint/dev-status.svg)](https://david-dm.org/tschortsch/gulp-bootlint?type=dev)

一个为[Bootlint-Customization](https://github.com/alex1221/bootlint-customization)gulp封装, 为Bootstrap项目开发的一个HTML的linter。

## 什么是 gulp-bootlint-customization?

gulp-bootlint-customization一个以gulp-bootlint为蓝本做的一个定制修改，首先非常感谢gulp-bootlint的制作者Jürg Hunziker。

## 第一步

如果您熟悉[gulp](http://gulpjs.com/) 只需使用以下命令从[npm](https://npmjs.org/package/gulp-bootlint)上安装插件:

```
npm install gulp-bootlint-customization --save-dev
```

否则，请首先查阅gulp[入门指南](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md#getting-started)。

## 创建一个新的gulp任务

在安装插件后，你可以在 `gulpfile.js` 文件中像这样，创建一个新的gulp任务：

```javascript
var gulp = require('gulp');
var bootlint  = require('gulp-bootlint-customization');

gulp.task('bootlint', function() {
    return gulp.src('./index.html')
        .pipe(bootlint());
});
```

## 选项

调用bootlint插件时，您可以将以下选项作为单个对象传递。

### options.stoponerror

* 类型: `Boolean`
* 默认: `false`

如果linted文件中有错误，则停止gulp task。

### options.stoponwarning

* 类型: `Boolean`
* 默认: `false`

如果linted文件中存在警告，则停止gulp task。。

### options.loglevel

* 类型: `String`
* 默认: `'error'`
* 选项: `'emergency'`, `'alert'`, `'critical'`, `'error'`, `'warning'`, `'notice'`, `'info'`, `'debug'`

定义应将哪些日志消息打印到 `stdout`.

### options.disabledIds

* 类型: `String[]`
* 默认: `[]`

显示忽略的[bootlint问题代码ID](https://github.com/alex1221/bootlint-customization/wiki)（as Strings）数组。

### options.issues

* 类型: `Array` of `LintWarning` and `LintError` objects
* 默认: `[]`

所有发现的问题 (对象类型 `LintWarning` 和 `LintError`) 都存储在此数组中.
您可以在执行该模块后访问并使用它们。

LintWarning和LintError这些类在这里描述https://github.com/alex1221/bootlint-customization#api%E6%96%87%E6%A1%A3。

### options.reportFn

* 类型: `Function`
* 参数:
  * `Object` **file** - 有linting错误的文件。
  * `Object` **lint** - Linting错误。
  * `Boolean` **isError** - 如果当前linting问题是Error，则为 true。
  * `Boolean` **isWarning** - 如果当前linting问题是Warning，则为 true。
  * `Object` **errorLocation** - 文件中的错误位置.

这个函数，它将把lint错误记录到控制台。只有在您想要决定如何输出lint错误时，请使用此方法。
如果需要，可以通过设置 `reportFn: false` 来关闭。
____
### options.summaryReportFn

* 类型: `Function`
* 参数:
  * `Object` **file** - 被linted的文件。
  * `Integer` **errorCount** - 文件中的错误总数。
  * `Integer` **warningCount** - 文件中的警告总数。

这个函数，它将把最后的lint错误/警告摘要记录到控制台。如果您想要自定义如何输出时，请使用此方法。
如果需要，可以通过设置 `summaryReportFn: false` 来关闭。

### 选项使用示例:

```javascript
var gulp = require('gulp');
var bootlint  = require('gulp-bootlint-customization');

gulp.task('bootlint', function() {
    var fileIssues = [];
    return gulp.src('./index.html')
        .pipe(bootlint({
            stoponerror: true,
            stoponwarning: true,
            loglevel: 'debug',
            disabledIds: ['W009', 'E007'],
            issues: fileIssues,
            reportFn: function(file, lint, isError, isWarning, errorLocation) {
                var message = (isError) ? "ERROR! - " : "WARN! - ";
                if (errorLocation) {
                    message += file.path + ' (line:' + (errorLocation.line + 1) + ', col:' + (errorLocation.column + 1) + ') [' + lint.id + '] ' + lint.message;
                } else {
                    message += file.path + ': ' + lint.id + ' ' + lint.message;
                }
                console.log(message);
            },
            summaryReportFn: function(file, errorCount, warningCount) {
                if (errorCount > 0 || warningCount > 0) {
                    console.log("please fix the " + errorCount + " errors and "+ warningCount + " warnings in " + file.path);
                } else {
                    console.log("No problems found in "+ file.path);
                }
            }
        }));
});
```

## 发布历史

### gulp-bootlint-customization发布历史

* 2018-05-25 - v1.0.1: 第一次发布。

### gulp-bootlint发布历史

* 2018-04-24 - v0.9.0: Replaced deprecated gulp-util (thanks to [TheDancingCode](https://github.com/TheDancingCode))
* 2017-02-01 - v0.8.1: Bumped dependency versions
* 2016-05-24 - v0.8.0: Possibility to provide array where found issues are stored
* 2016-04-12 - v0.7.2: Bumped dependency versions
* 2015-11-25 - v0.7.1: Updated Bootlint to v0.14.2
* 2015-11-16 - v0.7.0: Updated Bootlint to v0.14.1 / Added options to define reporters (thx @chrismbarr) / Bumped dependency versions
* 2015-07-28 - v0.6.4: Bumped dependency versions
* 2015-06-21 - v0.6.0: Added option to define log level
* 2015-06-18 - v0.5.1: Bumped dependency versions
* 2015-04-28 - v0.5.0: Added parameters to stop task on error or warning
* 2015-04-25 - v0.4.0: Updated Bootlint to v0.12.0 / Bumped other dependency versions
* 2015-02-24 - v0.3.0: Updated Bootlint to v0.11.0 / Bumped other dependency versions
* 2015-01-26 - v0.2.3: Updated Bootlint to v0.10.0
* 2015-01-07 - v0.2.2: Updated dependencies
* 2015-01-01 - v0.2.1: Code cleanup
* 2015-01-01 - v0.2.0: Updated Bootlint to v0.9.1
* 2015-01-01 - v0.1.1: Fail on linting errors
* 2014-11-23 - v0.1.0: First public release

## License

Copyright (c) 2016 Jürg Hunziker. Licensed under the MIT License.
