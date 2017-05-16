<!-- $size: 16:9 -->

![bg](images/slide_bg.png)

## Angular Training
# Directives

Peter Bouda, hey@peterbouda.eu

---

![bg](images/slide_bg.png)

# What is a directive?

* Components without a view
* We use them to react on DOM events and to manipulate the DOM
* There are two types
	* Attribute directives
	* Structural directives
* Angular contains built-in directives for your app

---

![bg](images/slide_bg.png)

# Attribute Directives

* Modify the appearance or the behaviour of a DOM element
* Are used as an attribute of an element
* Do not depend of a specific implementation of a component

---

![bg](images/slide_bg.png)

# NgStyle

```
@Component({
    selector: 'app-style-example',
    template: `
        <p style="padding: 1rem"
            [ngStyle]="{
       	        'color': 'red',
                'font-weight': 'bold',
                'borderBottom': borderStyle
            }">
            <ng-content></ng-content>
        </p>
    `
})
export class StyleExampleComponent {
    borderStyle = '1px solid black';
}
```

---

![bg](images/slide_bg.png)

# NgClass

```
@Component({
    selector: 'app-class-as-string',
    template: `
        <p ngClass="centered-text underlined" class="orange">
            <ng-content></ng-content>
        </p>
    `,
    styles: [`
        .centered-text {
            text-align: center;
        }
        .underlined {
            border-bottom: 1px solid #ccc;
        }
        .orange {
            color: orange;
        }
    `]
})
export class ClassAsStringComponent {
}
```

---

![bg](images/slide_bg.png)

# Structural Directives

* Make use of the `template` element to specify how to render a component
* Special syntax as syntactiv sugar

```
*structuralDirective="expression"
```

equivalent to:

```
<template [structuralDirective]="expression">
...
</template>
```

---

![bg](images/slide_bg.png)

# Structural Directives

```
@Component({
  selector: 'app-directive-example',
  template: `
    <p *structuralDirective="expression">
      Under a structural directive
    </p>
  `
})
```

Is equivalent to:

```
@Component({
  selector: 'app-directive-example',
  template: `
    <template [structuralDirective]="expression">
    <p>
      Under a structural directive
    </p>
    </template>
  `
})
```

---

![bg](images/slide_bg.png)

# NgIf

```
@Component({
    selector: 'app-root',
    template: `
        <button type="button" (click)="toggleExists()">
            Toggle Component
        </button>
        <hr>
        <app-if-example *ngIf="exists">
            Hello
        </app-if-example>
    `
})
export class AppComponent {
    exists = true;
    toggleExists() {
        this.exists = !this.exists;
    }
}
```

---

![bg](images/slide_bg.png)

# NgFor

```
@Component({
    selector: 'app-root',
    template: `
        <app-for-example *ngFor="let episode of episodes" [episode]="episode">
            {{episode.title}}
        </app-for-example>
    `
})
export class AppComponent {
    episodes = [
        { title: 'Winter Is Coming', director: 'Tim Van Patten' },
        { title: 'The Kingsroad', director: 'Tim Van Patten' },
        { title: 'Lord Snow', director: 'Brian Kirk' },
    ];
}
```

---

![bg](images/slide_bg.png)

# NgFor

## Local variables

<table>
	<tr>
    	<td>index</td>
        <td>The index of the current element</td>
    </tr>
	<tr>
    	<td>first</td>
        <td>Is it the first element?</td>
    </tr>
	<tr>
    	<td>last</td>
        <td>Is it the last element?</td>
    </tr>
	<tr>
    	<td>even</td>
        <td>Is the current index even?</td>
    </tr>
	<tr>
    	<td>odd</td>
        <td>Is the current index odd?</td>
    </tr>
</table>

---

![bg](images/slide_bg.png)

# NgFor

```
@Component({
    selector: 'app-root',
    template: `
        <app-for-example
        *ngFor="let episode of episodes; let i = index; let isOdd = odd"
        [episode]="episode"
        [ngClass]="{ odd: isOdd }">
            {{i+1}}. {{episode.title}}
        </app-for-example>
    `
})
```

---

![bg](images/slide_bg.png)

# NgSwitch

```
@Component({
    selector: 'app-root',
    template: `
      <div class="tabs-selection">
        <app-tab [active]="isSelected(1)" (click)="setTab(1)">Tab 1</app-tab>
        <app-tab [active]="isSelected(2)" (click)="setTab(2)">Tab 2</app-tab>
      </div>
      <div [ngSwitch]="tab">
        <app-tab-content *ngSwitchCase="1">Tab content 1</app-tab-content>
        <app-tab-content *ngSwitchCase="2">Tab content 2</app-tab-content>
        <app-tab-content *ngSwitchDefault>Select a tab</app-tab-content>
      </div>
    `
})
export class AppComponent {
    tab: number = 0;
    setTab(num: number) { this.tab = num; }
    isSelected(num: number) { return this.tab === num; }
}
```

---

![bg](images/slide_bg.png)

# Custom attribute directive

```
import { Directive, HostBinding, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appChangeColor]'
})
export class ChangeColorDirective {
  @Input() defaultColor = 'green';
  @HostBinding('style.color') color = this.defaultColor;

  @HostListener('mouseenter') mouseenter() {
    this.color = 'red';
  }

  constructor() { }
}
```

In HTML:

```
<li appChangeColor [defaultColor]="green">{{ menuItem }}</li>
```

---

![bg](images/slide_bg.png)

# Custom structural directive

```
import { Directive, TemplateRef, ViewContainerRef, Input } from '@angular/core';

@Directive({
  selector: '[appShowActive]'
})
export class ShowActiveDirective {
  @Input() set appShowActive(item) {
    if(item.active) {
      this.viewContainer.createEmbeddedView(this.templateRef);
    }
  }
  constructor(private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef) {}
}
```

In HTML:

```
<li *ngFor="let menuItem of menuItems">
  <span *appShowActive="menuItem">{{ menuItem.title }}</span>
</li>
```