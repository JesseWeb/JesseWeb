---
title: gulp插件--Plumber,让出错的gulp keep Running。
date: 2017-05-27 09:49:00
tags: gulp
---
编译sass,babelJS的时候出问题了gulp直接报错中断watch了咋办?
Plumber来帮你

    $ yarn add gulp-plumber

安装好后,在易出错的task内，首个pipe执行plumber函数。
Example

    const gulp = require('gulp';)
    const gulpLoadPlugins = require('gulp-load-plugins');
    const $ = gulpLoadPlugins();
    gulp.task('css',()=>{
        return gulp.src('app/styles/*.scss')
            .pipe($.plumber())
            .pipe($.sass({
                style:'compressed'
            }))
            .pipe(gulp.dest('.tmp/styles'))
    })

    gulp.task('watch',()=>{
        gulp.watch('app/styles/*.scss',['css'])
    })
这样一来,即使你的sass文件有错误,watch也不会挂掉了;



