# setTabIndexes
A different approach to keyboard accessibility for web-apps, involving the setting and resetting of tab indexes.

Generally, the advice (across the web) is to use the following integer values for the HTML attribute `tabindex`:
 - `1` (or another positive integer)
 - `0`
 - `-1`

This technique makes use of negative integers.

## How `setTabIndexes()` works

``html
  <div class="themeSettings panel --exitRight" tabindex="-1">
    <h2 class="heading --settings">Pick Theme</h2>
    
    <button type="button" class="theme none" title="Remove Theme" tabindex="-8">

      <svg xmlns="http://www.w3.org/2000/svg"
           lang="en-GB"
           class="defaultSettingIcon"
           viewBox="0 0 150 150">
      
        <title>Remove Theme</title>
      
        <defs>
      
          <style><![CDATA[
      
            .defaultSettingIcon path {
              fill: rgb(0, 0, 0);
              opacity: 0.2;
              transform: translateX(2.3px);
            }
      
            :root[data-mode="dark"] .defaultSettingIcon path {
              fill: rgb(255, 255, 255);
            }
      
            .theme.none:hover .defaultSettingIcon path,
            .theme.none:focus .defaultSettingIcon path {
              opacity: 0.9;
              fill: rgb(255, 255, 255);
            }
      
          ]]></style>
      
        </defs>
      
        <path d="M 60.562999,131.52779 C 44.722553,128.06323 28.650716,115.48424 21.499278,100.95367 11.017851,79.657108 13.725485,56.278255 28.766604,38.204516 l 4.957849,-5.957459 -2.940248,-2.816936 c -1.617136,-1.549315 -2.599781,-3.15136 -2.183655,-3.560101 0.416126,-0.40874 4.581593,-0.842109 9.256593,-0.963042 l 8.5,-0.219879 0.284983,10.156451 C 46.798867,40.429597 46.680023,45 46.37803,45 46.076036,45 44.484697,43.5375 42.84172,41.75 L 39.854489,38.5 35.925793,43 c -19.44101,22.268083 -14.742678,56.364481 10.047915,72.919 8.459719,5.64919 16.928889,8.07737 28.23877,8.09631 5.029566,0.008 9.743199,0.39475 10.474741,0.85851 2.285018,1.44857 1.020149,6.45044 -1.830076,7.23695 -4.007498,1.10586 -16.007213,0.79207 -22.294144,-0.58298 z m 39.155654,-7.99962 c -0.55381,-0.5538 0.024,-18.74699 0.614097,-19.33712 0.2206,-0.22059 1.80305,1.19567 3.51656,3.14725 l 3.11547,3.54832 3.2648,-3.69331 C 118.22996,98.142888 121.85714,88.100286 121.85714,75 c 0,-13.935166 -3.77884,-23.707539 -12.90269,-33.367377 C 98.808718,30.890877 87.006231,26 71.230369,26 c -9.109985,0 -9.453682,-0.08248 -10.572011,-2.53694 -1.801598,-3.954077 0.541827,-5.497385 9.300391,-6.124953 22.316218,-1.599001 44.776331,11.532647 54.762991,32.018014 2.18359,4.479134 4.45734,10.528528 5.05278,13.4431 1.38512,6.779879 1.38512,17.621679 0,24.401558 -1.47938,7.241292 -8.06469,20.393371 -12.79497,25.553921 l -3.89995,4.2547 2.81522,2.36885 c 4.09324,3.44424 2.16432,4.62175 -7.57101,4.62175 -4.47334,0 -8.345657,-0.21232 -8.605157,-0.47183 z M 71.842087,113.75 c -0.0083,-0.1375 -0.623366,-1.99742 -1.366855,-4.13315 -1.06318,-3.05406 -2.117232,-4.14629 -4.936861,-5.11563 -3.297958,-1.13378 -3.761749,-1.04441 -5.791313,1.11596 -4.219384,4.49133 -5.558174,4.15838 -13.008866,-3.23522 -3.873626,-3.843943 -6.881049,-7.690245 -6.881049,-8.800422 0,-1.084655 1.113526,-3.134371 2.474501,-4.554923 2.264625,-2.363759 2.37743,-2.899111 1.329999,-6.311951 -0.962858,-3.13728 -1.760532,-3.895009 -5.025998,-4.774307 -2.134822,-0.574848 -4.145821,-1.734001 -4.468886,-2.575895 -0.784437,-2.04421 3.363829,-17.346141 5.274917,-19.45787 1.259096,-1.391285 2.179002,-1.500126 5.51726,-0.652786 3.676261,0.933135 4.236477,0.796293 6.80181,-1.661452 2.622201,-2.512228 2.728398,-2.930055 1.712293,-6.736901 -0.971807,-3.640882 -0.867671,-4.233159 1.011884,-5.755131 1.152347,-0.933114 5.555341,-2.558212 9.784431,-3.61133 8.886471,-2.212887 10.607613,-1.68266 11.972468,3.688326 0.742167,2.920582 1.501697,3.646086 4.714985,4.503761 3.432226,0.916114 4.05824,0.779742 6.023488,-1.312168 4.205135,-4.476161 5.549581,-4.140031 12.995798,3.249128 3.873627,3.843944 6.881047,7.690246 6.881047,8.800423 0,1.084655 -1.15584,3.178535 -2.56853,4.653067 -2.13355,2.226951 -2.45761,3.272222 -1.91354,6.172334 0.5563,2.96537 1.22671,3.662661 4.4499,4.628352 5.9121,1.771309 6.20676,2.648796 3.90567,11.631217 -2.97752,11.622938 -3.16437,11.859135 -8.8492,11.185852 -4.261556,-0.504719 -5.046855,-0.275177 -7.376093,2.156025 -2.384551,2.488934 -2.516197,3.04891 -1.544288,6.568871 0.827427,2.99669 0.767535,4.13132 -0.270717,5.12869 -2.398871,2.3044 -20.747078,6.88711 -20.848255,5.20713 z M 79.857143,83.109047 C 85.81628,78.816804 84.366702,68.364334 77.409145,65.457281 67.079326,61.141207 58.179269,73.287212 65.391277,81.858197 c 3.768546,4.478663 9.318287,4.958543 14.465866,1.25085 z" />
      
      </svg>

    </button>

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

    <button type="button" class="settingsIcon --changePanel --back" title="Back to Previous Panel" tabindex="-9">
      <svg class="changePanelIcon" viewBox="0 0 768 768"><title>Back to Previous Panel</title><use href="#changePanelIconPath" /></svg>
    </button>
    
    <button type="button" class="settingsIcon --changePanel --forward" title="Forward to Next Panel" tabindex="-9">
      <svg class="changePanelIcon" viewBox="0 0 768 768"><title>Forward to Next Panel</title><use href="#changePanelIconPath" /></svg>
    </button>
    
    <button type="button" class="settingsIcon --close" title="Close Settings" tabindex="-9">
      <svg class="closeIcon" viewBox="0 0 410 410"><title>Close Settings</title><use href="#closeIconGroup" /></svg>
    </button>

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
