---
title: gulp插件--再也不引gulp包-gulp-load-plugins。
date: 2017-05-26 15:22:45
tags: gulp
---
是不是觉得写gulp的时候,开通总是要require各种插件,well现在你只需要require一个gulp和gulp-load-plugins就行。

    $ yarn add gulp-load-plugins

load-plugins 能够自动追踪你的packge.json文件里的gulp依赖。

所以只要是npm的gulp包必须加上--save-dev
如果用yarn,直接add就行了(推荐使用yarn,贼快)

在gulpfile.json中引入load-plugins

    var gulp = require('gulp');
    var gulpLoadPlugins = require('gulp-load-plugins');
    var $ = gulpLoadPlugins();
这时候,你的所有包,都已经被存放在了$下

比如说你现在要在一个task中使用别的gulp插件

    gulp.task('default',()=>{
        return gulp.src('js/*.js')
            .pipe($.jshint())
            .pipe($.jshint.reporter('default'))
            .pipe($.uglify())
            .pipe($.concat('app.js'))
            .pipe(gulp.dest('dist'));
    })
是不是很方便??