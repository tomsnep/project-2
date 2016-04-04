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

## Accessibility

Tool: [Accessibility Developer Tools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb/related)  

###Meaningful images should not be used in element backgrounds
![meaningful images](/tests/Accessibility/ADT_meaningful_img.png)

If we talk about accessibility, the logo ```<figure>``` element is a problem. The ```<figure>``` is not meant to display a single logo, figure's should contain images that associated with a ```<article>``` or something like that. Besides that, if the background css fails there is not ```alt``` attribute to tell the user what the image is for. The ```alt``` attribute is very important for the accessibility, therefore an ```<img>``` tag should be used.

The logo images in ```partup:client-header``` and ```partup:client-footer``` should be a img tag.  
Instead of the ```<figure>``` there should be a ```<img>``` tag:  
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
###Language attribute html element
The language of the document is not identified. Identifying the language of the page allows screen readers to read the content in the appropriate language. It also facilitates automatic translation of content. Therefore it could be usefull to set the ```lang``` attribute.

```<html lang="en">```

###Redundant link in the footer

The ```footer.html``` contains a redundant link, this results in additional navigation and repetition for keyboard and screen reader users and can cause confusion for the user. That's why it is not desirable to have redundant links in the page.

```
<li><a href="{{ pathFor 'about' }}">{{_ 'footer-menu-company-about'}}</a></li>
<li><a href="{{ pathFor 'about' }}">{{_ 'footer-menu-company-community'}}</a></li>
```

###Semantic HTML

For accessibility semantic HTML is very important. At the moment, the HTML structure not completely built up semantic. The header structure is not quite right and there are many sections only used purely for styling. A section must contain a ```<header>``` or ```<h>``` element, therefore it is better to use a ```<div>``` as purely comes to styling. To make the HTML good accessibel the sections are replaced with a ```<div>```. Together with Sem Bakkum we analyzed the HTML source and made some adjustments, in this example we only changed the first part of the page:

```
<!DOCTYPE html>
<html>
	<head>
	<meta charset="UTF-8">
	<title>Part-up</title>
	</head>
​
	<body>
​
		<header> 
​
			<div>
				<a href="/">
					<h1><img alt="Part-up" src="images/logo.png" class="pu-brand"></h1>
				</a>
			</div>
​
			<nav>
				<ul class="pu-sub-nav pu-list pu-list-horizontal">
					<li>
						<a href="/discover" class="pu-button pu-button-header">Discover</a>
					</li>
					<li>
						<a href="/start" class="pu-button pu-button-header">Start a part-up</a>
					</li>
				</ul>
				<ul class="pu-sub-personal pu-list pu-list-horizontal">
					<li>
						<a href="/register" class="pu-button pu-button-header pu-button-header-desktop pu-button-header-nostripe" data-register>Register</a>
					</li>
					<li>
						<a href="/login" class="pu-button pu-button-header pu-button-header-desktop" data-login>Login</a>
					</li>
				</ul>
			</nav>
​
		</header>
​
		<main>
			<div>
​
				<h2>Your flexible workforce</h2>
​
				<p>
					Part-up gets you your best team for the job. Just share your dream or talent and we’ll help you find the right team! You are free to choose what you work on and who you work with. Together you decide on the activities and what each will contribute. You build your profile and show the world what you're good at!
				</p>
​
				<button class="pu-button pu-button-large" data-start-video>
					<i class="picon-caret-right"></i>
					Part-up in 96 seconds
				</button>
​
			</div>
			<div>
​
				<h2 class="pu-textline pu-textline-subtle">
	                <span>Keuze van de founders</span>
	            </h2>
​
	            <div>
		            <p class="pu-sub-box">
			        	"This is an english quote for an english partup"
			        </p>
​
			        <div>
			        	<ul class="cell pu-sub-partup-stats">
	                        <li>
	                            <strong>0</strong>
	                            <span>activities</span>
	                        </li>
	                        <li>
	                            <strong>0</strong>
	                            <span>supporters</span>
	                        </li>
	                        <li>
	                            <strong>254</strong>
	                            <span>days active</span>
	                        </li>
	                    </ul>
​
	                    <div class="cell last">
​
	                        <div class="pu-partupcircle pu-partupcircle-avatarsexpanded">
​
	                            <a href="/partups/organise-a-meteor-meetup-vGaxNojSerdizDPjb">
	                                <canvas class="pu-sub-radial" data-percent="12.557320955732038" width="195" height="195"></canvas>
	                                <img src="https://s3-eu-west-1.amazonaws.com/partup-development/360x360/images/5Jfp8RWF6DXLy8w45-5Jfp8RWF6DXLy8w45.png" class="pu-sub-inner" data-partup-tile-focuspoint="">
	                            </a>
​
	                            <ul class="pu-sub-avatar-list">   
	                                 <li style="-webkit-transform: translate3d(220px, 95px, 0); -moz-transform: translate3d(220px, 95px, 0); transform: translate3d(220px, 95px, 0); transition-delay: 0s;" data-hovercontainer="HoverContainer_upper" data-hovercontainer-context="a7qcp5RHnh5rfaeW9">
	                                    <a href="/profile/a7qcp5RHnh5rfaeW9" class="pu-avatar pu-avatar-partuptile">
	                                        <img src="https://s3-eu-west-1.amazonaws.com/partup-development/80x80/images/deH7FMJjaSmxnqHjr-deH7FMJjaSmxnqHjr.png" alt="" class="pu-sub-image">
	                                    </a>
	                                </li>
	                            </ul>
	                            
	                        </div>
	        
	                    </div>
​
	                    <section class="pu-sub-description">
​
		                    <h3><a href="/partups/organise-a-meteor-meetup-vGaxNojSerdizDPjb">Organise a Meteor Meetup</a></h3>
​
		                    <p>organise a meetup at lifely</p>
​
		                </section>
​
		        	</div>
		        </div>
		    </div>
​
	</body>
​
</html>
```

