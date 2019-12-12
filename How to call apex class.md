# Apex Class
```javascript
public with sharing class dsiplaycases {
       @AuraEnabled(cacheable=true)
       public static List<Case> getCaseList(Integer v_Offset, Integer v_pagesize){ 
           return [select id, casenumber, subject from case limit :v_pagesize OFFSET :v_Offset];
       }
}
```
## Using Wire
```javascript
import { LightningElement, wire, track, api } from 'lwc';
import getCaseList from '@salesforce/apex/dsiplaycases.getCaseList';

/* eslint-disable no-console */
 /* eslint-disable no-alert */

export default class displaycases extends LightningElement {

@wire(getCaseList, { v_Offset: 5, v_pagesize: 6 }) cases;

}
```
## Imperatively
```javascript
import { LightningElement, wire, track, api } from 'lwc';
import getCaseList from '@salesforce/apex/dsiplaycases.getCaseList';

/* eslint-disable no-console */
 /* eslint-disable no-alert */

export default class displaycases extends LightningElement {
Handler(){
    getCaseList({v_Offset: 5, v_pagesize: 6}).then(result=>{
        alert(result);
    });
}

}
```
