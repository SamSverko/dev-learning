# [angular-8](https://angular.io/)

---

## [Your first app](https://angular.io/start)

### Template syntax

* `<div *ngFor="let product of products"></div>` Structural directive, where it manipulates the DOM structure.
* `{{ product.name }}` Interpolation renders a property's value as text.
* `<a [title]="product.name">{{ product.name }}</a>` Property binding lets you use the property value in a template expression.
* `<p *ngIf="product.description">Description: {{ product.description }}</p>` Structural directive, where the element is only created if the current product has a description.
* `<button (click)="share()">Share</button>` Event binding is done by using `()` around the event, where this one binds the click event to the `share()` funciton.

### Components

* A component is comprised of 3 things:
	* **A component class:** Handles data and functionality.
	* **An HTML template:** Determines what is presented to the user.
	* **Component-specific styles:** Defines the look and feel.

### Input

* Inside a component's class, `@Input() product;` indicates that the property value will be passed in from the component's parent.

### Output

* Inside a component's class, `@Output() notify = new EventEmitter();` indicates that the component will emit an event when the value of the notify property changes.

---

## Routing

### Registering a route

* `{ path: 'products/:productId', component: ProductDetailsComponent }` Added to the `RouterModule` in the main `app.module.ts` file directs a route.
* `<a [title]="product.name + ' details'" [routerLink]="['/products', productId]"> {{ product.name }} </a>` This `RouterLink` directive gives the router control over the anchor element.

---

## Deployment

### Building locally

* `npm install -g @angular/cli` Install the Angular CLI globally.
* `ng new [my-project-name]` Creates a new Angular CLI workspace.

---

## General

* `ng serve --open` Launch server, watch files, and rebuilds app as you make changes. The `--open` flag opens the browser to the server `http://localhost:4200/`.
* `ng generate component heroes` Generates a new component in the `src/app/heroes` directory, and generates the files needed for the `HeroesComponent` component.
* `[(ngModel)]` is Angular's two-way data binding syntax.
