Happy Medium CSS Style Guide
===============

Welcome to Happy Medium's internal CSS style guide. We use this for any project involving websites at Happy Medium. You should have a general idea of **specificity** and the [SCSS syntax](http://sass-lang.com/).

This style guide is modeled off of [Github's CSS Styleguide](https://github.com/styleguide/css).

## Coding Style

* Use soft-tabs with a four space indent (for readability).
* Put spaces after `:` in property declarations.
* Put spaces before `{` in rule declarations.
* Use hex color codes `#000` unless using rgba.
* Use `//` for comment blocks (instead of `/* */`).
* Document styles gratuitously (especially since the end-result is stripped and minified).

Here is good example syntax:

		// This is a good example!
		.styleguide-format {
			border: 1px solid #0f0;
			color: #000;
			background: rgba(0,0,0,0.5);
		}

## SCSS Style

* Any `$variable` or `@mixin` used in more than one file should be stored in the `/generic` folder. Others can be placed at the top of the file in which they're used.
* As a rule of thumb, don't nest further than three levels deep. If you find yourself going further, think about reorganizing your rules (either the specificity needed, or the layout of the nesting).
* Place any `@include` calls to mixins and nested selectors at the end of parent selector to reduce clutter.

# File Organization

In general, CSS is organized within projects in an atomic-based structure using `/generic`, `/base`, and `/object` folders:

		/sass/
			/base/
				_global-classes.scss
				_headings.scss
				_links.scss
				_text.scss
				...
			/generic/
				_mixins.scss
				_reset.scss
				_variables.scss
			/objects/
				_buttons.scss
				_carousel.scss
				_cover.scss
				_footer.scss
				_header.scss
				_icons.scss
				_layout.scss
				_lists.scss
				_main.scss
				_nav.scss
				_sections.scss
				_text.scss

## Px vs. Ems

Use `ems` for `font-size`, as it can be scaled quickly and efficiently. Additionally, unit-less `line-height` is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the `font-size`.

When possible, use `ems` for `margin`, `padding`, and other proportionally-focused attributes. Use `ems` with media queries as well.

For fixed-width and other font-size-independent attributes, consider using `rems` as they aren't effected by the parent container's `font-size`. __Note: You will need to use a rem polyfill to support older browsers.__

Use `px` for `border` and other decorations (`box-shadow`, `text-shadow`).

## Specificity (classes vs. IDs)

Elements that **occur exactly** once inside a page should use IDs, otherwise, use classes. When in doubt, use a class name.

* **Good** candidates for IDs: header, footer, modal popups.
* **Bad** candidates for IDs: navigation, item listings, item view pages (ex: issue view).

When styling a component, start with an element + class namespace (prefer class names over IDs), prefer direct descendant selectors by default, and use as little specificity as possible. Here is a good example:

		<ul class="category-list">
			<li class="item">Category 1</li>
			<li class="item">Category 2</li>
			<li class="item"><a href="#">Category 3</a></li>
		</ul>

		ul.category-list { // element + class namespace

			> li { // direct descendant selector > for list items
				list-style-type: disc;
			}

			a { // minimal specificity for all links
				color: #ff0000;
			}
		}

### CSS Specificity Guidelines

* If you must use an id selector (`#selector`) make sure that you have no more than _one_ in your rule declaration. A rule like `#header .search #quicksearch { ... }` is considered harmful.
* When modifying an existing element for a specific use, try to use specific class names. Instead of `.listings-layout.bigger`, use rules like `.listings-layout.listings-bigger`. Think about `ack/greping` your code in the future.
* The class names `disabled`, `mousedown`, `danger`, `hover`, `selected`, and `active` should always be namespaced by a class (`button.selected` is a good example).

## Separate Style from Layout

In general, try to separate style from layout. This allows for modular reuse of styles throughout your site while keeping layout independent. Remember: it's easier to add or remove a class from an HTML element than refactor all of your CSS!

		// bad example
		<div class="two-columns-featured">
			<div class="col"></div>
			<div class="col"></div>
		</div>

		.two-columns-featured {
			background: #efefef;
			font-size: 1.2em;

			> .col {
				width: 48%;
				float: left;
				margin: 0 2%;
			}
		}

		// good example
		<div class="two-columns featured">
			<div class="col"></div>
			<div class="col"></div>
		</div>

		.two-columns {
			> .col {
				width: 48%;
				float: left;
				margin: 0 2%;
			}
		}

		.featured {
			background: #efefef;
			font-size: 1.2em;
		}

Remember: End users don't care how your CSS looks, as long as the website looks OK. Think about other _developers_ when you are building and documenting your stylesheets!