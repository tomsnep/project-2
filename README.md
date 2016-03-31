# project-2 Part-up

## Image Optimalization with Gulp

I placed a new ```gulpfile.js``` in the root of the application. The gulpfile includes a single task to optimize the images with ```imagemin```:

```
var gulp = require('gulp');
var imagemin = require('gulp-imagemin');
var pngquant = require('imagemin-pngquant');

// Image Optimalisation 
gulp.task('imgopt', function() {
	gulp.src('./app/public/images/*')
		.pipe(imagemin({
			progressive: true,
			//quality stands instructs pngquant to use the least amount of colors needed to meet or the maximum quality. If the result is below the given quality, the image won't be saved
			use: [pngquant({quality: '65-80'})]
		}))
		.pipe(gulp.dest('./app/public/images'));
});
 
// Watch Files For Changes
gulp.task('watch', function() {
    gulp.watch('./app/public/images/*', ['imgopt']);
});

// Default Task
gulp.task('default', ['imgopt']);
```

There was 640.29kB (44.4%) saved:  
![CLI_screenshot](/tests/Gulp_Image_optimalization/CLI _screenshot.png)

I decided to not upload the timeline results as they are very unreliable in my opinion. When i tested the timeline after the image optimalization the website rendered 3s slower, 5 minutes later it rendered 2s faster... Not very trustworthy.

## Accesibility

Tool: [Accessibility Developer Tools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb/related)  

###Meaningful images should not be used in element backgrounds
![meaningful images](/tests/ADT_meaningful_img.png)

The logo images in ```partup:client-header``` and ```partup:client-footer``` should be a img tag.  
Instead of the ```<figure>``` there should be a ```<img>``` tag  
```
<template name="Header_logo">
    <h1>
        <span>{{_ 'header-title' }}</span>
        <img src="/images/logo.png" class="pu-brand" alt="Part-up logo">
        <!-- <figure class="pu-brand"></figure> -->
    </h1>
</template>
```
and this:  
```
<template name="Footer">
    <footer class="pu-footer {{# unless partupResponsive }}pu-footer-desktop{{/ unless }}">
        <div class="pu-sub-footer">
            <section class="pu-sub-description">
	            <img src="/images/logo-footer.png" class="pu-brand" alt="Part-up logo">
                <!-- <figure class="pu-brand pu-brand-footer"></figure> -->
				...........
```

Therefore the css also needs a little adjustment the background needs to be removed:   
```
.pu-brand
    font-family: $font-secondary
    <!-- background-image: url(/images/logo.png) -->
    background-size: contain
    background-position: center
    background-repeat: no-repeat
    width: 94px
    height: 26px
    background-color: $color-primary
    box-shadow: 0px 0px 5px 4px $color-primary

    h1
        display: none

    &-footer
        <!-- background-image: url(/images/logo-footer.png) -->
        width: 118px
        height: 34px
        box-shadow: none
        background-color: transparent
```


