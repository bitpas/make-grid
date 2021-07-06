# @bitpas/make-grid

**Unstable until v1. Updates may include breaking changes. Use at your own risk.**

Extensible base helpers for making CSS grids.

## Installation

Install `@bitpas/make-grid` with npm.

```sh
npm install @bitpas/make-grid
```

## Basic usage

Import `make-grid` into your project.

```scss
@import '@bitpas/make-grid';
```

Include the `make-grid` mixins in your selectors.

```scss
.row {
  @include make-grid-row;
}

.col {
  @include make-grid-col;
  @include make-grid-props;
}
```

By default, `make-grid-props` creates size selectors to set an element's width. The default selectors are now available to use in your project's markup.

```html
<div class="row">
  <div class="col size1of2">half</div>
  <div class="col size1of4">fourth</div>
  <div class="col size1of8">eighth</div>
  <div class="col size1of8">eighth</div>
</div>
```

### Default selectors

In order to keep file size small, `make-grid` currently outputs simplified fractions. For example, `size2of4` is invalid since it simplifies to `size1of2`. Proposed behavior changes are open to discussion.

```
size1of2
size1of3
size2of3
size1of4
size3of4
size1of6
size5of6
size1of8
size3of8
size5of8
size7of8
size1of12
size5of12
size7of12
size11of12
```

## Customizing

The `make-grid-props` mixin takes optional arguments to customize its output.

```scss
make-grid-props($property, $pre, $post);
```

You can use `make-grid-props` to set any CSS property that accepts a percentage as a valid value by passing the property name as a string to `$property`.

Prepend the generated selector name—overriding the default `size` prefix—by passing any string to `$pre`. For example, passing `foo__` as an argument to `$pre` makes the selector accessible as `foo__1of2` in the markup.

Append the generated selector name by passing any string to `$post`. For example, passing `--bar` as an argument to `$post` makes the selector accessible as `size1of2--bar` in the markup.

Leverage the `make-grid-props` helper to construct grid based tools such as offsets and responsive selectors (discussed in the next section):

```scss
.col {
  @include make-grid-col;
  @include make-grid-props;
  @include make-grid-props('margin-left', 'offset');
}
```

```html
<div class="row">
  <div class="col size1of2 offset1of2">offset by half</div>
</div>
```

## Responsive grids

This package is unopinionated about responsiveness and currently excludes built in handling of breakpoints. Responsive support is relatively simple to setup as shown in the following use case.

```scss
$project-breakpoints: (
  phone: 340px,
  tablet: 640px,
  desktop: 1024px,
);

@function get-breakpoint($bp) {
  map-get($project-breakpoints, $bp);
}

.col {
  @include make-grid-col;
  @include make-grid-props('width', 'size');

  @each $breakpoint, $val in $project-breakpoints {
    @media (min-width: get-breakpoint($breakpoint)) {
      @include make-grid-props('width', 'size', #{-$breakpoint});
    }
  }
}
```

Responsive selectors are now available to use in your project's markup.

```html
<div class="row">
  <div class="col size2of3 size1of2-tablet size5of12-desktop">responsive</div>
</div>
```
