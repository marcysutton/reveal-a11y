# reveal-a11y
[Reveal.js](https://github.com/hakimel/reveal.js/) plugin for better slide accessibility when shared or distributed online. Include CSS and JS in your slide's index file to hide offscreen slides and improve accessibility for keyboard and screen reader users.

## Installation
1. Clone this repository or download a zip file to copy the files to your machine. 
	
1. Copy `accessibility` directory to the Reveal.js `plugin` directory.
	
1. Include CSS file in the `<head>` of `index.html`:
	
	```html
	<link rel="stylesheet" href="plugin/accessibility/helper.css">
	```
	
1. Include JavaScript file as dependency in `index.html`:
	
	```javascript
	<script>
	// More info https://github.com/hakimel/reveal.js#configuration
	Reveal.initialize({
		dependencies: [
                    { src: 'plugin/accessibility/helper.js', async: true, condition: function() { 
                    	return !!document.body.classList; 
                    } 
                }]
	});
	</script>
	```

## Features

### Hidden offscreen slides

This plugin adds CSS to "really hide" offscreen slides using `display: none;` on an element wrapping each slide. This technique was used to avoid issues with transitions and `display: none`. For this to work, the styles must be loaded in HTML as a `<link>` tag (as opposed to injecting dynamically with JavaScript).

### Dynamically labeled slide sections

HTML `<section>` elements commonly used for slides will act as landmarks in screen readers. To make them easier to identify, this plugin dynamically adds an `aria-label` property with a value of "Slide 1", as an example. For nested slides, it will add "Slide 1, child 1" with numbers relative to that slide.

Purpose: uniquely labeled sections are more useful in assistive technology than `<section>` soup.

Before (your code):
```html
<section>Reveal.js</section>
<section>
	<section>It might be dated</section>
	<section>But it's still useful</section>
</section>
```

After (dynamically rendered code):
```html
<section aria-label="Slide 1">Reveal.js</section>
<section aria-label="Slide 2">
	<section aria-label="Slide 2, child 1">It might be dated</section>
	<section aria-label="Slide 2, child 2">But it's still useful</section>
</section>
```

### Dynamic Skip Links / Table of Contents 

This plugin only supports nesting of sections 2 levels deep. It injects a "Skip to Navigation" link at the beginning of the source order as well as an unordered list of slide links at the end. The content for those links is sourced from each slide: it grabs either the first child text node or the bare `textContent`. Various selectors for injected markup can be manipulated as options.

Default options (modifiable in `plugin/accessibility/helper.js`):

```javascript
new SkipLinks({
	enabled: true,

	global_skip_link_id: 'global-skip-link',

	slide_skip_links_id: 'table-of-contents',

	global_skip_link_text: 'Show navigation',

	skip_link_target_selector: '.accessibilityWrapper',

	controls_selector: '.controls'
})
```
