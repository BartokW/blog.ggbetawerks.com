---
title: Upload Image and Convert to Data URL in Angular
date: '2021-02-18 17:12:59'
header: 
  image: "/assets/images/angular.svg"
  teaser: "/assets/images/angular.svg"
tags:
- angular
- software-development
---

I have a little application that I wanted the user to be able to upload their own images, and be able to store them in a json format so that the configuration can be stored somewhere for example storing it in the browsers localstorage. &nbsp;To do this, I wanted to be able to load the image into a data url which is in a string format, making it easier to store. &nbsp;I'm using Angular, but the basic loading of an image is using the JavaScript FileReader class, and so with some minor tweaks you should be able to use with other JavaScript frameworks.

First I need a way to upload the image, this is done using the html input control.
```html
    <input 
    	type="file"
    	accept="image/png, image/jpeg"
    />
```
Now I need to be able to do something when the user chooses a file, for this I'll add an Angular function to handle the input control's on change event.
```typescript
    handleFileUpload(files: FileList){
    	// Just logging the selected files for now
    	console.log(files);
    }
```

```html
	<input
		type="file"
		accept="image/png, image/jpeg"
		(change)="handleFileInput($event.target.files)"
	/>
```
<figure class="kg-card kg-code-card"><figcaption>Added the change event handler</figcaption></figure>

To actually load the image into a data url which looks like this: `data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==` I will use the JavaScript FileReader class. &nbsp;This needs to have an onload callback set to do something with the file once it has been loaded.
```typescript
    handleFileUpload(files: FileList){
    	const fileReader = new FileReader();
    	// Set the onload callback to do something with the loaded file
    	fileReader.onload = (e) =>{
    		// just log the data url
    		console.log(fileReader.result.toString());
    	};
    	// Trigger the file load
    	fileReader.readAsDataURL(files.item(0));
    }
```
To display the image I will need to store the data url somewhere, as I want to be able to have several images uploaded. For this I will use an array of strings.
```typescript
imageList: string[] = [];

handleFileUpload(files: FileList){
	// A list of images to be displayed
	const fileReader = new FileReader();
    // Set the onload callback to do something with the loaded file
    fileReader.onload = (e) =&gt;{
    	// Add the newly loaded image data url to the imageList
    	this.imageList.push(fileReader.result.toString());
    };
    // Trigger the file load
    fileReader.readAsDataURL(files.item(0));
}
```
<figure class="kg-card kg-code-card"><figcaption>Changed the onload callback to push the image to an imageList array</figcaption></figure>

I just need to have an image tag for each image setting the source to the stored value in the array. &nbsp;So I can just iterate over the array using an ngFor, creating an img html element for each.

```html
<img *ngFor="let image of imageList" [src]="image" />
```
<figure class="kg-card kg-code-card"><figcaption>Simple angular for loop over each of the images in the imageList</figcaption></figure>

### Unsafe Url

There have been a few situations where I have run into an Angular error complaining about unsafe urls. This has been mostly when hard coding a data url in my code for testing purposes, but there is a way around it. &nbsp;You just need to use the Angular DomSanitizer to mark the url safe. However, that then returns a `SafeUrl` instead of a `string` which typescript will complain about when trying to add it to your imageList. &nbsp;I built this simple little method that takes a data url and marks it safe, and then converts back to a string which fixed the problem for me anytime it arose.
```typescript
    markDataUrlSafe(dataUrl: string): string {
    	// Marks the url safe
        const safeUrl: SafeUrl = this.domSanitizer.bypassSecurityTrustUrl(dataUrl);
        // Sanitizes the passed in value, returning a url string
        const url: string = this.domSanitizer.sanitize(SecurityContext.URL, safeUrl);
        return url;
      }
```
