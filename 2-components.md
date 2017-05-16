<!-- $size: 16:9 -->

![bg](images/slide_bg.png)

## Angular Training
# Components and Data Binding

Peter Bouda, hey@peterbouda.eu

---

![bg](images/slide_bg.png)

# What is a component?

* Basic building blocks of an Angular app
* The visible elements of a web application, like header, menu, product list
* Consist of a class (logic) and a template (view)
* We use them in a component hierarchy to construct the app

---

![bg](images/slide_bg.png)

# Using components

In app.component.template.html:
```
<div class="container">
	<app-header></app-header>
	<app-products></app-products>
</div>
```

In products.component.template.html:
```
<ul>
	<li><app-product-item [id]="1"></app-product-item></li>
</ul>
```

---

![bg](images/slide_bg.png)

# Using the CLI to create components

Create a component:

	$ ng generate component product
    
With inline styles, without tests, no sub-directory:

	$ ng g c header -is --spec false --flat

**Components will be created below the app folder or in the current directory**

---

![bg](images/slide_bg.png)

# A simple component

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styles: []
})
export class HeaderComponent {
  menuItem1: string = 'Home';
  menuItem2: string = 'Produkte';
  menuItem3: string = 'Über uns';

  constructor() { }

}
```

---

![bg](images/slide_bg.png)

# The template with string interpolation

```
<ul>
  <li>{{ menuItem1 }}</li>
  <li>{{ menuItem2 }}</li>
  <li>{{ menuItem3 }}</li>
</ul>
```

---

![bg](images/slide_bg.png)

# Styles

* Angular supports inline styles or external stylesheets
* Support for different pre-processors like SCSS, SASS, ...
* Styles are scoped via Shadow DOM (emulation)
* Gobal styles in `app/styles.css`
* Or add .css files in .angular-cli.json

---

![bg](images/slide_bg.png)

# ng-content

With ng-content HTML is passed from parent to child:

```
<app-header>
  <h2>Der zweite Titel!</h2>
</app-header>
```

And then in `header.component.html`:

```
<ul>
<!-- Some code -->
</ul>
<ng-content></ng-content>
```

---

![bg](images/slide_bg.png)

# Input

* Components can receive data: think of arguments of a function call
* Generally: property binding via `[attributeName]="data"`
* The `@Input` decorator defines a class property as input
* Optionally you can pass a parameter to `@Input` to rename the input

---

![bg](images/slide_bg.png)

# Input

```
import { Component, Input } from '@angular/core';

…

export class HeaderComponent {
  …
  @Input('addThis') menuItem4: string;

  constructor() { }
}


<app-header [addThis]="'Only in the header'"></app-header>
```

---

![bg](images/slide_bg.png)

# Output

* Components can output data as events: think of (asynchronous) return values of function calls
* Generally: event binding via `(eventName)="onEvent()"`
* The `@Output` decorator defines a class property as output
* The output is an `EventEmitter` of a certain type

---

![bg](images/slide_bg.png)

# Output

```
import { Component, Input, Output, EventEmitter } from '@angular/core';

…
export class HeaderComponent {
  …
  @Output() selected = new EventEmitter();

  constructor() { }
}

<app-header [addThis]="'Only in the header'" (selected)="onSelect()"></app-header>
```

---

![bg](images/slide_bg.png)

# Overview of bindings and template syntax

* Property binding
	* From DOM to component
	* `[attributeName]="data"`
* Event binding
	* From component to DOM
	* `(eventName)="doSomething()"`
* Two-way binding
	* From component to DOM and back
	* `[(ngModel)]="data"`

---

![bg](images/slide_bg.png)

# Built-in property binding

```
<img [src]="srcVariable">

<button [disabled]="isValid">

<div [ngClass]="myClasses">
```

---

![bg](images/slide_bg.png)

# Built-in event binding

```
<button (click)="onClick()">

<input (keyup)="onKey($event)">

<div (mouseenter)="mouseenter($event)">
```

---

![bg](images/slide_bg.png)

# Two-way binding

```
<input type="number" name="number1" [(ngModel)]="number1">
<input type="number" name="number2" [(ngModel)]="number2">

{{ number1 + number2 }}

<button (click)="number1 = 10">Nummer 1 wird 10</button>
```

---

![bg](images/slide_bg.png)

# Life cycle of a component

<table>
	<tr>
    	<th>#</th>
    	<th>Lifecycle hook</th>
    	<th>Description</th>
    </tr>
    <tr>
    	<td>1</td>
    	<td>ngOnChanges</td>
        <td>Before <code>ngOnInit</code> and whenever there is a change of a bound property</td>
    </tr>
    <tr>
    	<td>2</td>
    	<td>ngOnInit</td>
        <td>On component initialization and after <code>ngOnChanges</code></td>
    </tr>
   	<tr>
    	<td>3</td>
    	<td>ngDoCheck</td>
        <td>During Angular change detection run</td>
    </tr>
   	<tr>
    	<td>4</td>
    	<td>ngAfterContentInit</td>
        <td>After content of <code>ng-content</code> was inserted</td>
    </tr>
   	<tr>
    	<td>5</td>
    	<td>ngAfterContentChecked</td>
        <td>Aftercontent of #4 was checked</td>
    </tr>
   	<tr>
    	<td>6</td>
    	<td>ngAfterViewInit</td>
        <td>After initialization of the view and child views</td>
    </tr>
   	<tr>
    	<td>7</td>
    	<td>ngAfterViewChecked</td>
        <td>After view and child views were checked</td>
    </tr>
   	<tr>
    	<td>8</td>
    	<td>ngOnDestroy</td>
        <td>Before component gets destroyed</td>
   	</tr>
</table>
    
