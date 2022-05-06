# **Directives**

## Attribute vs Structural

| [Attribute Directives](https://angular.io/guide/attribute-directives) | [Structural Directives](https://angular.io/guide/structural-directives) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Look like a normal HTML Attribute (possibly with data binding or event binding) | Look like a normal HTML Attribute but have a leading * (for desugaring) |
| Only affect/change the element they are added to             | Affect a whole area in the DOM (elements get added/removed)  |

### Attribute Directive

Directive is composed very simliarly to Components.

```typescript
import { Directive, OnInit, ElementRef, Renderer2 } from '@angular/core'

@Directive({
  selector: '[appBasicHighlight]'
})
export class BasicHighlightDirective implements OnInit {
  constructor(private renderer: ElementRef, private renderer: Renderer2) {}
  
  ngOnInit() {
    this.renderer.setStyle(this.elRef.nativeElement, 'backgroundColor', 'blue');
  }
}
```

Add the Directive as a declaration to the app module.

Update the HTML to use the directive.

```html
<p appBasicHighlight>
  Style me with basic directive!
</p>
```

More information about Renderer can be found here: https://angular.io/api/core/Renderer2

[HostListener](https://angular.io/api/core/HostListener) allows to listen to events to take an action.

```typescript
// remove rendered item from Oninit to HostListener
@HostListner('mouseenter') mouseover(eventData: Event) {
	this.renderer.setStyle(this.elRef.nativeElement, 'backgroundColor', 'blue');
}
@HostListner('mouseleave') mouseleave(eventData: Event) {
	this.renderer.setStyle(this.elRef.nativeElement, 'backgroundColor', 'transparent');
}
```

[HostBinding](https://angular.io/api/core/HostBinding) to bind to host properties.

```typescript
@HostBinding('style.backgroundColor') backgroundColor: string = 'transparent';

@HostListener('mouseenter') mouseover(eventData: Event) {
  this.backgroundColor = 'blue';
}
@HostListener('mouseleave') mouseleave(eventData: Event) {
  this.backgroundColor = 'transparent';
}

// Or use custom binding
@Input() defaultColor: string = 'transparent';
@Input() highlightColor: string = 'blue';
@HostBinding('style.backgroundColor') backgroundColor: string = this.defaultColor;

@HostListener('mouseenter') mouseover(eventData: Event) {
  this.backgroundColor = this.highlightColor;
}
@HostListener('mouseleave') mouseleave(eventData: Event) {
  this.backgroundColor = this.defaultColor;
}
```

Adjust HTML to  use the custom binding.

```html
<p appBasicHighlight [defaultColor]="'yellow'" [highlightColor]="'red'">
  Style me with a better directive!
</p>
```

### Structural Directive

```typescript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]'
})
export class UnlessDirective {
  @Input() set appUnless(condition: boolean) { //set to the same name as the directive
    if(!condition) {
      this.vcRef.createEmbeddedView(this.templateRef);
    } else {
      this.vcRef.clear();
    }
  }
  
  constructor(private templateRep: TemplateRef<any>, private vcRef: ViewContainerRef) {}
}
```

```html
<div *appUnless="onlyOdd">
  <li
      class="list-group-item"
      [ngClass]="{odd: even % 2 !== 0}"
      [ngStyle]="{backgroundColor: even % 2 !== 0 ? 'yellow' : 'transparent'}"
      *ngFor="let even of evenNumbers">
    {{ even }}
  </li>
</div>
```

NgSwitch can be used instead of NgIf if the condition gets too complicated or large.

```html
// Assuming the "value" in the directive has been set to 5
<div [ngSwitch]="value">
  <p *ngSwitchCase="5">Value is 5</p> // only this line will display
    <p *ngSwitchCase="10">Value is 10</p>
    <p *ngSwitchDefault>Value is Default</p>
</div>
```

