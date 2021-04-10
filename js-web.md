# js-web

## custom html tags

[source](https://javascript.info/custom-elements)

**as new**
```
class MyElement extends HTMLElement {
  constructor() { super(); /* ... */ }
  connectedCallback() { /* ... */ }
  disconnectedCallback() { /* ... */  }
  static get observedAttributes() { return [/* ... */]; }
  attributeChangedCallback(name, oldValue, newValue) { /* ... */ }
  adoptedCallback() { /* ... */ }
 }
customElements.define('my-element', MyElement);
/* <my-element> */
```

**as extension**
```
class MyButton extends HTMLButtonElement { /*...*/ }
customElements.define('my-button', MyElement, {extends: 'button'});
/* <button is="my-button"> */
```

**as new with comments**
```
class MyElement extends HTMLElement {
  constructor() {
    super();
    // element created
  }

  connectedCallback() {
    // browser calls this method when the element is added to the document
    // (can be called many times if an element is repeatedly added/removed)
  }

  disconnectedCallback() {
    // browser calls this method when the element is removed from the document
    // (can be called many times if an element is repeatedly added/removed)
  }

  static get observedAttributes() {
    return [/* array of attribute names to monitor for changes */];
  }

  attributeChangedCallback(name, oldValue, newValue) {
    // called when one of attributes listed above is modified
  }

  adoptedCallback() {
    // called when the element is moved to a new document
    // (happens in document.adoptNode, very rarely used)
  }

  // there can be other element methods and properties
}
```

**full example**
```
<script>
class TimeFormatted extends HTMLElement {

  render() { // (1)
    let date = new Date(this.getAttribute('datetime') || Date.now());

    this.innerHTML = new Intl.DateTimeFormat("default", {
      year: this.getAttribute('year') || undefined,
      month: this.getAttribute('month') || undefined,
      day: this.getAttribute('day') || undefined,
      hour: this.getAttribute('hour') || undefined,
      minute: this.getAttribute('minute') || undefined,
      second: this.getAttribute('second') || undefined,
      timeZoneName: this.getAttribute('time-zone-name') || undefined,
    }).format(date);
  }

  connectedCallback() { // (2)
    if (!this.rendered) {
      this.render();
      this.rendered = true;
    }
  }

  static get observedAttributes() { // (3)
    return ['datetime', 'year', 'month', 'day', 'hour', 'minute', 'second', 'time-zone-name'];
  }

  attributeChangedCallback(name, oldValue, newValue) { // (4)
    this.render();
  }

}

customElements.define("time-formatted", TimeFormatted);
</script>

<time-formatted id="elem" hour="numeric" minute="numeric" second="numeric"></time-formatted>

<script>
setInterval(() => elem.setAttribute('datetime', new Date()), 1000); // (5)
</script>
```


## create html elements (w/shadow dom)

[sourced from](https://code-boxx.com/create-custom-html-elements/)

**example with child element**
```
class Vertical extends HTMLElement {
  // (A) CONSTRUCTOR
  constructor() {
    super(); // Necessary for HTML elements...
 
    // (A1) SHADOW DOM ELEMENTS
    this._title = document.createElement('h1');
    this._text = document.createElement('p');
 
    // (A2) CSS STYLES
    // ! Note: shadow DOM cannot be styled from external CSS !
    // ! Either inline the styles here, or use @import !
    this._styles = document.createElement('style');
    this._styles.textContent = `
    h1 {
      color: red;
      font-size: 2em;
    }
    p {
      font-weight: bold;
      writing-mode: vertical-rl;
      text-orientation: mixed;
    }`;
 
    // (A3) ATTACH STYLES & ELEMENTS INTO SHADOW DOM
    this._shadow = this.attachShadow({mode: 'open'});
    this._shadow.appendChild(this._styles);
    this._shadow.appendChild(this._title);
    this._shadow.appendChild(this._text);
  }
 
  // (B) LISTEN FOR ATTRIBUTES CHANGE
  static get observedAttributes() { return ['title', 'text']; }
  attributeChangedCallback(name, oldValue, newValue) {
    // (B1) SET TITLE ATTRIBUTE TO SHADOW DOM _TITLE <H1> 
    if (name=="title") { this._title.innerHTML = newValue; }
    // (B2) SET TEXT ATTRIBUTE TO SHADOW DOM _TEXT <P> 
    if (name=="text") { this._text.innerHTML = newValue; }
  }
 
  // (C) OPTIONAL CALLBACKS
  // (C1) WHEN THIS CUSTOM ELEMENT IS INSERTED INTO DOM
  connectedCallback() { console.log("Connected"); }
  // (C2) WHEN THIS CUSTOM ELEMENT IS REMOVED FROM DOM
  disconnectedCallback() { console.log("Disconnected"); }
  // (C3) WHEN THIS CUSTOM ELEMENT IS ADOPTED
  adoptedCallback() { console.log("Adopted"); }
}
 
// (D) REGISTER OUR CUSTOM HTML ELEMENT 
// This will register to "<vertical-text>"
// MUST have dash for custom elements, verticaltext will fail and not work
customElements.define('vertical-text', Vertical);
```

**example using template and slots**
```
<!-- (A) VERTICAL TEXT BOX TEMPLATE -->
<template id="vertemp">
  <style>
  h1 {
    color: red;
    font-size: 2em;
  }
  p {
    font-weight: bold;
    writing-mode: vertical-rl;
    text-orientation: mixed;
  }
  </style>
  <h1><slot name="vert-title"></slot></h1>
  <p><slot name="vert-text"></slot></p>
</template>

<!-- (B) JAVASCRIPT -->
<script>
// (B1) ADAPT TEMPLATE INTO SHADOW DOM
class Vertical extends HTMLElement {
  constructor() {
    super();
    this._shadow = this.attachShadow({mode: 'open'});
    this._shadow.appendChild(document.getElementById("vertemp").content);
  }
}
// (B2) REGISTER CUSTOM ELEMENT
customElements.define('vertical-text', Vertical);
</script>

<!-- (C) CUSTOM VERTICAL TEXT BOX -->
<vertical-text>
  <span slot="vert-text">FOO BAR</span>
  <span slot="vert-title">THE TITLE</span>
</vertical-text>
```
