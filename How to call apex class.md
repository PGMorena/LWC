# Expose Apex Methods to Lightning Web Components
## To expose an Apex method to a Lightning web component, the method must  be static and either global or public. Annotate the method with @AuraEnabled.
```javascript
public with sharing class ContactController {
    @AuraEnabled(cacheable=true)
    public static List<Contact> getContactList(Integer recordSize) {
        return [SELECT Id, Name, Title, Phone, Email, Picture__c FROM Contact LIMIT :recordSize];
    }
}
```

To call an Apex method, a Lightning web component can:
1. Wire a property
2. Wire a function
3. Call a method imperatively

## Using Wire
```javascript
import { LightningElement, wire, track, api } from 'lwc';
import getContactList from '@salesforce/apex/dsiplaycases.getContactList';

/* eslint-disable no-console */
 /* eslint-disable no-alert */

export default class displaycases extends LightningElement {

@wire(getContactList, { recordSize: 5}) contacts;

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
    getCaseList({recordSize: 5}).then(result=>{
        alert(result);
    });
}

}
```
To use @wire to call an Apex method, you must set cacheable=true.

To call an Apex method imperatively, you can choose to set cacheable=true. This setting caches the result on the client, and prevents Data Manipulation Language (DML) operations.
