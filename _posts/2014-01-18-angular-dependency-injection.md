---
layout: post
title: "Angular Dependency Injection"
date: 2014-01-18 08:38:11.0
author: mahon
categories: 
---
I just spent two awesome days at <a href="http://ng-conf.org">ng-conf 2014</a> where were presented with a challenge to improve upon the already <em>awesome</em> Angular dependency injection. At first I didn't really have any ideas. "The only way to improve the DI framework would require interfaces," I thought. And I left it at that.

My subconscious mind, however, kept working on it and I woke up at five this morning with a few ideas. I admit that this solution needs some improvement, but I thought I'd at least write it down and see what comes of it.

One of the few weaknesses that Angular has in its dependency framework is because JavaScript lacks interfaces. This doesn't mean however that Angular couldn't build an interface system. I'm not sure if these ideas could be used without a framework like Angular, so I write this document with the understanding that it would naturally fit into Angular.

Before I begin, I should explain a little about how I personally use Angular, and more importantly, how the various pieces used in Angular come together to help the code stay organized. When I finally convinced my employer, to use Angular, the first thing we found is that it was very easy for everyone to have their own way to structure their code. It is for this reason that I came up with a coding standard that we now currently use.

There is, obviously, nothing in Angular that enforces this standard and I've seen a lot of good code that does it differently, but this is what makes most sense to me.

As you know, under the hood services and factories are the same thing in Angular. While I see no reason for this to change, I try to keep them and their purposes logically separated.

<strong>Factories:</strong>
A factory is used only when dealing with things that need to be created or saved. In general this means that they use $resource (or sometimes $http) to interact with our API. Controllers never directly interact with a factory, rather they are always used by a service. Factories always have the postfix, "Factory".

<strong>Services:</strong>
A service is used to keep logic for a certain thing together. This helps with code reuse and to keep the size of a controller to a minimum. Services are used directly from controllers and other services. Often services use factories to get or save off data. Services are always prefixed with a dollar sign and a two letter abbreviation for the company or project it is used for. (Example: If I'm building a task app called Task App a users service may be named: $taUsers.

With that out of the way, I'll explain what I think could be a good idea for Angular. I hope that there are others who can improve upon this because it has some spots that are a bit clunky.

I consider myself pretty new in the software engineering world, but when I think of dependency injection I think of interfaces. Most of my knowledge about DI is colored by the C# dependency injection framework: Windsor. The general principle that Angular lacks when it comes to dependency injection is that there can be more than one dependency that meets the requirements of the dependency. The particular implementation used is usually chosen at runtime based on the DI frameworks configuration. The only problem is that to do this correctly: there really needs to be interfaces, or something that acts like an interface, in JavaScript. I'm not sure what the best practice would be for declaring the members of an interface, but this is one idea. (Using ECMAScript 5.)

<b>For "Pet Application" this would be used to create an interface.</b>
<pre lang="javascript" line="1">app.interface('iAnimalService', {
  species: angular.STRING,
  commonName: angular.STRING,
  numberOfLegs: angular.NUMBER,
  speak: function(duration){}
});</pre>
<b>Implementing the interface.</b>
<pre lang="javascript" line="10">app.service('$paDog', function(){
  angular.implements('iAnimalService', this);//will throw an error if the requirements are not met

  this.species = “Canis lupus”;
  this.speak = function(duration){
    //…
  }
  //etc.
});</pre>
You can see that the $paDog service implements the iAnimalService interface and that any derivation from the contract will cause an error to be thrown.

I hope that this was helpful. I would love to hear feedback on this and how it could be improved or thoughts that others have.

<div class='archived comments'>

<div class='comment'>Hey great article, I just wanted to mention you have a duplicate paragraph .

"<b>There is, obviously, nothing in Angular that enforces this standard and I’ve seen a lot of good code that does it differently, but this is what makes most sense to me.</b>"  <div class='by'>Andrew Del Prete on 2014-01-22 12:02:21.0  </div></div>
</div>