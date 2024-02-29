#Angular
## Intoduction
Angular is a development platform built on TS [[Skills/TS/Documentation|Documentation]].
It includes several main features:
	1) Building scalable application;
	2) Convenient libs and built in components (default) e.g. routing, forms management, SSR ([[Server side rendering]]);
	3) Default suitable tools for developers
Angular is used both for single-person project and enterprise-level apps. As usual Angular uses [[CLI]] and TS [[Skills/TS/Documentation|Documentation]]. All entities in angular use suffixes.
## Module
Modules are a great way to organize an application and extend it with capabilities from external libraries.
NgModule metadata does the following:
	1) Declares which components, directives, and pipes belong to the module (`declarations`)
	2) Makes some of those components, directives, and pipes public so that other module's component templates can use them
	3) Imports other modules with the components, directives, and pipes that components in the current module need (`imports`)
	4) Provides services that other application components can use (`providers`)
The Angular [[CLI]] generates the following basic `AppModule` when creating a new application.
Every Angular application has at least one module, the root module. You `bootstrap` that module to launch the application.
```typescript
// imports
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';

// @NgModule decorator with its metadata
@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```
## Components
**name.component.ts**
Components are basic block in angular app. It usually used for maintaining scalability in project. It has a decorator to define options of copmponent.
	`standalone` - Standalone components allows you to be independent from [module](#Module) in Angular;
	`selector` - Alias for name of component in other html templates;
	`template(Url)` - HTML template or url to the template;
	`styles(Url)` - Scoped stylesheet(s) of component;
	`imports` - Tools and libs that used in component (CommonModule, FormsModule etc.) when component is standalone.
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
### Pass data to a child component
For simple passing data you can use `@Input` or `Output`. Usage depends on where you pass the data *Parent-Child* or *Child-Parent*. 
Child example:
```typescript
export class ChildComponent {
@Input() product: Product | undefined;
@Output() notify = new EventEmitter();
}
```
Child template:
```html
<button type="button" (click)="notify.emit()">Notify Me</button>
```
Parent example:
```typescript
export class ProductListComponent {

  products = [...products];

  share() {
    window.alert('The product has been shared!');
  }

  onNotify() {
    window.alert('You will be notified when the product goes on sale');
  }
}
```
Parent template:
```html
<button type="button" (click)="share()">
  Share
</button>

<child
  [product]="product" 
  (notify)="onNotify()">
</child>
```
### Adding navigation
The application already uses the Angular `Router` to navigate to the `ProductListComponent`. This section shows you how to define a route to show individual product details. There is next principle adding routing **for modules**.
```typescript
@NgModule({
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    RouterModule.forRoot([
      { path: '', component: ProductListComponent },
      { path: 'products/:productId', component: ProductDetailsComponent },
    ])
  ],
  declarations: [
    AppComponent,
    TopBarComponent,
    ProductListComponent,
    ProductAlertsComponent,
    ProductDetailsComponent,
  ],
```
Template. `['/products', product.id]` is equal to '`/products' + product.id`.
```html
<div *ngFor="let product of products">

  <h3>
    <a
      [title]="product.name + ' details'"
      [routerLink]="['/products', product.id]">
      {{ product.name }}
    </a>
  </h3>

  <!-- . . . -->

</div>
```
Routed component.
```typescript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

import { Product, products } from '../products';
export class ProductDetailsComponent implements OnInit {

  product: Product | undefined;

  constructor(private route: ActivatedRoute) { }
  ngOnInit() {
  // First get the product id from the current route.
  const routeParams = this.route.snapshot.paramMap;
  const productIdFromRoute = Number(routeParams.get('productId'));

  // Find the product that correspond with the id provided in route.
  this.product = products.find(product => product.id === productIdFromRoute);
}

}
```
Template.
```html
<h2>Product Details</h2>

<div *ngIf="product">
  <h3>{{ product.name }}</h3>
  <h4>{{ product.price | currency }}</h4>
  <p>{{ product.description }}</p>
</div>
```
## Directives
**name.directive.ts**
Directives are used for handling behaviour of elements in angular. Examples of this include: displaying content based on a certain condition, rendering a list of items based on application data, changing the styles on an element based on user interaction, etc. There some examples of basical directives.
*\*ngIf*
```html
<section class="admin-controls" *ngIf="hasAdminPrivileges">
The content you are looking for is here.
</section>
```
*\*ngFor*
```html
<ul class="ingredient-list">
  <li *ngFor="let task of taskList">{{ task }}</li>
</ul>
```
*\*ngSwitch*
```html
<div [ngSwitch]="userRole">
  <p *ngSwitchCase="'admin'">Admin View</p>
  <p *ngSwitchCase="'user'">User View</p>
  <p *ngSwitchDefault>Guest View</p>
</div>
```
*\*ngClass*
```html
<div [ngClass]="{'active': isActive, 'disabled': isDisabled}">...</div>
```
*\*ngStyle*
```html
<div [ngStyle]="{'color': fontColor, 'font-size': fontSize + 'px'}">...</div>
```
*\*ngModel* (Two-way data binding)
```html
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