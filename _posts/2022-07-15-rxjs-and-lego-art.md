---
title: RxJS and LEGO<sup>&reg;</sup> Art
date: '2022-07-15 14:05:00'
header:
 image: "/assets/images/rxjs-lego-art/angular-mosaic.png"
 teaser: "/assets/images/rxjs-lego-art/angular-mosaic.png"
tags:
- angular
- software-development
- lego
---
I recently gave a lightning talk (short talk) at the [Angular Community Meetup](https://angularcommunity.net/) talking about using RxJS in making LEGO<sup>&reg;</sup> mosaics

## Plural of LEGO<sup>&reg;</sup>
Any discussion of LEGO<sup>&reg;</sup> must include the most important part, and pet peeve of mine, the plural form. According to the LEGO Group, the plural should be Lego Bricks, or Lego Elements, but never Legos.
![](/assets/images/rxjs-lego-art/lego-tweet.png)
The adult fan community does accept Lego as the plural, but never Legos.

## What is LEGO<sup>&reg;</sup> Art
Art can be anything, but when it comes to Lego the two primary forms I think of are sculptures and mosaics. A mosaic being an image made out of something else, in this case an image made out of Lego pieces.

[Examples of items I've built and or designed](https://ggbrickwerks.art/)

## What is RxJS
![](/assets/images/rxjs-lego-art/rxjs.ico)

Definition taken from  [https://rxjs.dev/](https://rxjs.dev/): RxJS is a library for reactive programming using Observables, to make it easier to compose asynchronous or callback-based code.

My simplification: RxJS provides a way of reacting to events, and flowing data through a set of operators, which then transform the data, without having to setup a complex set of callback functions.


## Mosaic Steps
![](/assets/images/rxjs-lego-art/process.png)

The basic steps to converting an image to a mosaic is to crop the image to the desired aspect ratio, and target area. Then we resize the cropped image to the desired mosaic size, targeting 1 pixel per stud (piece), and finally adjust the colours to the closest match of actual Lego piece colours.

In order to do this, we have three inputs, ignoring the initial image selection. We need to know the desired mosaic size specified in studs, which provides the aspect ratio when cropping the image. Then we need the cropped image, and the available colours.

## Code Examples
The code is available on [GitHub](https://github.com/BartokW/rxjs-lego) and can be run on [StackBlitz](https://stackblitz.com/edit/rxjs-lego?file=src%2Fapp%2Fapp.component.ts).

I'm using a third-party component for selecting the crop area on an image, and so the first step indicated previously can be skipped. 
In the code snippet below, there's two observables, targetDimensions$ and croppedImageDataSubject$ and we want to run code anytime either of those changes. In order to do that, using the RxJS function combineLatest will take an array of observables and create a new observable that will emit a value if either of the sources change. In this case, I've taken the target mosaic dimensions and the cropped image, and pipe that through a custom RxJS operator to resize the image, and it then returns the resized images as a data url.

{% highlight typescript %}
//Step 1 Resize cropped image
this.resizedImageDataURL$ = combineLatest([
    this.targetDimensions$,
    this.croppedImageDataSubject$,
]).pipe(this.resizeImage());
{% endhighlight %}

The next step takes the resized image, and the currently selected colours and pipes it through another custom operator that does the colour replacement, returning both a data url, and a list of points with colour information.

{% highlight typescript %}
//Step 2 Do colour replacement
let mosaicData$ = combineLatest([
    this.resizedImageDataURL$,
    this.selectedColorsSubject$,
]).pipe(this.replaceColours());
{% endhighlight %}

Next we use the map operator to split out the data url and the point array which can be then be used to display the colour replaced image.

{% highlight typescript %}
// Extract the Color Replaced Image
this.mosaicImageDataURL$ = mosaicData$.pipe(
    map(([image, points]: [string, ColoredPoint[]]) => {
    return image;
    })
);

// Extract the Coloured points for generating the mosaic image
this.mosaicPoints$ = mosaicData$.pipe(
    map(([image, points]: [string, ColoredPoint[]]) => {
    return points;
    })
);
{% endhighlight %}

Based on another previous talk at the Angular Community Meetup by Mike Ryan, I took inspiration from his talk and [sample code](https://github.com/MikeRyanDev/reactive-charts-angular-meetup-2021) for using angular to generate an svg image, and used the array of points and colour information to generate a mosaic that looks approximately like what a built mosaic would look like.

