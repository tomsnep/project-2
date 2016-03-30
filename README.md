# project-2 Part-up

### Image Optimalization with Gulp

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