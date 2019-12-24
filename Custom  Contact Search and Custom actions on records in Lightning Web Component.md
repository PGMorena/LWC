# Apex Class
```Javascript
public with sharing class ContactSearch {
  
     @AuraEnabled(cacheable=true)
    public static List<Contact> getContactList(String searchInput) {
        if(searchInput == ''){
            return [SELECT Id, FirstName, LastName, Title, Phone, Email FROM Contact];
        }else{
            System.debug('searchInput'+searchInput);
            String query  = 'SELECT Id, FirstName, LastName, Title, Phone, Email FROM Contact where FirstName LIKE \''+String.escapeSingleQuotes(searchInput)+'%\' OR LastName LIKE \''+String.escapeSingleQuotes(searchInput)+'%\' LIMIT 10';
            List<Contact> conList = Database.query(query);
            System.debug('conList'+conList);
            return conList;
        }
        
    }
}
```

# contactSearch Lightning Web Component
```javascript
<template>
    <lightning-card title="Datatable Example" icon-name="custom:custom63">
        <div class="slds-m-around_medium">
            <lightning-input type="search" name="input1" onchange={handleInputchange} label="" placeholder="Search by First Name or Last Name" ></lightning-input>
        </div>       
        <div class="slds-m-around_medium">
            <template if:true={contact.data}>
                <lightning-datatable
                    key-field="Id"
                    data={contact.data}
                    columns={columns}
                    onsave={handleSave}
                    onrowaction={handleRowAction}
                    draft-values={draftValues}>
                </lightning-datatable>
            </template>
            <template if:true={contact.error}>
                <!-- handle Apex error -->
            </template>
        </div>
    </lightning-card>
    <template if:true={isOpenModal}>
            <section role="dialog" tabindex="-1" aria-labelledby="modal-heading-01" aria-modal="true" aria-describedby="modal-content-id-1" class="slds-modal slds-fade-in-open">
                    <div class="slds-modal__container">
                      <header class="slds-modal__header">
                        <button class="slds-button slds-button_icon slds-modal__close slds-button_icon-inverse" title="Close">
                          <svg class="slds-button__icon slds-button__icon_large" aria-hidden="true">
                            <use xlink:href="/assets/icons/utility-sprite/svg/symbols.svg#close"></use>
                          </svg>
                          <span class="slds-assistive-text">Close</span>
                        </button>
                        <h2 id="modal-heading-01" class="slds-modal__title slds-hyphenate">Update Contact</h2>
                      </header>
                      <div class="slds-modal__content slds-p-around_medium" id="modal-content-id-1">
                            <c-contact-Record-Edit-Form recordid1={recordId} oncontactupdated={handleContactUpdate} ></c-contact-Record-Edit-Form> 
                      </div>
                      <footer class="slds-modal__footer">
                        <button class="slds-button slds-button_neutral" onclick={handleCloseModal}>Cancel</button>
                      </footer>
                    </div>
                  </section>
                  <div class="slds-backdrop slds-backdrop_open"></div>
    </template>
</template>

import { LightningElement, wire, track, api } from 'lwc';
import getContactList from '@salesforce/apex/ContactSearch.getContactList';
import { updateRecord } from 'lightning/uiRecordApi';
import { deleteRecord } from 'lightning/uiRecordApi';
import { NavigationMixin } from 'lightning/navigation';
import CONTACT_OBJECT from '@salesforce/schema/Contact';
import { refreshApex } from '@salesforce/apex';
import { ShowToastEvent } from 'lightning/platformShowToastEvent';
import FIRSTNAME_FIELD from '@salesforce/schema/Contact.FirstName';
import LASTNAME_FIELD from '@salesforce/schema/Contact.LastName';
import ID_FIELD from '@salesforce/schema/Contact.Id';

const actions = [
    { label: 'Show details', name: 'show_details' },
    { label: 'Delete', name: 'delete' },
    { label: 'Edit', name: 'edit' }
];
const COLS = [
    { label: 'First Name', fieldName: 'FirstName', editable: true },
    { label: 'Last Name', fieldName: 'LastName', editable: true },
    { label: 'Title', fieldName: 'Title' },
    { label: 'Phone', fieldName: 'Phone', type: 'phone' },
    { label: 'Email', fieldName: 'Email', type: 'email' },
    { type: 'action', typeAttributes: { rowActions: actions } }
];

export default class ContactSearch extends NavigationMixin(LightningElement) {

    @track error;
    @track columns = COLS;
    @track draftValues = [];
    @track inputValue = '';
    @track isOpenModal = false;
    @track recordId;

    @wire(getContactList, {searchInput: '$inputValue'})
    contact;

    handleSave(event) {

        const fields = {};
        fields[ID_FIELD.fieldApiName] = event.detail.draftValues[0].Id;
        fields[FIRSTNAME_FIELD.fieldApiName] = event.detail.draftValues[0].FirstName;
        fields[LASTNAME_FIELD.fieldApiName] = event.detail.draftValues[0].LastName;

        const recordInput = {fields};

        updateRecord(recordInput)
        .then(() => {
            this.dispatchEvent(
                new ShowToastEvent({
                    title: 'Success',
                    message: 'Contact updated',
                    variant: 'success'
                })
            );
            // Clear all draft values
            this.draftValues = [];

            // Display fresh data in the datatable
            return refreshApex(this.contact);
        }).catch(error => {
            this.dispatchEvent(
                new ShowToastEvent({
                    title: 'Error creating record',
                    message: error.body.message,
                    variant: 'error'
                })
            );
        });
    }
    handleInputchange(event){
        const val = event.target.value;
       this.inputValue = val;
    }

    handleRowAction(event){
        const actionName = event.detail.action.name;
        const row = event.detail.row;
       
        if(actionName === 'show_details'){
            this[NavigationMixin.Navigate]({
                type: 'standard__recordPage',
                attributes: {
                    recordId: row.Id,
                    objectApiName: CONTACT_OBJECT.objectApiName,
                    actionName: 'view'
                }
            });
        }
        if(actionName === 'delete'){
            deleteRecord(row.Id)
            .then(() => {
            this.dispatchEvent(
                new ShowToastEvent({
                    title: 'Success',
                    message: 'Contact deleted',
                    variant: 'success'
                })
            );
            return refreshApex(this.contact);
        }).catch(error => {
            this.dispatchEvent(
                new ShowToastEvent({
                    title: 'Error deleting record',
                    message: error.body.message,
                    variant: 'error'
                })
            );
        });
        }
        if(actionName === 'edit'){
         this.isOpenModal = true;
         this.recordId = row.Id;
      
        }
       
        
    }
    handleCloseModal() {
        this.isOpenModal = false;
    }
    handleContactUpdate(){
        this.isOpenModal = false;
        return refreshApex(this.contact);
    }
}

```
contactRecordEditForm Lightning Web Component
```javascript
<template>
    <lightning-record-edit-form record-id={recordid1}
                                object-api-name="Contact">
        <lightning-messages>
        </lightning-messages>
        <lightning-input-field field-name="FirstName" data-field="FirstName">
        </lightning-input-field>
        <lightning-input-field field-name="LastName" data-field="LastName">
        </lightning-input-field>
        <lightning-input-field field-name="Title" >
        </lightning-input-field>
        <lightning-button
            class="slds-m-top_small"
            variant="brand"
            onclick={handleClick}
            name="Submit"
            label="Submit"
            >
        </lightning-button>
    </lightning-record-edit-form>
</template>

/* eslint-disable no-alert */
import { LightningElement, api } from 'lwc';
import { updateRecord } from 'lightning/uiRecordApi';
import FIRSTNAME_FIELD from '@salesforce/schema/Contact.FirstName';
import LASTNAME_FIELD from '@salesforce/schema/Contact.LastName';
import ID_FIELD from '@salesforce/schema/Contact.Id';
import { ShowToastEvent } from 'lightning/platformShowToastEvent';

export default class ContactRecordEditForm extends LightningElement {
    @api recordid1;
    handleClick(event){
        event.preventDefault();
        const fields = {};
        fields[ID_FIELD.fieldApiName] = this.recordid1;
        fields[FIRSTNAME_FIELD.fieldApiName] = this.template.querySelector("[data-field='FirstName']").value;
        fields[LASTNAME_FIELD.fieldApiName] = this.template.querySelector("[data-field='LastName']").value;
        const recordInput = {fields};

        updateRecord(recordInput)
        .then(() => {
            this.dispatchEvent(new CustomEvent('contactupdated'));

            this.dispatchEvent(
                new ShowToastEvent({
                    title: 'Success',
                    message: 'Contact updated',
                    variant: 'success'
                })
            )
        })
        .catch(error => {
            this.dispatchEvent(
                new ShowToastEvent({
                    title: 'Error creating record',
                    message: error.body.message,
                    variant: 'error'
                })
            );
        }); 

    }

}
```
