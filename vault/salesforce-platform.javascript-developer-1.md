---
id: bdf0d0a4-e383-4d9b-8d12-7c79a9645919
title: JavaScript Developer 1
desc: ''
updated: 1618163699605
created: 1610206576325
---

## JavaScript Developer 1 Certification

### References:
[JS Trailmix - Main reference](https://trailhead.salesforce.com/users/strailhead/trailmixes/prepare-for-your-salesforce-javascript-developer-i-credential)

![[computer-science.lang.javascript]]

---
## Map from Miro
<iframe width="768" height="432" src="https://miro.com/app/live-embed/o9J_lKwLo90=/?moveToViewport=-1704,98,2393,1696" frameBorder="0" scrolling="no" allowFullScreen></iframe>
---

## [Module:JavaScript Skills for Salesforce Developers](https://trailhead.salesforce.com/content/learn/modules/javascript-essentials-salesforce-developers?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

Learn JavaScript Core Concepts

#### JavaScript runtime
The JavaScript runtime is an engine that interprets JavaScript code. It can be part of a browser, or other runtime environment like a server. Modern JavaScript engines are sophisticated and powerful to optimize execution and designed to conform to the ECMAScript standard.

Keyword|Scope|Mutable Assignment|
|--- |--- |--- |
|var|function|yes|
|let|block|yes|
|const|block|no|

JS has **implicit type coercion**:
```javascript
let num1 = 9 * "3";
console.log(num1); // 27 (a number)
let num2 = 9 + "3";
console.log(num2); // "93" (a string)
```
 ==A browser’s primary job is to act as a client for a web server.==
![](/assets/images/2021-01-25-14-50-28.png)

The **DOM**
When a page is requested and then received by a browser, the browser parses the HTML and creates a description, or a model, of that page. This model is used to draw the page in the browser’s viewport. It’s also surfaced to JavaScript through the DOM.
`window.document.body.<...>`

![](/assets/images/2021-04-11-12-42-52.png)

[Conditional Rendering](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/create_conditional)
When writing Lightning web components, you can explicitly define sections of the UI that are rendered only when certain conditions are met. You can do this by adding an `if:true` or `if:false` directive to a nested `template` tag. 

#### [Creating Objects in JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

1. ` Object.create()`
2. Object Literal Notation: basically create object straight away such as: `const obj ={}`
3. with Constructor _f_ such as:
```javascript
function Bike(gears, startGear) {
  this.gears = gears;
  this.currentGear = startGear;
}
```
4. Object.assign()
Assigning Properties and Functions to Objects

Using:
1. . notation: `bike.frontGearIndex` or `bike.calculateGearRatio()`
2. [] notation `bike['frontGearIndex']` or `bike.['calculateGearRatio']()`

> ⚠️ Object mutability: DON'T mutate Objects!!! 

Inheritance: in JS use Prototypical inheritance, not 'Class' inheritance (btw, in [JS classes](https://trailhead.salesforce.com/content/learn/modules/modern-javascript-development/work-with-classes?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential) are special types of function, not such as other OOP languages, try for example to type: `typeof class {}` in browser/JS console)
Under the covers, the engine is still using `Object.create` and there is still no class (in the object-oriented sense), just that in-memory prototype object that is the actual source of inheritance. 

#### Functions
1. Function declaration(assignment): `function <name> (){ // code block here }`
2. Function call(invocation): `<name>()`
3. Function expression `const <name> = () => { // code block here }`
A function need to do something, at least it needs to return:

```javascript
function name (name){ 
    // code block here 
    const example = `Hi, ${name}` 
    // or
    return name
    }
   ```

4. Anonymous Functions: such as the HOF `map()`
`let myArray = [1, 5, 11, 17]; let newArray = myArray.map( function(item){ return item / 2 } );`
5. function as Event handlers: `var handleClick = function(event) {
}`
6. Assigning Event Handlers via DOM APIs (not used) in pure html: `<button onclick="handleClick(event)">Click to Go</button>`

#### Context, Scope, and Closures

- Context and this
As we explored, JavaScript revolves around objects. When a function is invoked, there is always an object container around that function. This object container is its context and the `this` keyword points to that context. 
==So context is not set when a function is declared, but rather where the function is invoked==

- The Global Object
In a browser, the global context is the `window` object. 
`this === window; // true`

- What is a Closure?
When a function is declared, it holds a reference to any variables or arguments declared within it, and any variables it references in the scope that it is contained within. This combination of its variables and arguments along with local variables and arguments from its containing scope is called a closure. 

```javascript
const greet = function(typeGreet){
  return function(name){
    console.log(`${typeGreet}, ${name}, how are you today?`)
  }
}

const morningGreet = greet('Goodmorning')
const afternoonGreet = greet('Goodafternoon')

morningGreet('Cris') // Goodmorning, Cris, how are you today?
afternoonGreet('Lucy') // Goodafternoon, Lucy, how are you today? 
```

#### Write Asynchronous JavaScript

You need to understand:
1. Callback Pattern
A callback is simply a function passed into another function that invokes it at some point in the future. 
```javascript
setTimeout(callback, timer)
Element.addEventListener(event, callback)
Array.map(function(item){...})
```
2. Arrow Functions: `(arg1, arg2) => {...function body...}`
Thanks to them we don't have anymore to bind `this`...
3. Promises & `Async/Await`
Promises developed as libraries to handle asynchronous code in a way that made it easier to reason about when your code succeeded or failed. 

<details><summary>
Apex vs LWC
</summary>

Consider the following Apex class and method: 
```javascript
public with sharing class ContactController {
    @AuraEnabled(cacheable=true)
    public static List<Contact> findContacts(String searchKey) {
        if (String.isBlank(searchKey)) {
            return new List<Contact>();
        }
    String key = '%' + searchKey + '%';
    return [SELECT Id, Name, Title, Phone, Email, Picture__c FROM Contact WHERE Name LIKE :key AND Picture__c != null LIMIT 10];
    }
...
```
VS
Lightning Web Components surfaces this method using a promise-based API. You would invoke it like this: 
```javascript
import { LightningElement } from 'lwc';
import findContacts from '@salesforce/apex/ContactController.findContacts';
export default class ApexImperativeMethodWithParams extends LightningElement {
    searchKey = '';
    contacts;
    error;
    handleSearch() {
        findContacts({ searchKey: this.searchKey })
        .then(result => {
            this.contacts = result;
            this.error = undefined;
        })
        .catch(error => {
            this.error = error;
            this.contacts = undefined;
        });
    }
}
```
</details>

---

[Module:Modern JavaScript Development](https://trailhead.salesforce.com/content/learn/modules/modern-javascript-development?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

- Feature supported by engine type: http://kangax.github.io/compat-table/es6/

<div>
  <iframe src="http://kangax.github.io/compat-table/es6/" height="315" width="560" allowfullscreen="" frameborder="0">
  </iframe>      
</div>
<br>

- [JS Classes](https://trailhead.salesforce.com/content/learn/modules/modern-javascript-development/work-with-classes?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential) 

|Member|Description|Example|
|--- |--- |--- |
|Constructor|The constructor is executed automatically when creating a new instance of the class. It guarantees that an initialization function is always called. This helps maintain a valid state for the class. But you don’t have to create a constructor. If one is not included, then the JavaScript engine creates an empty one for you.|`constructor(name) { this.name = name; }`|
|Static Methods|Static methods are not part of any instance of the class, meaning that you can refer to these methods without referring to an instance. The concept of static class members is not new to ES6, but the static keyword is. Prior to ES6, you had to put any methods you wanted static in the constructor. Now you can put them wherever you want in the class and just use the static keyword.|`static methodName() {return 'something';}`|
|Prototype Methods|These methods do not include the static keyword and must be referenced with an instance.|`printName() { console.log(this.name);}`|
|Getters and Setters|These accessor functions work just like object literals and work the same as they did in ES5. Essentially you just put the get and set keywords in front of the property name. If you define a getter without a setter, then the property becomes read-only.| `get area() { return this.height * this.width; }` `set area(value) { this.area = value; }`|

- [Modules](https://trailhead.salesforce.com/content/learn/modules/modern-javascript-development/organize-code-with-modules?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)
see example here: [Codepen-modules-link](https://codepen.io/criscarart/project/editor/DaVoeo)

- [Asynch JS & Promises](https://trailhead.salesforce.com/content/learn/modules/modern-javascript-development/write-asynchronous-javascript?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential):
The Promise object includes four methods that you may want to check out for more advanced promise scenarios. They include:
* `Promise.all`(iterable)—Returns promise only after all the promises in the iterable have resolved or rejected.
* `Promise.race`(iterable)—Returns promise after the first promise in the iterable has resolved or rejected.
* `Promise.resolve`(reason)—Returns promise that is resolved with the reason passed in as a parameter.
* `Promise.reject`(value)—Returns a promise that is rejected with the value passed in as a parameter.

[Testing & BDD](https://trailhead.salesforce.com/content/learn/modules/modern-javascript-development/test-your-javascript?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

- One of the more popular behavior-driven tools is [Jasmine](https://jasmine.github.io/).
![](/assets/images/2021-01-26-12-06-55.png)

Ex Jasmine test
```javascript
describe("A test suite is just a function", function() {
    it("This is a positive test", function() {
      expect(true).toBe(true);
    });
    it("This is a negative test", function() {
      expect(false).not.toBe(true);
    });
});  
```

---

## [Quick Start: SFDX](https://trailhead.salesforce.com/content/learn/projects/quick-start-salesforce-dx?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)
Notes:

<details><summary>
What Is a Scratch Org?
</summary>
in SFDX, A scratch org is a dedicated, configurable, and short-term Salesforce environment that you can quickly spin up when starting a new project, a new feature branch, or a feature test.
</details>

<details><summary>
What Is a Developer Hub Org?
</summary>
A Developer Hub (Dev Hub) is the main Salesforce org that you and your team use to create and manage your scratch orgs
</details>

Basic Commands in SFDX:
```
SFDX
Usage: sfdx COMMAND [command-specific-options]
 Help topics, type "sfdx help TOPIC" for more details:
 sfdx force # tools for the salesforce developer
 sfdx plugins # manage plugins
 sfdx update # update sfdx-cli
```
- create a Dev Hub:
```
sfdx auth:web:login -d -a DevHub
```
... and then Allow.

- to get help:
```
sfdx force --help
```
- create a scratch org:
```
Create a scratch org, set it as your default, and give it an alias
```
- delete an org:
```
sfdx force:org:delete
```
Display a list of all authorized orgs. You should see an entry listed with alias DevHub
```
sfdx force:org:list
```
[Use this Dev Hub as the default](https://trailhead.salesforce.com/content/learn/projects/develop-app-with-salesforce-cli-and-source-control/set-up-your-environment?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential#authorize-dev-hub-org-with-the-salesforce-cli:~:text=Use%20this%20Dev%20Hub%20as%20the%20default) for all Salesforce DX projects on this computer so that you don’t have to specify the --targetdevhubusername | -v CLI argument for each command
```
sfdx config:set defaultdevhubusername=DevHub --global
```
![](/assets/images/2021-01-18-11-22-49.png)

<details><summary>
Ex. SQOL to retrieve sample data from Devhub Org
</summary>

```apex
sfdx force:data:tree:export --targetusername DevHub --outputdir assets/data --query "SELECT Id, Name, Email__c, Phone__c, Mobile_Phone__c, Title__c, Picture__c, ( SELECT Id, Address__c, Assessed_Value__c, Baths__c, Beds__c, Broker__c, City__c, Date_Agreement__c, Date_Closed__c, Date_Contracted__c, Date_Listed__c, Date_Pre_Market__c, Description__c, Location__Longitude__s, Location__Latitude__s, Picture__c, Price__c, Name, State__c, Status__c, Tags__c, Thumbnail__c, Title__c, Zip__c FROM Properties__r ) FROM Broker__c"
```
</details>

[how to disable caching in any (scratch, dev, sandboxes...) Org](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/debug_disable_caching.htm)
![](/assets/images/2021-01-18-12-13-02.png)

[Import & Export data between Orgs](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_test_data_example.htm)

[Create a new SFDX project](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_create_new.htm)

Delete a Scratch org command:
```
sfdx force:org:delete
```
create a Scratch Org:
```
sfdx force:org:create --setdefaultusername --setalias sfdx-chan --definitionfile config/project-scratch-def.json
```
Push local source metadata to scratch org.
```
sfdx force:source:push
```
---

## [Develop an App with Salesforce CLI and Source Control](https://trailhead.salesforce.com/content/learn/projects/develop-app-with-salesforce-cli-and-source-control?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

---
## [Coding for Web Accessibility](https://trailhead.salesforce.com/content/learn/modules/coding-for-web-accessibility?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

Semantic MarkUp:

0. [ARIA](https://www.w3.org/TR/wai-aria-1.1/) (Accessible Rich Internet Applications): Remember the 3 attributes available: **roles**,**states**,**properties** follow as well this web components [blueprint](https://www.lightningdesignsystem.com/components/overview/)
1. [Nu HTML Checker](https://validator.w3.org/nu/):auth the use semantic HTML5
2. [Global focus guidelines](https://www.lightningdesignsystem.com/accessibility/guidelines/global-focus/): for accessible navigation
3. [Accessibility Object Model](https://wicg.github.io/aom/spec/)
4. [Web Components](https://www.webcomponents.org/introduction)
5. other tricks to write accessible components from [this module](https://trailhead.salesforce.com/content/learn/modules/coding-for-web-accessibility/write-accessible-components?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

---
## [Lightning Design System for Developers](https://trailhead.salesforce.com/content/learn/modules/lightning_design_system?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

> The CSS is fully namespaced with the `slds-` prefix and scoped with the `slds-scope` class to avoid CSS conflicts.

[lightningdesignsystem.com](https://www.lightningdesignsystem.com/)
BEM system:
* A **block** represents a high-level component (e.g. .car)
* An **element** represents a descendant of a component (e.g. .car__door)
* A **modifier** represents a particular state or variant of a block or element (e.g. `.car__door_red`)
Grid System
[CSS-tricks flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
Data:
-  in the current release the Design System doesn’t support built-in Visualforce components — the `<apex:*>`, `<chatter:*>` and other components

- The **grid** system:
```javascript
<div class="slds-grid">
 <div class="slds-col">Column 1</div>
 <div class="slds-col">Column 2</div>
 <div class="slds-col">Column 3</div>
</div>
```
- The **Data Table** Component
The Data Table component is an enhanced version of a HTML table for displaying tabular data with the Lightning UI styling. A Data Table is created by applying the `slds-table` class to a `<table>` tag. Use the `slds-table_bordered` class to apply a border.

- the use of [Remote Objects](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_remote_objects.htm) JavaScript Remoting or the REST API to access Salesforce data from your Visualforce pages based on Design System markup.

- Avatar, Icons and [SVG Sprite](https://css-tricks.com/svg-sprites-use-better-icon-fonts/)

<details><summary>
What is a Card and a Tile component?
</summary>

A [tile](https://www.lightningdesignsystem.com/components/tiles/) is a grouping of related information associated with a record.
[Cards](https://www.lightningdesignsystem.com/components/cards/) are used to apply a container around a related grouping of information.
</details>

---

## [Quick Start: Lightning Web Components](https://trailhead.salesforce.com/content/learn/projects/quick-start-lightning-web-components?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential): easy and quick way to start with a basic LWC HelloWorld component

---

## [Lightning Web Components Basics](https://trailhead.salesforce.com/content/learn/modules/lightning-web-components-basics)


[`sfdx force:` - Salesforce CLI Command Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference_force.htm)

[CLI](https://trailhead.salesforce.com/content/learn/modules/cli-basics?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

How to Run Commands:
```javascript
 $ command -flag arguments
 ```

![](/assets/images/2021-01-18-08-55-39.png)
* The command is: `sfdx force:project:create`
* The flag `-n` is required and tells the system what to name your new project.
* The argument is MyProject which is the name we are assigning to the project.
* Switches are a lot like flags but don't require arguments. Switches are identified by two hyphens -- and followed by a value.

Useful commands if you're lost:
```javascript
pwd // print working directory
history // sometime can be followed by
!<commandNumber>  // to repeat the same command from the past
cd ../ // etc.
```

- Install last version _npm_: 

```terminal
npm install npm@latest -g
```
- Update sfdx:
```terminal
sfdx update
```


#### [To develop Lightning web components efficiently, use the following tools and environments.](https://trailhead.salesforce.com/content/learn/modules/lightning-web-components-basics/create-lightning-web-components?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

0. https://webcomponents.dev/
1. Dev Hub and Scratch Orgs
2. Salesforce Command Line Interface (CLI)
3. [Lightning Component Library](https://developer.salesforce.com/docs/component-library/overview/components)
4. [Lightning Web Components Recipes ](https://github.com/trailheadapps/lwc-recipes)
5. [Lightning Data Service (LDS)](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.data_ui_api): to access data and metadata from Salesforce
6. [Lightning Locker](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.security_locker_service)
7. Lifecycle Hooks:
    - Lightning Web Components provides methods that allow you to “hook” your code up to critical events in a component’s lifecycle. These events include when a component is:
        * Created
        * Added to the DOM
        * Rendered in the browser
        * Encountering errors
        * Removed from the DOM
    - ex.: `connectedCallback()` and `disconnectedCallback()`


8. Decorators:
    - **@api**: Marks a field as public. Public properties define the API for a component.
    - **@track**: Tells the framework to observe changes to the properties of an object or to the elements of an array.
    - **@wire**: Gives you an easy way to get and bind data from a Salesforce org.

<details><summary>
How to use the @wire service?
</summary>

```javascript
import { adapterId } from 'adapter-module';
@wire(adapterId, adapterConfig)
propertyOrFunction;
```
* `adapterId` (Identifier)—The identifier of the wire adapter.
* `adapter-module` (String)—The identifier of the module that contains the wire adapter function.
* `adapterConfig` (Object)—A configuration object specific to the wire adapter.
* `propertyOrFunction` —A private property or function that receives the stream of data from the wire service. If a property is decorated with @wire, the results are returned to the property’s data property or error property. If a function is decorated with @wire, the results are returned in an object with a data property and an error property.

---
</details>

    
9. a LWC component contain:
    1. .js-meta.xml
    2. HTML file
    3. CSS file
    4. JS file
10. `isExposed` ( true or false) makes the component available from other namespaces

11. Handle Events in Lightning Web Components
![](/assets/images/2021-01-28-15-50-40.png)
![](/assets/images/2021-01-20-09-11-31.png)

Events Up, Properties Down
![](/assets/images/2021-01-20-09-14-11.png)

1. **Passing Information Up**
Information can be passed up using **events** ([CustomEvents](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent)) and **event listeners**.
2. **Passing Information Down**
Information can be passed down using **public properties** and **public methods**.
(Public properties are great solutions for passing down primitive values, simple objects, and arrays.)

> TIP To communicate down the component containment hierarchy, pass properties to the child via HTML attributes, or call its public methods. To communicate between components, use Lightning message service or a publish-subcribe utility.
        
References:
[reactivity](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.reactivity)
[Lightning Web Components Dev Guide](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.reference)

- [ ] https://trailhead.salesforce.com/content/learn/modules/lightning-web-components-basics/handle-events-in-lightning-web-components?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential
- [ ] https://trailhead.salesforce.com/content/learn/modules/lightning-web-components-basics/add-styles-and-data-to-a-lightning-web-component?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential
---

## [Lightning Web Components for Visualforce Developers](https://trailhead.salesforce.com/content/learn/modules/lwc-for-visualforce-developers?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

- Pre-requisites: [Learn to work with the JS](https://trailhead.salesforce.com/en/content/learn/trails/learn-to-work-with-javascript)
- [Mobile App customization](https://trailhead.salesforce.com/content/learn/modules/salesforce1_mobile_app)
- [webcomponents](https://github.com/WICG/webcomponents)

Low-code | Pro-code
---------|---------
Lightning pages | Lightning Web Components 
Quick actions | Aura Components
URL buttons | Visualforce
Utility bar |
Flows |

Traditional Visualforce Requests | Lightning Web Component Requests
---------------------------------|---------------------------------
![](/assets/images/2021-01-20-09-58-10.png) | ![](/assets/images/2021-01-20-09-58-53.png)
User requests a page. | User requests a component.
The server executes the page’s underlying code and sends the resulting HTML to the browser. | The component definition is returned to the client as a compiled JavaScript file.
The browser displays the page. | The JavaScript application generates the UI.
When the user interacts with the page again, the cycle repeats from step one. | When the user interacts with the page again, the JavaScript modifies the UI on the client.

[Use template directive to conditional rendering](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.create_conditional)

rendering Lists

Visualforce | LWC
--- | ---
<apex:repeat> | for:each and iterator:iteratorName

Invoking JS
```javascript
// eventHandler.html
<template>
    <a onclick={handleButtonClick}></a>
    
</template>
// eventHandler.js
import { LightningElement } from 'lwc';
export default class EventHandler extends LightningElement {
    handleButtonClick(event) {
        // Do whatever...
    }
}
```
[Visualforce Standard Components and
Base Lightning Web Components Side by Side](./assets/pdfs/EquivalentsReference.pdf)

Traditional Visualforce Data Access | Lightning Web Components Data Access
--- | ---
![](/assets/images/2021-01-20-11-36-22.png) | ![](/assets/images/2021-01-20-11-36-42.png)
Visualforce pages that retrieve or modify data use a standard controller or an Apex controller. Traditionally, the client invokes controller methods when the page loads or as actions. After a method runs, the Visualforce engine generates the final markup and sends it to the client. | Lightning web components that retrieve or modify data use Lightning Data Service or Apex methods. The client uses JavaScript to call Lightning Data Service or an Apex method. After Lightning Data Service or an Apex method runs, the Lightning Web Components engine returns the minimal needed data to the client. JavaScript on the client processes the data and generates the final markup.

- [Lightning Data Service](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.data_ui_api) is the preferred (and easiest) way to work with Salesforce data in Lightning web components.
Lightning Data Service is a similar concept to a Visualforce standard controller.

> Note
To use Lightning Data Service, you have two options: Use a base component that allows you to build forms, or use an LDS wire adapter or function. The code to use Lightning Data Service is very specific, and fully explained in Use Lightning Data Service to Work with Data in the Lightning Web Components and Salesforce Data module.

> Apex in LWC
Methods are stateless.
Methods must be @AuraEnabled, and can be marked as cacheable.
You import Apex methods and then call them.
You bind components to record pages in a metadata xml file, and access the record Id in the JavaScript file.
Proper error handling involves throwing exceptions, instead of using page messages.


View State:

Server State | Stateless Methods
---|---
Visualforce uses the view state to maintain state between server requests |  In Lightning web components, server requests are stateless.

Calling Apex:

 in Visualforce | Lightning Web Components
---|---
binding the controller and referencing the Apex methods in the page <apex:page> |  import the methods into the JS file and then invoke the methods in JS:<br> `import queryAccounts from '@salesforce/apex/AccountListControllerLwc.queryAccounts;`


Accessing a Record Id:

 in Visualforce | Lightning Web Components
---|---
retrieve the record Id from the `ApexPages.StandardController` | accessing a record Id in Apex is a little less straightforward
 &nbsp; | Expose the component so that it’s available on record pages.
&nbsp; |In JavaScript, declare recordId as a public property. The parent component (the record Flexipage) populates the value of the recordId property.
Pass the recordId as a parameter in Apex method calls.

Server Errors in Visualforce | Lightning Web Components
---|---
<apex:pageMessages> and <apex:pageMessage> to show errors in different ways |  you decide whether to let exceptions propagate or to gain more control over what’s shown to the user by throwing an AuraHandledException or a custom exception. 

VF | LWC
---|---
URLFOR <br> Visualforce pages often use the URLFOR function with `<apex:commandButton>` and `<apex:commandLink>` tags to navigate to pages inside or outside of Salesforce. The Navigation Service eliminates hardcoded URLs by using meaningful `PageReference` objects to perform navigation. If Salesforce changes a specific URL, code that links to it doesn't break. Apex controller methods also facilitate navigation by returning `PageReferences`.| [Lightning Navigation Service](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.use_navigate)



<details><summary>
How ti use the Navigation Service?
</summary>

1. Import the lightning/navigation JavaScript module into the Lightning web component.
2. Make the Lightning web component’s class extend NavigationMixin.
3. Call the Navigate method, passing the page reference of the page you want to navigate to.
</details>

> Note
You can use a Visualforce page within Lightning Experience.
Visualforce and Lightning web components can also coexist on the same page and interact.
Use the [Lightning Components for Visualforce](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.use_message_channel) JavaScript Library when you want to include a Lightning web component in a specific location on a Visualforce page

---

[Lightning Web Components for Aura Developers ](https://trailhead.salesforce.com/content/learn/modules/lightning-web-components-for-aura-developers?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)


Lightning Web Components is an implementation of the W3C’s Web Components standards.

> ⚠️
Important
✅ allowed Aura components can contain Lightning web components.
❌ not allowed Lightning web components can’t contain Aura components.


Aura | LWC
---|---
.cpm starting with `<aura:component>` |.html file starting with: `<template>` and can have nested `<template>` 
attributes: `<aura:attribute>` | ...become JS props `@api recordId; property;...`
Basic Aura Expressions | ...Become HTML Expressions
Migrate `<aura:if>` tags in an Aura component to <br> `<aura:if isTrue="{!v.property.Broker__c}">` | `if:true` or `if:false` directives in a Lightning web component’s HTML file <br> `<template if:true={property.data}>`
Complex Aura Expressions <br> `<aura:if isTrue="{!v.page > 1}">` | ...Become JavaScript Logic <br> `<template if:false={isFirstPage}>`
Aura Iterations <br> `<aura:iteration items="{!v.properties}" var="property">` | ...Become HTML Iterations <br> `<template for:each={properties.data.records} for:item="property">`
Aura Initializers <br> `<aura:handler name="init" value="{!this}" action="{!c.onInit}" />` | ... Become Lifecycle Hooks `connectedCallback()`
Aura CSS <br> Remove the proprietary `THIS`<br> `java.THIS .lower-third {` class that Aura components use. | ...Becomes Standard CSS
Lightning Web Components Inherit Aura Component Styling | &nbsp;

<details><summary>
Migrating a component from Aura to LWC
</summary>

This Aura component uses the lightning:formattedNumber base component.
```java
<aura:component>
    <lightning:formattedNumber value="5000" style="currency"
      currencyCode="USD" />
</aura:component>
```
To migrate this markup to a Lightning web component:

* Change the colon that separates the namespace (`lightning`) and component name (`formattedNumber`) to a dash.
* Change the camel-case component name (`formattedNumber`) to a dash-separated name (formatted-number).
* Change the camel-case attribute name (`currencyCode`) to a dash-separated name (`currency-code`).
Here’s the equivalent Lightning web component.

```java
<template>
    <lightning-formatted-number value="5000" style="currency"
      currency-code="USD">
    </lightning-formatted-number>
</template>
```
</details>

> 💯
The main takeaway is that Lightning web components use standard JavaScript modules

==Use Third-Party JavaScript Libraries==
> To use a third-party JavaScript library in an Aura component or a Lightning web component, you must upload the library as a static resource.

Aura | LWC
-|-
`<ltng:require>` such as <br> `<ltng:require scripts="{!$Resource.resourceName}" afterScriptsLoaded="{!c.afterScriptsLoaded}" />`| ...become `import resourceName from '@salesforce/resourceUrl/resourceName';`
In an Aura component, you can dynamically create a component in JavaScript using $A.createComponent(). | There’s no equivalent for dynamically creating a component in a Lightning web component.
2 ways data binding | 1 way data binding (check below)


<details><summary>
How do you access an ES6 module in Aura component?
</summary>

e added an `aura:id` so that we can get a reference to the module in JavaScript code.
</details>

Ref.
[Use 3rd party JS Lib](https://developer.salesforce.com/docs/component-library/documentation/lwc/lwc.create_third_party_library)

> ⚠️ Use the Wire Service to Get Data
To read Salesforce data, Lightning web components use a reactive wire service, which is built on Lightning Data Service. Components use @wire in their JavaScript class to read data from one of the wire adapters in the lightning/ui*Api namespace. For the list of wire adapters that Salesforce provides, see the Resources section of this unit. You can’t write your own custom wire adapters.

> ⚠️ Important - Use JavaScript API Methods to Write Data
Don’t use @wire to create, update, or delete a record. The wire service delegates control flow to the Lightning Web Components engine. Delegating control is great for read operations, but it isn’t great for create, update, and delete operations. As a developer, you want complete control over operations that change data. That’s why you perform create, update, and delete operations with a JavaScript API instead of with the wire service.

> Use Apex for Custom Data Access

> Use the Wire Service to Call an Apex Method:
- If an Apex method is cacheable (it doesn’t mutate data), you can invoke it from a component via the wire service. You must annotate the method with @AuraEnabled(cacheable=true)

> Call an Apex Method Directly
- If an Apex method mutates (creates, updates, or deletes) data and therefore isn’t cacheable, you must call the method directly in code.

> ⚠️ Note
Don’t `@wire` an Apex method that creates, updates, or deletes data.

> ⚠️  Work with External APIs
Working with external APIs in a Lightning web component is similar to Aura components. By default, you can’t make calls to third-party APIs from JavaScript code in Lightning web components. Add a remote site as a CSP Trusted Site to allow JavaScript component code to load assets from and make API requests to that site’s domain.

> ⚠️  Aura Component Data Binding
- Aura components can use two forms of expression syntax for data binding: {!expr} or {#expr}. We won’t go into the differences here, but let’s look at a simple example.
In Aura components, this data binding is two way. Changes to the `fName` attribute are passed into the child component’s `firstName` attribute, and changes in the value of the `firstName` attribute in the child component are passed back to the `fName` attribute of the owner component. This bidirectional data binding can be expensive for performance, and can create hard-to-debug errors due to the propagation of data changes, especially if you have deeply nested components
- Lightning Web Component Data Binding:
The data-binding behavior is intentionally simpler and more predictable in Lightning web components. The child component must treat any property values passed from the owner component as read-only. If the child component changes a value passed from an owner component, you see an error in the browser console.

To trigger a mutation for the property value provided by the owner component, the child component can send an event to the parent. If the parent owns the data, the parent can change the property value, which propagates down to the child component via the one-way data binding.

Aura | LWC
-|-
Instead of the proprietary `Event` object in Aura components | use the `Event` or `CustomEvent` standard DOM interfaces, as a pub-sub (publish-subscribe) pattern |
`event.fire()`| `this.dispatchEvent(myEvent)`
handle event with `<aura:handler> ` | ... become `<c-child onnotification={handleNotification}></c-child>` note the use of `on`

<details><summary>
What the Application Events Become a Publish-Subscribe Pattern?
</summary>

To have:The pubsub module exports three methods.

- **register**
Registers a callback for an event.
- **unregister**
Unregisters a callback for an event.
- **fire**
Fires an event to listeners.


</details>

<details><summary>
What does a component event in Aura map to in a Lightning web component?
</summary>
to web-standard DOM events in Lightning web components. 
</details>

<details><summary>
What prefix does an Aura handler add to the event name of a Lightning web component?
</summary>
**Lorem ipsum dolor sit amet...**
</details>

---

## [Lightning Web Components and Salesforce Data](https://trailhead.salesforce.com/content/learn/modules/lightning-web-components-and-salesforce-data?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)
:this trail and challenges teach you how to create LWC via:
    - LDS with LWC
    - APex classes + LWC

1. ==Lightning Data Service is the preferred (and easiest) way to work with Salesforce data. With Lightning Data Service, developers use JavaScript to connect components to Salesforce data. Lightning Data Service also provides security by making server calls to secure [User Interface API](https://developer.salesforce.com/docs/atlas.en-us.uiapi.meta/uiapi/) endpoints.==
    - read data: use the `lightning-record-form` base component;
    - to create, edit, delete records you need a but you need a [LDS function](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.reference_ui_api)
> Note
When we invoke an LDS function imperatively, it returns a [promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) (a JavaScript object that simplifies performing asynchronous calls).

2. Another mothid to work with dat and LWC is via the `@wire` connector (or [LWD wire adapters](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.reference_ui_api));
    - with property ord function ( 🤢 🤮 sintax)

> ⚠️  Note
When an `@AuraEnabled`method is cacheable, Data Manipulation Language (DML) operations are not allowed.

### Call Apex Methods from LWC
==There are two ways to interact with Apex methods from Lightning web components: either wire the method or call the method imperatively.== Let’s consider both approaches.

1. Call Apex Using `@wire`
2. Call Apex Imperatively
An alternative to calling Apex with @wire is to call Apex imperatively. Call Apex imperatively when you need to control the invocation of read operations, and when you modify records

> 👁‍🗨 Note
The preferred way to work with lists of records in Lightning web components is to use the `lightning-datatable` base component. Use `lightning-datatable` to create tables of data with features such as infinite scrolling, inline editing, header and row-level actions, resizing, and more. This UI component needs to be fed data. The most common way to generate that data is to call Apex in any of the ways explained earlier in this module.

### Handle Server Errors

How to handle server errors:
- that occur when you wire a property;
- that occur when you wire a function;
<details><summary>👁‍🗨 </summary>
> Note
The reduceErrors helper function in this example comes from the ldsUtils module of the LWC Recipes sample app. [LWC Recipes](https://github.com/trailheadapps/lwc-recipes) contains easy-to-digest examples of common patterns implemented as Lightning web components. Feel free to copy the ldsUtils module into your project, and use the reduceErrors function.
</details>
- that occur when you call an LDS function or Apex imperatively;

#### Wrap up: Use Cases for Interacting with Salesforce Data
|Use Case|Recommended Solution|Notes|
|--- |--- |--- |
|View or edit a record specifying its layout or a list of fields|`lightning-record-form`||
|View a record with a custom form layout or custom rendering of record data|`lightning-record-view-form`||
|Edit a record with a custom form layout, custom rendering of record data, or prepopulated field values|`lightning-record-edit-form`||
|Read metadata or read data for one record|LDS wire adapters|Can be combined, but operations will run on independent transactions|
|Create, edit, or delete one record|LDS functions|Can be combined, but operations will run on independent transactions|
|Read multiple records|Call Apex with `@wire`|Annotate the Apex method with `cacheable=true`|
|Read multiple records on a one-time invocation or modify multiple records|Call Apex imperatively|For reads, annotate the Apex method with `cacheable=true`|

When you work with data in Lightning web components, error handling varies. How you access errors depends on how you’re interacting with the data.

Handling Server Errors

|How to Work with Data|How to Access Server Errors|
|--- |--- |
|Use either an LDS wire adapter or an Apex method, and decorate a property|Use `reduceErrors` to handle the error that’s returned to the wired property in `decoratedProperty.error`|
|Use either an LDS wire adapter or an Apex method, and decorate a function|Use `reduceErrors` to handle the error that’s returned in the object parameter received by the wired function `decoratedFunction({data, error}) {...}`|
|Imperatively invoke either an LDS wire function or an Apex method|Use reduceErrors to handle the error that’s received as a parameter on the catch method's callback function `functionToInvoke().then(data => {...}).catch(error => {...});`|
---

## [Lightning Web Components Tests](https://trailhead.salesforce.com/content/learn/modules/test-lightning-web-components?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential)

> “Any bug not detected in the design phase will cost ten times more time to detect at the coding phase and an additional ten times more at the debugging phase.” 
        Dr. Nikolai Bezroukov, The Art of Debugging

### There are two types of tests that are commonly performed for Salesforce applications:
_unit tests_ and _end-to-end tests_. Their differences are in their scope and purpose.

||Unit Testing|End-to-End Testing|
|--- |--- |--- |
|Tests run fast|Yes|No|
|Tests are reliable|Yes|No|
|Tests are precise and allow you to identify exact problems|Yes|No|
|Tests cover many features of an application at once|No|Yes|
|Simulates a real user|No|Yes|

### What do you need to run the tests?
1. Jest is a powerful tool with rich features for writing JavaScript tests
Install Jest from SF CLI with the command: `sfdx force:lightning:lwc:test:setup`

2. Node is important: The `@salesforce/sfdx-lwc-jest` Node module lets you write, run, and debug Jest tests for Lightning Web Components. The Salesforce CLI makes it easy to install Jest and its dependencies into the project.

For to runt tests, watch etc. from package.json
```javascript
"scripts": {
    "lint": "npm run lint:lwc && npm run lint:aura",
    "lint:aura": "eslint **/aura/**",
    "lint:lwc": "eslint **/lwc/**",
    "test": "npm run test:unit",
    "test:unit": "sfdx-lwc-jest",
    "test:unit:watch": "sfdx-lwc-jest --watch",
    "test:unit:debug": "sfdx-lwc-jest --debug",
    "test:unit:coverage": "sfdx-lwc-jest --coverage",
    "prettier": "prettier --write \"**/*.{cls,cmp,component,css,html,js,json,md,page,trigger,xml,yaml,yml}\"",
    "prettier:verify": "prettier --list-different \"**/*.{cls,cmp,component,css,html,js,json,md,page,trigger,xml,yaml,yml}\""
  },
  ```

### Testing @Wire Service
Lightning web components use a reactive wire service built on Lightning Data Service to read Salesforce data. Components use `@wire` in their JavaScript class to read data from one of the wire adapters in the `lightning/ui*Api` modules.
Reactive variables are prefixed with a `$`.
==We import the test utility API from `sfdx-lwc-jest` to mock the data so our test isn’t dependent on unpredictable factors like remote invocation or server latency.==

[There are three adapters for mocking wire service data.](https://trailhead.salesforce.com/content/learn/modules/test-lightning-web-components/write-a-jest-test-for-wire-service?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-javascript-developer-i-credential) 
* **Generic wire adapter**: The generic adapter emits data on demand when you call the emit() API. It does not include any extra information about the data itself.
* **Lightning Data Service (LDS) wire adapter**: The LDS adapter mimics Lightning Data Service behavior and includes information about the data’s properties.
* **Apex wire adapter**: The Apex wire adapter mimics calls to an Apex method and includes any error status.


<details><summary>
What is a "stub" and why use it for mocking?
</summary>

Mocking Base Components
The sfdx-lwc-jest package lets us run Jest on Lightning Web Components. It sets up all the necessary Jest configs to run tests without any additional changes. We’ve been using it extensively in the prior modules. This package includes a set of stubs (programs that simulate components that a module undergoing tests depends on) for all components in the Lightning namespace. This includes all the Lightning Base Components. The stubs are installed in the `src/lightning-stubs` directory in the `node-modules/@salesforce/sfdx-lwc-jest` directory.

  **stub**  needs an .html and .js files
---
</details>


---
---

Last Trailheads to DO:

- [ ] [JavaScript Dev I Cert Prep: Variables, Collections, Functions, Classes](https://trailhead.salesforce.com/content/learn/modules/javascript-dev-i-cert-prep-variables-collections-functions-classes)

- [ ] [JavaScript Dev I Cert Prep: Browsers, Async Programming, Server Side JavaScript](https://trailhead.salesforce.com/content/learn/modules/javascript-dev-i-cert-prep-browsers-async-programming-server-side-javascript)

- [ ] [JavaScript Dev I Cert Prep: Debugging, Error Handling, Testing](https://trailhead.salesforce.com/content/learn/modules/javascript-developer-i-certification-prep-debugging-error-handling-testing) 

- [ ] [trail just before exam to recap](https://trailhead.salesforce.com/en/content/learn/trails/study-for-the-salesforce-javascript-developer-i-exam) 


---
---

[Salesforce Certified JavaScript Developer I Exam Guide](https://trailhead.salesforce.com/help?article=Salesforce-Certified-JavaScript-Developer-I-Exam-Guide)
notes:

