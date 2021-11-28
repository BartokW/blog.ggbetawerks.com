---
layout: single
title: Good Code is a Balancing Act
tags:
- software-development
---

As a software developer for the past 20 years, I've been given numerous rules and recommendations for the code I write. However, almost all of them can make your code worse if you blindly follow them. My suggestion is to understand why the rule or recommendation is being made and strive for code that matches that rather than blindly following the rules.

In this article I'm going to go over some of the rules I've been given, and show how when taken to the extreme they can actually make things worse.

## University Rules

In my first year university programming classes, there were three rules that they enforced using the electronic submission system that would automatically reject your code if you violated any one of the rules. &nbsp;Each rule was trying to teach you an aspect of writing good maintainable code, but having to actually follow them to the letter of the law can make things worse.

### Rule #1: Functions No Longer Than 10 Lines

The first rule I ran into there was that we were not allowed to submit code with functions longer than 10 lines. &nbsp;Since that time, I've never had a limit that short, but have had rules about keeping the function all on one screen. &nbsp;This was back when 1024 x 768 screen resolutions were the maximum standard. &nbsp;But in general the idea behind this rule is to keep your functions as short and readable as possible.

Most mature code bases will have a function somewhere that is really long, and doing way too many things. For example, at work right now we have one function that is 1800 lines long, which is really hard to find anything in it. This is what the line limit type rules are trying to avoid.

The first solution to the long function is to try to break it up into smaller functions, for example broken up into 12 pieces of functionality:
```typescript
function DoTheThing(){
    DoPart1();
    DoPart2();
    DoPart3();
    DoPart4();
    DoPart5();
    DoPart6();
    DoPart7();
    DoPart8();
    DoPart9();
    DoPart10();
    DoPart11();
    DoPart12();
}
```
However, if we're aiming for the 10 lines, then this is still too long, so let's try to shrink it some more by combining some functions together:
```typescript
function DoTheThing(){
    DoPart1And2();
    DoPart3And4();
    DoPart5And6();
    DoPart7And8();
    DoPart9And10();
    DoPart11And12();
}
```
This gets us under the 10 line limit and makes this function clear, however, you should take the rest of the code into account as well. Let's say you are trying to debug something failing, and so you navigate into the first function:
```typescript
function DoPart1And2(){
    DoPart1();
    DoPart2();
}
```
And again into the first one here, and you get:
```typescript
function DoPart1(){
    DoPart1a();
    DoPart1b();
    DoPart1c();
}
```
And this nested functions can keep going which gets really difficult to find the code you're looking for, and how it all fits together.

The other issue you can run into is someone trying to compress the code down, and so you wind up with something that looks like an Obfuscated C code contest entry.
```c
int main(int b,char**i){
   long long n=B,a=I^n,r=(a/b&amp;a)&gt;&gt;4,y=atoi(*++i),_=(((a^n/b)*(y&gt;&gt;T)|y&gt;&gt;S)&amp;r)|(a^r);printf("%.8s\n",(char*)&amp;_);
}
```
<figure><figcaption>https://www.ioccc.org/2020/burton/prog.c</figcaption></figure>

#### Goal of 10 Lines Long

The goal for this rule is to keep functions short and easily understood. But if you enforce an arbitrary limit it can be detrimental to the overall code base.

I recommend aiming for short easily understood functions, but find the balance between super short, and longer but more understandable.

### Rule #2: One Control Structure Per Function
