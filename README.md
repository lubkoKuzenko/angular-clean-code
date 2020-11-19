# Angular Clean Code

<img src="https://webref.ru/assets/images/angular-cookbook/angular.png" width="960">

## Table of Content

- [Introduction](#Introduction)
- [Angular Architecture](#angular-architecture)
  - [Project structure](#project-structure)
  - [Data flow architecture](#data-flow-architecture)
    - [Change Detection](#Change-Detection)
  - [State management](#state-management)
- [Angular Forms](#angular-forms)
  - [Basic setup](#Basic-setup)
  - [Custom FormGroup Validator](#Custom-FormGroup-Validator)
  - [Custom FormControl Validator](#Custom-FormControl-Validator)
  - [Child forms](#Child-forms)
- [Angular Routing](#angular-routing)
  - [Custom RouteReuseStrategy](#Custom-RouteReuseStrategy)

## Introduction

In order to maintain high quality of delivery and prevent technical debt from being created, we had to agree to a series of guidelines and good practices of how to plan, structure and write applications in Angular

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Angular Architecture

**Resources**

- ["Angular Application Architecture"](https://bulldogjob.pl/articles/539-scalable-angular-application-architecture)
- ["Change Detection in Angular"](https://vsavkin.com/change-detection-in-angular-2-4f216b855d4c)
- ["Everything you need to know about change detection in Angular"](https://blog.angularindepth.com/everything-you-need-to-know-about-change-detection-in-angular-8006c51d206f)

### What is a scalable architecture?

First of all, what does it mean that GUI application is scalable? GUI always runs as a single, separate application for every user, so there is no "high number of users" challenge that is typical for backend applications. Instead, GUI has to deal with the following scalability factors: increasing size of data loaded to the application, growing complexity and size of the project, usually followed by longer loading times.

There is also a part of the problem that is not visible from the outside, namely how an application scales from the programmer’s point of view. An application with bad, or not scalable architecture, tends to be very hard to develop after some time. Increasing complexity, technical debt and simply code smell, has a direct impact on project estimates, costs and the quality of the overall solution.

Good architecture does not guarantee that above problems will not occur in your application, however, it gives the development team a powerful tool to reduce and even eliminate most of the issues that they might encounter.

In short, well designed architecture should work equally good for small and big applications, should provide very good user experience regardless of the application’s size and amount of processed data. Additionally, it should provide a set of clear and easy to follow rules for developers in order to sustain the quality of the project. And finally, it should be simple and preferably based on widely accepted design patterns. We want to keep the learning curve of our applications as small as possible.

### Project structure

Application modules are clearly visible in the file tree, as separate directories. Every module directory contains all files (code, styles, templates etc.) that are related to a given module. A very important element of this approach is isolation of modules. Simply speaking, it means that every module is self-contained and does not refer to files from different modules, so, theoretically, you can delete one of them from the application, and the rest will work without any problems.

<img src="./assets/content_1.png" width="373" height="549">

Obviously, it’s not possible to strictly follow this rule in the real world. At least some services and components have to be reused across the whole application. Therefore, some parts of application functionality are stored in "Core" and "Shared" modules. Now our application structure looks like this:

<img src="./assets/content_2.png" width="521" height="331" style="background-color: #ffffff; padding: 2rem">

As you can see, there are now three main modules in the project:

- AppModule – the bootstrapping module, responsible for launching the application and combining other modules together

- CoreModule – core functionalities, mostly global services, that will be used in the whole application globally. They should not be imported by other application modules

- SharedModule – usually a set of components or services that will be reused in other application modules, not applied globally. They can be imported by feature modules.

All remaining modules (so-called feature modules) should be isolated and independent. Such a structure not only allows for clear concerns separation, but is also a convenient starting point for implementing lazy loading functionality, another crucial step in preparing a scalable application architecture.

### Data flow architecture

Let's start with data flow. Angular 2+, unlike its predecessor, prefers a one way data flow. This kind of approach is much easier to maintain and follow than two-way data binding. It is more obvious what is the source of data in a given module and how the change is propagated through the system. In Angular, data flows from top to bottom. From the parent component to the child component and from the component to the template.

Still, it's a good idea to add some additional set of rules over the basic data flow principles.

In our application we've introduced the idea of "smart" and "dummy" components. The smart components are also called "Containers". The idea behind this division is to clearly define the parts of the application that contain some logic, communicate with services and cause side effects (like service calls, state updates etc.). Every such action is implemented only in Containers. On the contrary, "stupid" components have very little or no logic at all. All the data they need is passed by @Input parameters. If a component wants to communicate with the outside word, it has to emit an event (via @Output attribute).

<img src="./assets/content_6.png" width="641" height="201" style="background-color: #ffffff; padding: 2rem">

Such architectural approach is intended to keep the number of Containers as small as possible. The more components in the application are "dummy", the simpler is the data flow and the easier it is to work with it. Deciding which component should take the role of a Container and which should be just a plain component is not a trivial task and needs to be resolved per particular case. However, usually the first step we take is assuming that a main screen component should be the smart one, as in the example below, when the container is marked with blue color and simple components are gray.

Such an approach to architecture is not only about readability of code and organized data flow. Dummy component are much easier to test. Their state is entirely induced by the Input they are provided with, they cause no side effect and the result of any component action is visible as a proper event being fired.

What is more, such behavior nicely corresponds with performance optimization of Angular’s change detection process. The change detection strategy for dummy components can be set to "onPush" which will trigger the change detection process for the component only when the input properties have been modified. It's an easy and very efficient method of optimizing Angular applications.

### Presentational Components

- are purely user interface and concerned with how things look.
- are not aware about the business logic, or services.
- receive data via @Inputs, and emit events via @Output.

### Container Components

- contain all the business logic.
- pass the data to the Presentational Components, and handle @Output events raised by them.
- have no UI logic.
- do have dependencies on other parts of your app, like services, or your state store.

#### Change Detection

On each asynchronous event, Angular performs change detection over the entire component tree. Although the code which detects for changes is optimized for [inline-caching](http://mrale.ph/blog/2012/06/03/explaining-js-vms-in-js-inline-caches.html), this still can be a heavy computation in complex applications. A way to improve the performance of the change detection is to not perform it for subtrees which are not supposed to be changed based on the recent actions.

#### `ChangeDetectionStrategy.OnPush`

The `OnPush` change detection strategy allows us to disable the change detection mechanism for subtrees of the component tree. By setting the change detection strategy to any component to the value `ChangeDetectionStrategy.OnPush` will make the change detection perform **only** when the component has received different inputs. Angular will consider inputs as different when it compares them with the previous inputs by reference, and the result of the reference check is `false`. In combination with [immutable data structures](https://facebook.github.io/immutable-js/), `OnPush` can bring great performance implications for such "pure" components.

### State management

#### Facade Design Pattern

Facade discusses encapsulating a complex subsystem within a single interface object. This reduces the learning curve necessary to successfully leverage the subsystem. It also promotes decoupling the subsystem from its potentially many clients.
The Facade object should be a fairly simple advocate or facilitator. It should not become an all-knowing oracle or “god” object.
Here is the good read for Facade design pattern in details

<img src="./assets/example-of-facade-designpattern-in-uml.png" width="100%">

I would recommend following steps to build Angular services using Facade pattern:

- Define all your Angular services as per your business requirement and/or keep adding more as needed.
- Create a service called “FacadeService” (feel free to use any other name here)
- Create a Module and provide all Angular services

```ts
// user.service.ts
import { Injectable } from "@angular/core";

@Injectable()
export class UserService {
  constructor() {}

  getUsers() {
    return [{ name: "test" }];
  }
}
```

```ts
// facade.service.ts
@Injectable()
export class FacadeService {
  // users
  public users$ = new BehaviorSubject<User[]>([]);

  constructor(public userService: UserService) {}

  public getUsers() {
    return this.users$.next(this.userService.getUsers());
  }
}
```

```ts
// ./containers/users.component.ts
@Component({
  templateUrl: "./users.html",
  styleUrls: ["./users.scss"]
})
export class UsersComponent implements OnInit {
  public users$ = this.facadeService.users$;

  constructor(public facadeService: FacadeService) {}

  public ngOnInit(): void {
    this.facadeService.getUsers();
  }
}
```

```html
<!-- ./containers/users.component.html  -->
{{ users$ | async | json }}
```

```ts
// feature.module.ts
@NgModule({
  imports: [CommonModule],
  declarations: [],
  providers: [UserService, FacadeService]
})
export class FeatureModule {}
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Angular Forms

**Resources**

- ["Custom Form Validators"](https://codecraft.tv/courses/angular/advanced-topics/basic-custom-validators/)
- ["Custom Form Validation in Angular"](https://www.digitalocean.com/community/tutorials/angular-custom-validation/)

- ["Reactive FormGroup validation with AbstractControl in Angular"](https://ultimatecourses.com/blog/reactive-formgroup-validation-angular-2/)

### Basic setup

```ts
import {
  ChangeDetectionStrategy,
  Component,
  Input,
  OnInit
} from "@angular/core";
import { FormControl, FormGroup, Validators } from "@angular/forms";

@Component({
  selector: "lib-form",
  templateUrl: "./form.component.html",
  styleUrls: ["./form.component.scss"],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class FormGeneralComponent implements OnInit {
  public form = new FormGroup({
    name: new FormControl("", [Validators.required]),
    description: new FormControl(undefined, [Validators.required]),
    status: new FormControl(1, [Validators.required])
  });

  get controls() {
    return this.form.controls;
  }

  public ngOnInit() {
    this.initializeFormValues();
  }

  // reset form
  public reset() {
    this.form.reset();
  }

  // populate form values
  public initializeFormValues() {
    this.form.patchValue({
      name: "name",
      description: "description",
      status: 2
    });
  }

  // GET form values
  public onSubmit() {
    this.form.getRawValue();
  }
}
```

### Custom FormGroup Validator

```ts

// form.component.ts

import { atLeastOneRequiredValidator } from "./validators.ts";

public form = new FormGroup({
  programName: new FormGroup(
    {
        program: new FormControl(""),
        switch: new FormControl(""),
        intervention: new FormControl(""),
    },
    { validators: atLeastOneRequiredValidator() },
  ),
});
```

```ts
// validators.ts
import { ValidatorFn, FormGroup, ValidationErrors } from "@angular/forms";

export const atLeastOneRequiredValidator = (): ValidatorFn => {
  return (group: FormGroup): ValidationErrors => {
    const control1 = group.controls["program"];
    const control2 = group.controls["switch"];
    const control3 = group.controls["intervention"];

    if (
      control1.value === "" &&
      control2.value === "" &&
      control3.value === ""
    ) {
      return { empty: true };
    }

    return null;
  };
};
```

### Custom FormControl Validator

```ts
// form.component.ts
import { ValidatePhone } from "./validators.ts";

public form = new FormGroup({
  phone: ['', [ValidatePhone]]
});
```

```ts
// validators.ts
export const ValidatePhone = (): ValidatorFn => {
  return (control: AbstractControl): ValidationErrors => {
    if (control.value && control.value.length != 10) {
      return { phoneNumberInvalid: true };
    }
    return null;
  };
};
```

### Child forms

TODO

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Angular Routing

**Resources**

- ["Angular Component Reuse Strategy"](https://medium.com/@juliapassynkova/angular-2-component-reuse-strategy-9f3ddfab23f5/)

### Custom RouteReuseStrategy

RouteReuseStrategy decides on whether the router should store the current route when deactivating it or whether the router should restore it when the user re-activates it.

```ts
// app.module.ts
import { RouteReuseStrategy } from "@angular/router";
import { CustomReuseStrategy } from "./router-reuse.strategy.ts";

@NgModule({
  declarations: [],
  imports: [],
  providers: [{ provide: RouteReuseStrategy, useClass: CustomReuseStrategy }],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

````ts
// router-reuse.strategy.ts
import { Injectable } from "@angular/core";
import {
  RouteReuseStrategy,
  ActivatedRouteSnapshot,
  DetachedRouteHandle
} from "@angular/router";
/**
 * Based on Angular `DefaultRouteReuseStrategy`.
 * Reuses routes as long as their route config is the same OR until future route data has pattribute `noReuse: true`
 *
 * @example ```json
 *   {
 *       path: "overview",
 *       component: OverviewComponent,
 *        data: {
 *            noReuse: true,
 *        },
 *    },
 * ```
 */
@Injectable()
export class CustomReuseStrategy implements RouteReuseStrategy {
  public shouldDetach(route: ActivatedRouteSnapshot): boolean {
    return false;
  }

  public store(
    route: ActivatedRouteSnapshot,
    detachedTree: DetachedRouteHandle
  ): void {}

  public shouldAttach(route: ActivatedRouteSnapshot): boolean {
    return false;
  }

  public retrieve(route: ActivatedRouteSnapshot): DetachedRouteHandle | null {
    return null;
  }

  public shouldReuseRoute(
    future: ActivatedRouteSnapshot,
    curr: ActivatedRouteSnapshot
  ): boolean {
    if (future.data && Boolean(future.data.noReuse)) {
      return !future.data.noReuse;
    }
    return future.routeConfig === curr.routeConfig;
  }
}
````
