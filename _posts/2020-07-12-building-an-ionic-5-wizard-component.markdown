---
title: Building an Ionic 5 Wizard Component in Angular
date: '2020-07-12 20:10:00'
header: 
  image: "/assets/images/ionic-angular-copy.svg"
tags:
- ionic
- angular
- software-development
---

Working on a project, I came across the need for a wizard style dialog, so I did the normal thing, doing a search and finding a few options for older versions of Ionic (1 or 2) and otherwise just finding suggestions to use the Slides component. &nbsp;So here's how I implemented an Ionic wizard modal component using the Ionic Slides component.

Source code is available on Github: [https://github.com/BartokW/ion-wizard/tree/modal-wizard](https://github.com/BartokW/ion-wizard/tree/modal-wizard)

Start by following the Ionic [Getting Started guide](https://ionicframework.com/getting-started#install), but use the blank template instead of the tabs (which is currently suggested on the page).

As we want this to be a modal, we should start with opening the modal, but the Ionic modal, requires you to pass a component in, so we need to create a component that will house the steps of our wizard. Use `ionic generate component` and pick a name, in my case I used `wizard`.

Wherever you want the button to open the modal you will need to inject a `ModalController` in the constructor, as well as provide a method that will create and present the modal, as is being done in `presentModal()`. And then in the template, add something that will trigger the `presentModal()` method, I used a paragraph tag, but a button would be more standard. &nbsp;Note that when creating the modal, we're passing in the `WizardComponent` as that's what we want to display in our new modal.

### home.page.ts
```typescript
    import { Component } from '@angular/core';
    import { ModalController } from '@ionic/angular';
    import { WizardComponent } from '../wizard/wizard.component';
    
    @Component({
      selector: 'app-home',
      templateUrl: 'home.page.html',
      styleUrls: ['home.page.scss'],
    })
    export class HomePage {
      constructor(private modalController: ModalController) {}
    
      async presentModal() {
        const modal = await this.modalController.create({
          component: WizardComponent,
          swipeToClose: false,
          backdropDismiss: false,
          showBackdrop: true,
        });
    
        return await modal.present();
      }
    }
```

For the modal options, I wanted the background darkened, as well as didn't want to be able to easily dismiss it, which is why those options were specified.

### home.page.html
```html
    ...
        <p (click)="presentModal()" style="cursor: pointer;">
          Click here for modal wizard
        </p>
    ...
```

Now that we have a model, it's time to start working on the wizard portion. &nbsp;First step is to add the `ion-slides` component. &nbsp;This is pretty much a copy from the Ionic documentation.

### wizard.component.ts
```typescript
    import {
      Component,
      OnInit,
    } from '@angular/core';
    
    @Component({
      selector: 'app-wizard',
      templateUrl: './wizard.component.html',
      styleUrls: ['./wizard.component.scss'],
    })
    export class WizardComponent implements OnInit {
      slideOpts = {
        initialSlide: 0,
        speed: 400,
      };
    
      constructor() {}
    
      ngOnInit() {}
    
    }
```    

### wizard.component.html
```html
    <ion-content>
      <ion-slides pager="true" [options]="slideOpts">
        <ion-slide>
          <h1>Slide 1</h1>
        </ion-slide>
        <ion-slide>
          <h1>Slide 2</h1>
        </ion-slide>
        <ion-slide>
          <h1>Slide 3</h1>
        </ion-slide>
      </ion-slides>
    </ion-content>
```

### wizard.component.scss
```css
    ion-slides {
      height: 100%;
    }
```

As we're wanting this to be a wizard, we want to disable swiping between each of the slides, as we want Previous and Next buttons for moving between the slides. &nbsp;The first way of disabling the swiping that I found used the api, and called `.lockSwipes(true)` which worked, but requires unlocking and relocking each time you navigate to a new page. I found an option that will disable swiping, but not lock the navigation &nbsp;via api. &nbsp;Add `allowTouchMove: false` to the `slideOpts` object.

```javascript
    slideOpts = {
        initialSlide: 0,
        speed: 400,
        allowTouchMove: false,
    };
```

Next we're going to want to get access to the slides api so that we can programmatically switch slides. This is done by importing `ViewChild` from `@angular/core` and `IonSlides` from `@ionic/angular` and then adding this line inside of your wizard component.

```typescript
    ...
    @ViewChild(IonSlides) slides: IonSlides;
    ...
```

Now lets add some methods for changing the slides.
```typescript
    ...
    prev() {
      this.slides.slidePrev();
    }
    
    next() {
      this.slides.slideNext();
    }
    ...
```

And in the template file add a toolbar and some buttons.
```html
    ...
    <ion-toolbar>
      <ion-buttons slot="start">
        <ion-button (click)="prev()">Previous</ion-button>
      </ion-buttons>
    
      <ion-buttons slot="end">
        <ion-button (click)="next()">Next</ion-button>
      </ion-buttons>
    </ion-toolbar>
```
As for dismissing the modal, that goes back to the Ionic documentation, where we need to inject the `ModalController` into the constructor, and add a function to dismiss, and then hook it up to a button in the template.
```typescript
    ...
    constructor(private modalController: ModalController) {}
    
    dismiss() {
      this.modalController.dismiss({});
    }
    ...
```

```html
    <ion-button (click)="dismiss()">Finish</ion-button>
```
