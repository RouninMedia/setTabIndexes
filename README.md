# setTabIndexes
`setTabIndexes()` represents a different approach to keyboard accessibility for web-apps, enabling dynamic resetting of tab indexes from positive to negative and vice versa.

Generally, the advice (across the web) is to use the following integer values for the HTML attribute `tabindex`:
 - `1` (or another positive integer) | *(focusable and reachable via sequential keyboard navigation)*
 - `0` | *(focusable and reachable **after** any sequential keyboard navigation)*
 - `-1` | *(focusable but **not** reachable via sequential keyboard navigation)*

Notably absent from the list above are negative integers lower than `-1` such as `-2`, `-3`, `-4` etc.

Arguably, the advice is for an era when every UI element was simultaneously visible on the screen and before screen UIs regularly featured dropdown menus, modals, dialogs etc.

Why? Because if we only ever use `1` (or another positive integer), `0` and `-1` as our three possible `tabindex` attribute values, then any *tabbable* element which is currently outside the viewport in some way (or in some form hidden from or not visible to the user), will _still need to be tabbed through_ while cycling through the tabbable elements.

This, it goes without saying, is less than desirable. It's much more intuitive for the user to be able to cycle through _only_ the tabbable elements they can see the tab focus cycling through.

To enable this kind of more intuitive user experience, this technique makes use of negative integer values, which corresponding to all the positive integer values in use. They represent those same positive integer values, but *turned off*.

______

## The `setTabIndexes() Function`

```js

const setTabIndexes = (selectors) => {

  selectors = (selectors.constructor.name === 'Array') ? selectors : [selectors];

  document.querySelectorAll('[tabindex]').forEach((tabFocusElement) => {

    let tabbingEnabled = false;

    let currentTabIndex = parseInt(tabFocusElement.getAttribute('tabindex'));

    if ((currentTabIndex > 1) || (currentTabIndex < -1)) {

      selectors.forEach((selector) => {

        if (tabFocusElement.closest(selector) !== null) {
        
          // data-tabindex-sign="-" INDICATES THAT tabbingEnabled WILL BE SET TO false ON AN ELEMENT WHERE IT WOULD NORMALLY BE SET TO true
          tabbingEnabled = (('tabindexSign' in tabFocusElement.dataset) && (tabFocusElement.dataset.tabindexSign === '-')) ? false : true;
        }

      });

      if (tabbingEnabled === true) {

        tabFocusElement.setAttribute('tabindex', Math.abs(currentTabIndex));
      }

      else {

        tabFocusElement.setAttribute('tabindex', (0 - Math.abs(currentTabIndex)));
      }
    }

    else {

      console.log(tabFocusElement);
    }
  });
}

```

_______

## Example HTML

```html
  <div class="themeSettings panel --exitRight" tabindex="-1">
    <h2 class="heading --settings">Pick Theme</h2>
    
    <button type="button" class="theme none" title="Remove Theme" tabindex="-8"></button>

    <button type="button" class="theme red" title="Red Theme" tabindex="-8"></button>
    <button type="button" class="theme orange" title="Orange Theme" tabindex="-8"></button>
    <button type="button" class="theme sand" title="Peach Theme" tabindex="-8"></button>

    <button type="button" class="theme purple" title="Purple Theme" tabindex="-8"></button>
    <button type="button" class="theme magenta" title="Magenta Theme" tabindex="-8"></button>
    <button type="button" class="theme pink" title="Pink Theme" tabindex="-8"></button>
    <button type="button" class="theme lightberry" title="Light Berry Theme" tabindex="-8"></button>

    <button type="button" class="theme lightblue" title="Light Blue Theme" tabindex="-8"></button>
    <button type="button" class="theme blue" title="Blue Theme" tabindex="-8"></button>
    <button type="button" class="theme lime" title="Lime Theme" tabindex="-8"></button>
    <button type="button" class="theme sage" title="Sage Theme" tabindex="-8"></button>

    <button type="button" class="theme bluegray" title="Blue Gray Theme" tabindex="-8"></button>
    <button type="button" class="theme taupe" title="Taupe Theme" tabindex="-8"></button>
    <button type="button" class="theme green" title="Green Theme" tabindex="-8"></button>
    <button type="button" class="theme teal" title="Teal Theme" tabindex="-8"></button>

    <button type="button" class="settingsIcon --changePanel --back" title="Back to Previous Panel" tabindex="-9"></button>
    <button type="button" class="settingsIcon --changePanel --forward" title="Forward to Next Panel" tabindex="-9"></button>
    <button type="button" class="settingsIcon --close" title="Close Settings" tabindex="-9"></button>

  </div>
```
______

