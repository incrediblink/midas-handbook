# Wordpress Snippets
## Hide the header and footer on a page
Add the following CSS rules to the page settings.
```css
#footer-outer, #header-space, #header-outer {
    display: none;
}
```

## Anchors link
[Anchors links](https://html.com/anchors-links/) enable users to navigate to a certain part of a webpage instantaneously. One can add an anchor by adding an `id` attribute to the target element. Then, users can jump to the target element by clicking on an `<a>` element that links to `https://{current-url}#{id}`.

However, if one try to add anchors on MIDAS website, they will find out that, although the webpage does scroll to the desired region after clicking on the `<a>` element, the target element with the corresponding `id` attribute is covered by the website header and therefore not visible to the users. To fix this, we need to create a ghost element that is slightly above the target element. Thereby, when the webpage scrolls to the ghost element, the target element below it is visible.

To achieve that, we first need to know how much higher the ghost element needs to be compared to the target element. Create a `Raw JS` block in the editor and write down the following JavaScript code.

```JavaScript
// Calculate the needed offset
var html = document.getElementsByTagName("html")[0];
var header = document.getElementById("header-space");
var offset = html.offsetTop + header.offsetHeight + 16;
```

Here, we first calculate the total height of the website header, which equals the sum of the `<html>` and `<div id="header-space">`’s heights. We then add another 16 pixels to the offset, just for aesthetic purpose. Notice that, for the sake of browser compatibility, we use `var` rather than the fancier `let` and `const` to declare variables.

```JavaScript
// Locate the target element
var title = document.getElementById("core-title");

// Create the ghost element
var ghost = document.createElement("div");
ghost.style.position = "absolute";
ghost.style.top = (-offset) + "px";
ghost.id = "core";

// Insert the ghost element before the target element
// The parent element of the ghost element needs to have a position style of `relative`
title.parentElement.style.position = "relative";
title.parentElement.insertBefore(d, title);
```

_Voila!_ Running the script will create a ghost element that is above the target element. When the users access `https://{current-url}#core`, the webpage will navigate to just the right position. You can try it out and check out the source code [here](https://midas.umich.edu/certificate/approved-courses/#domain).