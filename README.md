# setTabIndexes
A different approach to keyboard accessibility for web-apps, involving the setting and resetting of tab indexes.

Generally, the advice on the web is to use the following values for the HTML attribute `tabindex`:
 - `1` (or another positive integer)
 - `0`
 - `-1`

This technique makes use of negative integers.

## HTML

```html
<button type="button" class="maxified switch" tabindex="-3" data-tabindex-sign="-">Maxified</button>
<button type="button" class="minified switch" tabindex="3" data-tabindex-sign="+">Minified</button>
```

## JavaScript
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
