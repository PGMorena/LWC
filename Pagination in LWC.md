Pagination is used for displaying large number of records in chunks. Using pagination we can control the number of records displayed on each page. 


![Image of Pagination in LWC](https://github.com/Rakesh1Singh/LWC/blob/master/Display%20Case%20Pagination.JPG?raw=true)

# Apex Class
```javascript
public with sharing class dsiplaycases {
       @AuraEnabled
       public static Integer TotalRecords(){
           return [Select count() from Case];
       }
       @AuraEnabled(cacheable=true)
       public static List<Case> getCaseList(Integer v_Offset, Integer v_pagesize){ 
           return [select id, casenumber, subject from case limit :v_pagesize OFFSET :v_Offset];
       }

       @AuraEnabled(cacheable=true)
       public static Integer getNext(Integer v_Offset, Integer v_pagesize){
           v_Offset += v_pagesize;
           return v_Offset;
       }

       @AuraEnabled(cacheable=true)
       public static Integer getPrevious(Integer v_Offset, Integer v_pagesize){
           v_Offset -= v_pagesize;
           return v_Offset;
       }
}
```
## Explanation:
1. TotalRecords() -> This method returns total number of case records in an org. This is used to disabled the next button when page        reached to the last page.
2. getCaseList -> This method return list of cases as per the page size and offset.
3. getNext/getPrevious -> These method sets the current offset value.

## Paginator.html
```javascript
<template>
    <lightning-layout>
        <lightning-layout-item>
            <lightning-button label="Previous" icon-name="utility:chevronleft" onclick={previousHandler1}></lightning-button>
        </lightning-layout-item>
        <lightning-layout-item>
                <lightning-button label="First Page" icon-name="utility:chevronleft" onclick={FirstPageHandler1}></lightning-button>
        </lightning-layout-item>
        <lightning-layout-item>
                <select class="slds-select slds-text-color_success" name = "optionSelect" onchange={changeHandler} >
                        <option value="">Page size</option>
                        <option value="5">5</option>
                        <option value="10">10</option>
                        <option value="15">15</option>
                        <option value="20">20</option>
                    </select> 
        </lightning-layout-item>    
        <lightning-layout-item flexibility="grow"></lightning-layout-item>
        <lightning-layout-item>
                <lightning-button label="Last Page" icon-name="utility:chevronright" icon-position="right" onclick={LastPageHandler1}></lightning-button>
            </lightning-layout-item>
        <lightning-layout-item>
            <lightning-button label="Next" icon-name="utility:chevronright" icon-position="right" onclick={nextHandler1}></lightning-button>
        </lightning-layout-item>
    </lightning-layout>
</template>
```

## Paginator.js
```javascript
import { LightningElement, api, track } from 'lwc';
/* eslint-disable no-console */
 /* eslint-disable no-alert */
export default class Paginator extends LightningElement {
     @api
    changeView(str){
        if(str === 'trueprevious'){
            this.template.querySelector('lightning-button.Previous').disabled = true;
        }
        if(str === 'falsenext'){
            this.template.querySelector('lightning-button.Next').disabled = false;
        }
        if(str === 'truenext'){
            this.template.querySelector('lightning-button.Next').disabled = true;
        }
        if(str === 'falseprevious'){
            this.template.querySelector('lightning-button.Previous').disabled = false;
        }
    }
    renderedCallback(){
          this.template.querySelector('lightning-button.Previous').disabled = true;
    }
    previousHandler1() {
        this.dispatchEvent(new CustomEvent('previous'));
    }

    nextHandler1() {
        this.dispatchEvent(new CustomEvent('next'));
    }
    FirstPageHandler1(){
        this.dispatchEvent(new CustomEvent('firstpage'));
    }
    LastPageHandler1(){
        this.dispatchEvent(new CustomEvent('lastpage'));
    }
    changeHandler(event){
        event.preventDefault();
        const s_value = event.target.value;
        const selectedEvent = new CustomEvent('selected', { detail: s_value});

        this.dispatchEvent(selectedEvent);
 
    }
}
```
## Explanation:
1. Paginator component(child) contains all buttons(First Page, Last Page, Previous and Next) and Page Size dropdown. Whenever any event    happens,it is creating an event and dispatching it to the Parent component(DisplayCases). And then Parent component handles the event    and does the needful.
2. changeView() method is used to disable/enable the buttons(eg. When we are on the first page, previous button should be disabled).
3. renderedCallback() called after every render of the component. This hook flows from child to parent.
4. Other methods are used for dispatching the events.

# Display cases

## displaycases.html
```javascript
<template>
        <lightning-card title="Case Records" icon-name="custom:custom63">
    
            <div class="slds-m-around_medium">
                <template if:true={cases.data}>
                   <lightning-datatable
                        key-field="Id"
                        data={cases.data}
                        columns={columns}
                        
                       >
                    </lightning-datatable>
                </template>
                <br/>
                <c-paginator  onprevious={previousHandler2} onnext={nextHandler2} onselected={changeHandler2} onfirstpage={firstpagehandler} onlastpage={lastpagehandler}></c-paginator>
            </div>
        </lightning-card>
</template>
```

## displaycases.js
```javascript
import { LightningElement, wire, track, api } from 'lwc';
import getCaseList from '@salesforce/apex/dsiplaycases.getCaseList';
import getNext from '@salesforce/apex/dsiplaycases.getNext';
import getPrevious from '@salesforce/apex/dsiplaycases.getPrevious';
import TotalRecords from '@salesforce/apex/dsiplaycases.TotalRecords';

/* eslint-disable no-console */
 /* eslint-disable no-alert */

 const COLS = [
    { label: 'Case Number', fieldName: 'CaseNumber' },
    { label: 'Subject', fieldName: 'Subject' }

 ];
export default class displaycases extends LightningElement {
@track columns = COLS;
@track v_Offset=0;
@track v_TotalRecords;
@track page_size = 10;

//Fetching records from apex class
@wire(getCaseList, { v_Offset: '$v_Offset', v_pagesize: '$page_size' }) cases;

//Executes on the page load
connectedCallback() {
    TotalRecords().then(result=>{
        this.v_TotalRecords = result;
    });
}

previousHandler2(){
    getPrevious({v_Offset: this.v_Offset, v_pagesize: this.page_size}).then(result=>{
        this.v_Offset = result;
        if(this.v_Offset === 0){
            this.template.querySelector('c-paginator').changeView('trueprevious');
        }else{
            this.template.querySelector('c-paginator').changeView('falsenext');
        }
    });
}
nextHandler2(){
    getNext({v_Offset: this.v_Offset, v_pagesize: this.page_size}).then(result=>{
        this.v_Offset = result;
       if(this.v_Offset + 10 > this.v_TotalRecords){
            this.template.querySelector('c-paginator').changeView('truenext');
        }else{
            this.template.querySelector('c-paginator').changeView('falseprevious');
        }
    });
}

changeHandler2(event){
    const det = event.detail;
    this.page_size = det;
}
firstpagehandler(){
    this.v_Offset = 0;
    this.template.querySelector('c-paginator').changeView('trueprevious');
    this.template.querySelector('c-paginator').changeView('falsenext');
}
lastpagehandler(){
    this.v_Offset = this.v_TotalRecords - (this.v_TotalRecords)%(this.page_size);
    this.template.querySelector('c-paginator').changeView('falseprevious');
    this.template.querySelector('c-paginator').changeView('truenext');
}

}
```
## Explanation:
1. DisplayCases component(parent) is using datatable to display case records.
2. Handlers are setting offset value for records displayed and invoking changeView mathod of child to enable or disable the buttons.
3. The connectedCallback() lifecycle hook fires when a component is inserted into the DOM. This will execute the logic before rendering    of component and after constructor logic execution. Components renders after connectedCallback(). Here in connectedCallback we are      fetching number of case object recordson page load.
