---
layout: post
title: Angular 2 and D3
---

### Ground Rules
Alright, so this is my first ever 'technical' blog post. I'm writing, really, to solidify my own learning. So please, please, please correct me if you see an error or an obviously better(however subjective that may be) way of doing things.
The first big thing on my plate is figuring out D3. I have a hankering to learn something about data visualization, so I'm going to bite the bullet and stack Angular 2 and D3 and see if I can't get this ball rolling. My plan is to document as much minutiae and thought process as possible. I'm hoping this will help the other newbs, like me, out there. I am a new, so I think like a newb, so I hope I can write in a way that will help other newbs. Erm, these posts probably won't go in any particular order. AAAAAND, I probably won't go over configuration much. That varies so widely it's almost pointless. But if you're stuck at this point, DM me, and I'll help if I can. Okay, ground rules set.

## D3 Selectors in Angular 2
The first and most basic thing I started playing with was the D3 selector. It's handled really much the same way it is in Angular 1 with just a few small differences. There are probably some alternate ways to do this, so again, please speak up if I've gotten this wrong. I did learn this from a couple of well-known authors, so I hope I'm on solid ground.

### Constructor vs. ngOnInit vs. OnView
If y'all are like me and coming from a strictly Javascript background with little to no true experience in true OOP (object-oriented programming), then the idea of a constructor will be a little new. But don't be afraid. I started to feel like a grown up developer when I learned about them and the structure they could help me infuse into my code. I have gravitated to TypeScript much more quickly than I thought I would. But I digress...
If you are setting up a page to accept a D3 visualization, you will need to select the HTML element first that will contain that visualization. I have seen this handled three ways in the tutorials and books that I followed: one can place the selection in the constructor or place it under ngOnInit or in the OnView hook.
#### Constructor
A constructor is not specific to Angular 2. It can be used in Javascript, TypeScript and just about any other language. A constructor is just a way to set up an object so that object can be used repeatedly. When you instantiate the object, it will be set up with the attributes that you declare under the constructor block. How does this differ from an object literal one might see in Javascript? It's not really except that an object literal is meant to be used just-in-time or on the fly and probably should only be used in that one instance. Though it could be reused, this isn't a great practice. Constructors are great for setting attributes but it's probably best to save the action for ngOnInit. I played with making the D3 selection in the constructor but I didn't want to couple the constructor to an actual HTML element so tightly. For me, it made more sense to make the D3 selection in ngOnInit.
Though I said constructors are not specific to Angular 2, they are used when creating a component in Angular 2, so you will see it included in the code snippet below.
#### ngOnInit
This is a beast specific to Angular 2. ngOnInit is considered a lifecyle hook in Angular 2. It will be called shortly after the component is created.[ The Angular documentation is actually really clear on the subject and I highly recommend taking a look at it if you are at all curious.](https://angular.io/docs/ts/latest/guide/lifecycle-hooks.html)

So going along with this idea that we sort of 'set up' an object in a constructor, hang some attributes on it and then act on that object in ngOnInit, this is how I ended up selecting the HTML element into which I would display the D3 visualization.

````

import { Component, ElementRef, OnInit } from '@angular/core'; //Note that we have to inject OnInit here.
import * as D3 from 'd3'; //imports the D3 library for use. This is key. And this format only works for D3 v. 4 if you have the corret Typings installed.

@Component({
    selector: 'd3-visualization', //this is NOT the selector for D3. This tells Angular where to build the component.
    template: require('./d3-bar-graph.component.html')
})

export class D3Component {
    host;
    baseNode;

    constructor(public _element: ElementRef) {  //this is the constructor I was babbling about earlier. Notice that I have elected NOT to include anything here. HOWEVER, I did set a public variable '_element' based on ElementRef, which we injected above.
    }

    ngOnInit() {
        this.host = D3.select(this._element.nativeElement); //if you have used D3, this will look familiar, except for the nativeElement part. Check the console.log if you are curious what is being selected here. 
        console.log('host init', this.host.nodes()); //.nodes() is a D3 function that will display the actual element selected.
        this.baseNode = this.host.select('#scatterplot'); //I'm not showing the HTML here but basically I have a div with an id of scatterplot, and that is what I'm selecting. It is possible to use just about any query selector here. D3 provides a select and a selectAll.
        console.log('baseNode', baseNode.nodes());
    }
    
```
So this is probably clear as mud. I'll get better at writing technical posts, promise. Thanks for following along and hey, do ping me if you get stuck. If I can help, I will. I fully understand what it's like to be new and struggling to learn and I'm happy to give back if I can.
