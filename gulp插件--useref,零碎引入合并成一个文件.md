---
title: gulp插件--useref,零碎引入合并成一个文件。
date: 2017-05-26 14:22:41
tags: gulp
---
### 使用场景</br>
当js、css引用过多时,请求次数过多。</br>
### 如何使用 </br>
在html中添加标记

    <!-- build:<type> <path> <parameters> -->
    ... HTML Markup, list of script / link tags.
    <!-- endbuild -->`
Example

    <html>
    <head>
        <!-- build:css css/combined.css -->
        <link href="css/one.css" rel="stylesheet">
        <link href="css/two.css" rel="stylesheet">
        <!-- endbuild -->
    </head>
    <body>
        <!-- build:js scripts/combined.js -->
        <script type="text/javascript" src="scripts/one.js"></script> 
        <script type="text/javascript" src="scripts/two.js"></script> 
        <!-- endbuild -->
    </body>
    </html>
在gulp中注册task

    var gulp = require('gulp'),
    useref = require('gulp-useref');
    gulp.task('default', function () {
        return gulp.src('app/*.html')
            .pipe(useref())
            .pipe(gulp.dest('dist'));
    });
最好不要写在watch中,最后项目写完后执行这个task就好

执行gulp default,输出来的就是合并好的css和js了

    <html>
    <head>
        <link rel="stylesheet" href="css/combined.css"/>
    </head>
    <body>
        <script src="scripts/combined.js"></script> 
    </body>
    </html>