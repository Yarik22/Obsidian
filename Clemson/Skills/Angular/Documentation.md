#Angular
## Intoduction
Angular is a development platform built on TS [[Skills/TS/Documentation|Documentation]].
It includes several main features:
	1) Building scalable application;
	2) Convenient libs and built in components (default) e.g. routing, forms management, SSR ([[Server side rendering]]);
	3) Default suitable tools for developers
Angular is used both for single-person project and enterprise-level apps. As usual Angular uses [[CLI]] and TS [[Skills/TS/Documentation|Documentation]]. All entities in angular use suffixes.
## Module
## Components
**name.component.ts**
Components are basic block in angular app. It usually used for maintaining scalability in project. It has a decorator to define options of copmponent.
	`standalone` - Standalone components allows you to be independent from [module](#Module) in Angular;
	`selector` - Alias for name of component in other html templates ;
	`template(Url)` - HTML template or url to the template ;
	`styles(Url)` - Scoped stylesheet(s) of component;
	`imports` - Tools and libs that used in component (CommonModule, FormsModule etc.).
```typescript
@Component({
  standalone: true,
  selector: 'todo-list-item',
  template: ` <li>(TODO) Read cup of coffee introduction</li> `,
  styles: ['li { color: papayawhip; }'],
})
export class TodoListItem {
  /* Component behavior is defined in here */}```
Each component may have state and privaty fields, methods that can be used in templates that refer to this copmponent. Templates and styles can be shown as plain or url to template file.
```typescript
@Component({ ... })
export class TodoList {
  taskTitle = '';
  isComplete = false;

  updateTitle(newTitle: string) {
    this.taskTitle = newTitle;
  }

  completeTask() {
    this.isComplete = true;
  }
}```
Each template is rendered to the DOM tree and poses a template of html and ts syntax.
```html
//{{}} - Interpolation
//() - Event binding
//[] - Property, attribute, class, style binding
<p>Title: {{ taskTitle }}</p>

<button (click)="handleClick()">Title: {{ taskTitle }}</button>
<img [src]="imgUrl"/>
<div [attr.role]="userRole"> Some text...<div/>
<div [class.active]="isActive"> Some text...<div/>
<div [style.color]="fontColor"> Some text...<div/>
```
## Directives
**name.directive.ts**
Directives are used for handling behaviour of elements in angular. Examples of this include: displaying content based on a certain condition, rendering a list of items based on application data, changing the styles on an element based on user interaction, etc. There some examples of basical directives.
*\*ngIf*
```typescript
<section class="admin-controls" *ngIf="hasAdminPrivileges">
The content you are looking for is here.
</section>
```
*\*ngFor*
```typescript
<ul class="ingredient-list">
  <li *ngFor="let task of taskList">{{ task }}</li>
</ul>
```
*\*ngSwitch*
```typescript
<div [ngSwitch]="userRole">
  <p *ngSwitchCase="'admin'">Admin View</p>
  <p *ngSwitchCase="'user'">User View</p>
  <p *ngSwitchDefault>Guest View</p>
</div>
```
*\*ngClass*
```typescript
<div [ngClass]="{'active': isActive, 'disabled': isDisabled}">...</div>
```
*\*ngStyle*
```typescript
<div [ngStyle]="{'color': fontColor, 'font-size': fontSize + 'px'}">...</div>
```
*\*ngModel* (Two-way data binding)
```typescript
<input type="text" [(ngModel)]="username">
```
Example of custom directive
```typescript
@Directive({
  selector: '[appHighlight]',
})
export class HighlightDirective {
  private el = inject(ElementRef);
  constructor() {
    this.el.nativeElement.style.backgroundColor = 'yellow';
  }
}```
And usage
```html
<p appHighlight>Look at me!</p>
```

## Services
**name.service.ts**
Services are usually used for business logic sharing throug dependency injection pattern.
	`providedIn` - This allows you to define what parts of the application can access the service. For example, ‘root’ will allow a service to be accessed anywhere within the application.
```typescript
import {Injectable} from '@angular/core';
@Injectable({
  providedIn: 'root',
})
class CalculatorService {
  add(x: number, y: number) {
    return x + y;
  }
}```
Recieving service in the component:
```typescript
import { Component } from '@angular/core';
import { CalculatorService } from './calculator.service';

@Component({
  selector: 'app-receipt',
  template: `<h1>The total is {{ totalCost }}</h1>`,
})
export class Receipt {
  //private calculatorService = inject(CalculatorService);
  totalCost = this.calculatorService.add(50, 25);
  constructor(calculatorService:CalculatorService){
  }
}
```