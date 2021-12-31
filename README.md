# tailwindcss-def

## What does this plugin do?

This plugin adds a new `def:` variant to your Tailwind CSS configuration. Any class using this variant _can be overridden by other TailwindCSS classes_.

So, this allows you to create a **component** (Vue, React, etc.) that has _default_ classes for various styles, but when you _use_ that component, you can override those defaults _with normal Tailwind (or other) classes_.

There's no run-time JavaScript involved, this is all pure CSS.

## How do I use it?

### 1. Install

```sh
npm install tailwindcss-def
```

### 2. Add this to your `tailwindcss.config` file in the `module.exports` object:

```js
plugins: [
	require('tailwindcss-def')(),
],
```

### 3. Create a component with a default style

```HTML
<!-- MyButton.vue -->
<template>
	<button class="font-semibold def:bg-blue-500"><slot/></button>
</template>
```

### 4. Override as needed for your component instances

```HTML
<MyButton>My Blue Button</MyButton>
<MyButton class="bg-red-500">My Red Button</MyButton>
```

## Which classes should I default?

Whichever ones you want the component user to be able to easily override. Probably not everything.

## Caveats

1. Only one "level" of override is possible using this approach. You can't override an override. Well, you can, but you're back to the normal limitations of either using a class that is added before Tailwind's classes, or that has a higher CSS selector specificity (_e.g._ a deeper selector or an `!important` directive).

2. I haven't tested this with more complex classes like media queries.

3. This CSS feature is [not supported](https://caniuse.com/mdn-css_selectors_where) in IE11 or Safari 13.x and below.

4. Like any variant, using this will increase your CSS bundle, since the `def:` variants are defined as separate rules from the classes they are based on.

## How does it work?

This variant wraps your class in a `:where()` selector. These have a lower specificity than a class, which allows them to be overridden by a simple class.

## Why did I create this plugin?

I've been making web apps since 1995, and I really enjoy using Tailwind CSS.

But as a component author, there are also a few annoyances with the utility-class approach. One is that I like to create components that have _reasonable default styles_, but that _also_ allow the component user (usually me!) to "tweak" the style later. But with TailwindCSS's "flat" approach (there's not much actual "cascading" going on, just lots of individual classes), this becomes difficult. The usual recommendationa are:

1. **Don't style your components.** I don't like this approach. I like reasonable, beautiful defaults.

2. **Pass overridable styles as component properties.** 1995 called, it wants its `<FONT COLOR>` back!

3. **Use a higher-level selector in your component instance.** Yes, but that means having to go back to semantic classes and deeper selectors, and either not being able to use TailwindCSS classes for those component instances, or having to use them with `@apply` so they can consistently override the default.

4. **CSS-in-JS**. _I.e._, try to rewrite the whole logic of "what overrides what" in JavaScript and apply the "winning" classes/styles dynamically. A few people are working on this, but it feels kludgy, slow, and brittle to me for 99% of use cases.

So, I [came up with this idea](https://github.com/tailwindlabs/tailwindcss/discussions/3523) earlier this year, and decided it was finally time to make it a reusable plugin.

## License

MIT
