# Parent to child
To invoke child function from parent you need to do the below steps:
1. Anonate child function with @api
2. Use this.template.querySelector to access component in parent's javascript function.
```javascript
<--------------------Parent---------------------------------------------->
<template>
        <lightning-card title="Case Records" icon-name="custom:custom63">
                <c-child></c-child>
            </div>
        </lightning-card>
</template>
//put this line where you want to invoke child method
this.template.querySelector('c-child').changeView('falseprevious');

<---------------------child----------------------------------------------->
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
