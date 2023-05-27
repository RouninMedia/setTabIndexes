# setTabIndexes
A different approach to keyboard accessibility for web-apps, involving the setting and resetting of tab indexes.

Generally, the advice (across the web) is to use the following integer values for the HTML attribute `tabindex`:
 - `1` (or another positive integer)
 - `0`
 - `-1`

This technique makes use of negative integers.

## How `setTabIndexes()` works

Possible `tabindex` values:

 - `1` never changes: the element is focusable and *always* reachable via sequential keyboard navigation
 - `-1` never changes: the element is focusable but *never* reachable via sequential keyboard navigation
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


## `setTabIndexes() Function`

```js

const setTabIndexes = (selectors) => {

  selectors = (selectors.constructor.name === 'Array') ? selectors : [selectors];

  document.querySelectorAll('[tabindex]').forEach((tabFocusElement) => {

    let tabbingEnabled = false;

    let currentTabIndex = parseInt(tabFocusElement.getAttribute('tabindex'));

    if ((currentTabIndex > 1) || (currentTabIndex < -1)) {

      selectors.forEach((selector) => {

        if (tabFocusElement.closest(selector) !== null) {
        
          // data-tabindex-sign DETERMINES WHETHER tabbingEnabled WILL BE true OR false
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