## How `setTabIndexes()` works

We can run the setTabIndexes() function on the `HTML` above, via:

    setTabIndexes()    




Possible `tabindex` values:

 - a value of `1` never changes: the element is focusable and *always* reachable via sequential keyboard navigation
 - a value of `-1` never changes: the element is focusable but *never* reachable via sequential keyboard navigation
 - `2` (and higher): the element is focusable and *currently* reachable via keyboard navigation (but may be turned off)
 - `-2` (and lower): the element is focusable and *currently* unreachable via keyboard navigation (but may be turned on)

```html
  <div class="themeSettings panel --exitRight" tabindex="-1">
    <h2 class="heading --settings">Pick Theme</h2>
    
    <button type="button" class="theme none" title="Remove Theme" tabindex="-8"></button>

    <button type="button" class="theme red" title="Red Theme" tabindex="-8"></button>
    <button type="button" class="theme orange" title="Orange Theme" tabindex="-8"></button>
    <button type="button" class="theme sand" title="Peach Theme" tabindex="-8"></button>

    <button type="button" class="theme purple" title="Purple Theme" tabindex="-8"></button>
    <button type="button" class="theme magenta" title="Magenta Theme" tabindex="-8"></button>
    <button type="button" class="theme pink" title="Pink Theme" tabindex="-8"></button>
    <button type="button" class="theme lightberry" title="Light Berry Theme" tabindex="-8"></button>

    <button type="button" class="theme lightblue" title="Light Blue Theme" tabindex="-8"></button>
    <button type="button" class="theme blue" title="Blue Theme" tabindex="-8"></button>
    <button type="button" class="theme lime" title="Lime Theme" tabindex="-8"></button>
    <button type="button" class="theme sage" title="Sage Theme" tabindex="-8"></button>

    <button type="button" class="theme bluegray" title="Blue Gray Theme" tabindex="-8"></button>
    <button type="button" class="theme taupe" title="Taupe Theme" tabindex="-8"></button>
    <button type="button" class="theme green" title="Green Theme" tabindex="-8"></button>
    <button type="button" class="theme teal" title="Teal Theme" tabindex="-8"></button>

    <button type="button" class="settingsIcon --changePanel --back" title="Back to Previous Panel" tabindex="-9"></button>
    <button type="button" class="settingsIcon --changePanel --forward" title="Forward to Next Panel" tabindex="-9"></button>
    <button type="button" class="settingsIcon --close" title="Close Settings" tabindex="-9"></button>

  </div>
```

## Example HTML

```html
<div class="codeboxHeader --input" data-display="maxified">
  <button type="button" class="maxified switch" tabindex="-3" data-tabindex-sign="-">Maxified</button>
  <button type="button" class="minified switch" tabindex="3" data-tabindex-sign="+">Minified</button>
</div>
```

## JavaScript (for updating the `data-tabindex-sign` attribute)
```js
let displayHeaderSwitches = [... document.querySelectorAll('.codeboxHeader .switch')];

for (codeSwitch of displayHeaderSwitches) {
  codeSwitch.dataset.tabindexSign = (codeSwitch.dataset.tabindexSign === '-') ? '+' : '-';
}
```
