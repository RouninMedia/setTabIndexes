# setTabIndexes
`setTabIndexes()` represents a different approach to keyboard accessibility for web-apps, enabling dynamic resetting of tab indexes from positive to negative and vice versa for different *zones* (modals, dialogs, dropdown menus etc.) across the app.

Generally, the advice (across the web, dating back to the 2000s) is to use only the following integer values for the HTML attribute `tabindex`:

 - `1` (or another positive integer) | *(focusable and reachable via sequential keyboard navigation)*
 - `0` | *(focusable and reachable **after** any sequential keyboard navigation)*
 - `-1` | *(focusable but **not** reachable via sequential keyboard navigation)*

Notably absent from the list above are negative integers lower than `-1` such as `-2`, `-3`, `-4` etc.

Arguably, this advice is for an era when every UI element was simultaneously visible on the screen - that is, before screen UIs regularly featured dropdown menus, modals, dialogs etc.

The advice above is, essentially, written for the _web of documents_. It could benefit with a (long overdue) reformulation for the _web of apps_.

Why might such a reformulation be necessary? Because, when only the three values above are in use, any *tabbable* elements reachable via sequential keyboard navigation but currently outside the viewport in some way (or in some form hidden from or not visible to the user), will _still need to be tabbed through_ whenever the user cycles through the tabbable elements.

This, it goes without saying, is less than desirable. It leads to a sub-standard user experience. 

It's much more intuitive for the user to be able to cycle through _only_ those tabbable elements they can see the **tab focus** cycling through.

The `JS` + `HTML` technique below enables this more intuitive user experience. It does so by taking advantage of negative alongside positive integer values.

In a nutshell, the negative values represent positive integer values which have been *turned off*.

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
