# Parent to child
To invoke child function from parent, you need to do the below steps:
1. Anonate child function with @api
2. Use this.template.querySelector to access component in parent's javascript function.
sample code below:
```javascript
<--------------------Parent------------------------------------------------>
<template>
        <lightning-card title="Case Records" icon-name="custom:custom63">
                <c-child></c-child>
            </div>
        </lightning-card>
</template>
//put this line where you want to invoke child method
this.template.querySelector('c-child').changeView('falseprevious');

<---------------------Child-------------------------------------------------->
import { LightningElement, api} from 'lwc';
/* eslint-disable no-console */
 /* eslint-disable no-alert */
export default class Child extends LightningElement {
    @api
    changeView(str){  
      //this method is being called from parent component
    }   
}
```
<-----------------------OR ------------------------------------------------------->
1. Anotate any variable with  @api anotation
2. Pass the value while calling child in parent
sample code below:
```javascript
<--------------------Parent----------------------------------------------------->
<template>
        <lightning-card title="Case Records" icon-name="custom:custom63">
                <c-child changeView="pass this value to child"></c-child>
            </div>
        </lightning-card>
</template>

<---------------------Child----------------------------------------------->
import { LightningElement, api} from 'lwc';

export default class Child extends LightningElement {
    @api changeView;
}
```
# Child to Parent
1. Create a new event in child and dispatch
```javascript
<---------------------Child----------------------------------------------->
import { LightningElement, api, track } from 'lwc';

export default class Paginator extends LightningElement {

    changeHandler(event){
        event.preventDefault();
        const s_value = event.target.value;
        const selectedEvent = new CustomEvent('selected', { detail: s_value});

        this.dispatchEvent(selectedEvent);
 
    }
}
```
2. Handle in parent 1(Declaratively)
        --> if event name is 'selected' then while handling in parent use 'onselected'
```javascript
<--------------------Parent----------------------------------------------------->
<template>
        <lightning-card title="Case Records" icon-name="custom:custom63">
                <c-paginator onselected={changeHandler2}></c-paginator>
            </div>
        </lightning-card>
</template>

import { LightningElement, wire, track, api } from 'lwc';
export default class displaycases extends LightningElement {
@track page_size = 10;

changeHandler2(event){
    const det = event.detail;
    this.page_size = det;
}
}
```
3. Handle in parent 2(Attach an Event Listener Programmatically)
```javascript
<--------------------Parent----------------------------------------------------->
<template>
        <lightning-card title="Case Records" icon-name="custom:custom63">
                <c-paginator></c-paginator>
            </div>
        </lightning-card>
</template>

import { LightningElement, wire, track, api } from 'lwc';
export default class displaycases extends LightningElement {
@track page_size = 10;
constructor() {
    super();
    this.template.addEventListener('selected', this.changeHandler2.bind(this));
  }
changeHandler2(event){
    const det = event.detail;
    this.page_size = det;
}
}
```

