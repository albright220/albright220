# Category Expansion Panels: Generation and Testing

---

| Table of Contents                                              |
| -----------------                                              |
| [Overview and Background](#Overview-and-Background)            |
| [Material UI Expansion Panel](#Material-UI-Expansion-Panel)    |
| [Angular Tools](#Angular-Tools)  |
| [Auto-generating Expansion Panels by Category](#Auto-generating-Expansion-Panels-by-Category) |
| [Testing Expansion Panels With Cypress](#Testing-Expansion-Panels-With-Cypress) |

## Overview and Background

In this documentation, we go over how to implement the [Material Design Expansion Panel](https://material.angular.io/components/expansion/overview) for use in displaying categorized lists of items (in our examples, arrays of categorized Product objects).

### Material UI Expansion Panel

#### What is a `<mat-expansion-panel>`?

A `<mat-expansion-panel>` is a component from the [Angular Material UI component library](https://material.angular.io/components/categories) which lets you display content through a `<mat-expansion-panel-header>`, which can have both a `<mat-expansion-title>` and a `<mat-expansion-description>` to display content, as well as a clickable dropdown content area, which is populated by whatever you add within the `<mat-expansion-panel>` tags, but outside of the header tags.

When using multiple `<mat-expansion-panel>`, you can nest them within a `<mat-accordion>`, which allows you to control aspects of all panels such as opening and closing, and logically group them together.

### Angular Tools

#### What is `ngForOf`?

[ngForOf](https://angular.io/api/common/NgForOf#description), shorthand `*ngFor` is an Angular tool that allows us to iterate over items in any iterable structure, repeating and displaying the anchored html element for each iteration.

For Example:

```HTML
*ngFor="let element of listOfElements"
```

Using this inside of an [HTML DOM element](https://www.bing.com/ck/a?!&&p=25822109a365e1fc1aa5d071e62bc5ca5a92517b0499e802356fa2b588f74755JmltdHM9MTY0OTcwODAyMCZpZ3VpZD1hOWRlMWZmYS0wZDA3LTRiOTgtYjk4Yy01ZDNjMTA5MTYxMDYmaW5zaWQ9NTE3NA&ptn=3&fclid=e0dd41c9-b9d3-11ec-9f96-8d1071fef101&u=a1aHR0cHM6Ly93d3cudzNzY2hvb2xzLmNvbS9qc3JlZi9kb21fb2JqX2FsbC5hc3A_bXNjbGtpZD1lMGRkNDFjOWI5ZDMxMWVjOWY5NjhkMTA3MWZlZjEwMQ&ntb=1), e.g. `<h1>, <table>, <span>` or Angular component or template, e.g. `<mat-expansion-panel>, <mat-card>, <ng-template>`, acts like a for loop, repeating the html element for every iteration over an iterable collection.

One way to use *ngFor is to iterate over an array. In the world of Angular projects, you can define an array in your components typescript file:

```Typescript
let elementArray: string[] = ["pass 1", "pass 2", "pass 3"]
```

And then use an `*ngFor` to loop over it in the html:

```HTML
<div *ngFor="let element of this.elementArray">
    <p> This will show up three times </p>
</div>
```

This will repeat this div element and its subelements for the three elements inside of the elementArray.

When the webpage is generated, it will display the following:

```Markdown
---
This will show up three times
This will show up three times
This will show up three times
---
```

#### What is `ngClass`?

[ngClass](https://angular.io/api/common/NgClass#description) is another angular tool which allows us to add or remove css classes on html elements.

#### What is interpolation?

Angular [interpolation](https://angular.io/guide/interpolation?msclkid=12868734ba0911ecada4ec97a08d8748) allows you to embed expressions within html, dynamically showing the result of the expression as displayed text. The syntax for this is to add the expression between two sets of curly braces inside of a set of html tags, like so:

<!-- {% raw %} -->
```HTML
<open-tag> {{ expressionToEvaluate }} <close-tag>
```
<!-- {% endraw %} -->

As an example, if you define a string variable `greeting` within the typescript file:

```Typescript
const greeting: string = 'Hello, friend!'
```

Then you can evaluate this variable and display it in the html using interpolation:

<!-- {% raw %} -->
```HTML
<p> {{ greeting }} <p>
```
<!-- {% endraw %} -->

This will display as:

```Markdown
Hello, friend!
```

## Auto-generating Expansion Panels by Category

Now that we know what these three angular tools are and how they are used, let's learn how to generate our category expansion panels. We will start off by looking at a chunk of code from [Team Rocinante's 2nd Iteration Pantry App](https://github.com/UMM-CSci-3601-S22/it-2-rocinante), inside of the `product-list.component.html`:

<!-- {% raw %} -->
```HTML
<mat-accordion>
    <mat-expansion-panel *ngFor="let productCategory of (this.categoryNameMap | keyvalue)"
                        [ngClass]="productCategory.key.replace(' ', '-') + '-expansion-panel'">
        <mat-expansion-panel-header>
            <mat-panel-title [ngClass]="productCategory.key.replace(' ', '-') + '-panel-title capitalize'">
                {{ productCategory.key }}
            </mat-panel-title>
        </mat-expansion-panel-header>

        <mat-nav-list [ngClass]="productCategory.key.replace(' ', '-') + '-nav-list'">
            <span fxLayout="row" *ngFor="let product of productCategory.value">
                <a mat-list-item [routerLink]="['/products', product._id]" class="product-list-item">
                    <h3 matLine class="product-list-name"> <font size="4"><b> {{product.productName}} </b></font> </h3>
                    <p matLine class="product-list-brand"> {{product.brand}} </p>
                    <p matLine class="product-list-category capitalize"> {{product.category}} </p>
                    <p matLine class="product-list-store"> {{product.store}} </p>
                    <mat-divider></mat-divider>
                </a>
            </span>
        </mat-nav-list>

    </mat-expansion-panel>
</mat-accordion>
```
<!-- {% endraw %} -->

### Mat Expansion Panel

Let's start by looking at the opening `<mat-expansion-panel>` tag:

```HTML
...
    <mat-expansion-panel 
        *ngFor="let productCategory of (this.categoryNameMap | keyvalue)"
        [ngClass]="productCategory.key.replace(' ', '-') + '-expansion-panel'"
    >
...
```

Within the `*ngFor`, we loop over a predefined array of objects `categoryNameMap`, which consists of product category keys and arrays of products as values.

```HTML
*ngFor="let productCategory of (this.categoryNameMap | keyvalue)"
```

The [KeyValuePipe](https://angular.io/api/common/KeyValuePipe#description) being applied to the categoryNameMap, `(this.categoryNameMap | keyvalue)` will convert the objects inside into arrays of *keys* and arrays of *values*.

The array of keys is then used inside of the ngClass to dynamically generate class names for the expansion panels based on the category as they are generated:

```HTML
[ngClass]="productCategory.key.replace(' ', '-') + '-expansion-panel'"
```

Where `productCategory.key` is the key of the current object being iterated over inside of the `categoryNameMap`, which would be a category.

The `.replace(' ', '-')` is necessary to replace any spaces in category names with hyphens, so that no extra classes are generated after spaces.

### Mat Expansion Panel Header

Next let's look at the `<mat-expansion-panel-header>`:

<!-- {% raw %} -->
```HTML
<mat-expansion-panel-header>
    <mat-panel-title [ngClass]="productCategory.key.replace(' ', '-') + '-panel-title capitalize'">
        {{ productCategory.key }}
    </mat-panel-title>
</mat-expansion-panel-header>
```
<!-- {% endraw %} -->

We use the `<mat-panel-title>` tag to set the title text for our expansion panel. Just like with the `<mat-expansion-panel>`, we use `ngClass` to dynamically generate the CSS class names for our title. Here we add an additional class named `capitalize`.

For the title text, we use Angular interpolation syntax to display the `productCategory.key`:

<!-- {% raw %} -->
```HTML
...
    {{ productCategory.key }}
...
```
<!-- {% endraw %} -->

Lastly let's look at the `<mat-nav-list>`:

<!-- {% endraw %} -->
```HTML
<mat-nav-list [ngClass]="productCategory.key.replace(' ', '-') + '-nav-list'">
    <span fxLayout="row" *ngFor="let product of productCategory.value">
        <a mat-list-item [routerLink]="['/products', product._id]" class="product-list-item">
            <h3 matLine class="product-list-name"> <font size="4"><b> {{product.productName}} </b></font> </h3>
            <p matLine class="product-list-brand"> {{product.brand}} </p>
            <p matLine class="product-list-category capitalize"> {{product.category}} </p>
            <p matLine class="product-list-store"> {{product.store}} </p>
            <mat-divider></mat-divider>
        </a>
    </span>
</mat-nav-list>
```
<!-- {% endraw %} -->

Just like before, we use `ngClass` to dynamically generate unique names for each `<mat-nav-list>` based on the category. in the `<span>`, we also use `*ngFor` once again to iterate over each array of products mapped to each category. We then use interpolation to display values within each individual product object on each `<mat-list-item>`.

## Testing Expansion Panels With Cypress

Testing expansion panels is similar to testing normal text on a page: we check that the expansion title has the correct text, and then we check that the contents have the correct text.

At this point is when generating each expansion panel with unique class names comes in handy, because now we can use `cy.get()` on our css classes without worrying about how angular identifies and evaluates its custom elements like `<mat-expansion-panel>`.

Let's start off by looking at a Cypress e2e test for [Team Rocinante's](https://github.com/UMM-CSci-3601-S22/it-2-rocinante) product category expansion panels:

```Typescript
it('Should check that expansion panels have the correct titles and items by categories', () => {

    page.getExpansionTitleByCategory('baked goods')
        .should('have.text', ' baked goods ');

    page.getExpansionItemsByCategory('baked goods').each($product => {
      cy.wrap($product).find('.product-list-category')
        .should('have.text', ' baked goods ');
    });

    page.getExpansionTitleByCategory('miscellaneous')
        .should('have.text', ' miscellaneous ');

    page.getExpansionItemsByCategory('miscellaneous').each($product => {
        cy.wrap($product).find('.product-list-category')
            .should('have.text', ' miscellaneous ');
    });
});
```

First let's go over `getExpansiontitleByCategory(category)` and `getExpansionItemsByCategory(category)`. These two methods are defined in the `.po` file:

```Typescript
getExpansionTitleByCategory(category: string) {
    return cy.get('.' + category.replace(' ', '-') + '-expansion-panel .' + category.replace(' ', '-') + '-panel-title');
  }

getExpansionItemsByCategory(category: string) {
    return cy.get('.' + category.replace(' ', '-') + '-expansion-panel .' + category.replace(' ', '-') + '-nav-list .product-list-item');
  }
```

Both of these method take one string argument, which is the category. They then take the category get the css category specific classes assigned to parts of each expansion panel.

Using `page.getExpansionTitleByCategory('baked goods')` will go to the baked goods expansion panel title on the page, allowing us to use the cypress expect `.should('have.text', ' baked goods')` to test that the title has the correct category text.

The same is done with `page.getExpansionItemsByCategory('baked goods')`, except here we use...

```Typescript
.each($product => {cy.wrap($product).find('.product-list-category')...})`
```

to iterate over each list item (given the css class .product-list-item) inside of the `<mat-nav-list`> (css class .bakery-nav-list). The `cy.wrap($product)` lets us cover and focus on the element, and the `.find('.product-list-category')` grabs the text inside of the html element with the product-list-category css class, which contains the category name.

Finally, we use the Cypress `.should('have.text', ' baked goods ')` to check that the category text matches what the category should be.

In conclusion, this document is too long and you probably won't read this far. If you do, congratulations! You don't win anything but good job reading the whole thing.
