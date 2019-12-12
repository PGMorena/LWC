# Call Lightning Web Component in VF Page
Steps are below:
1. Create Lightning Web Component
2. Create a Lightning App
3. Add LWC dependency in Lightning App
4. Create a VF Page where you want to call Lightning web component
5. Inside script of VF page, use $Lightning.use(See the sample code below)

# Handling LWC events in VF Page
steps are below:
1. Use window.postMessage to dispatch your event
2. Handle in VF page using addEventListener(See sample code)

## LWC
```javascript
.html
<template>
   <lightning-layout>
    <lightning-layout-item>
       <lightning-input type="string"  label="Enter a value in LWC" placeholder="Msg to vf page"></lightning-input>
        <lightning-button label="Click"  onclick={clickHandler}></lightning-button>
    </lightning-layout-item>
</lightning-layout>
</template>

.js
import { LightningElement} from 'lwc';
/* eslint-disable no-console */
 /* eslint-disable no-alert */
export default class VisualForceLwc extends LightningElement {

    vfOrigin = "https://rs60033-dev-ed--c.ap5.visual.force.com";
    clickHandler(){
        const inpt = this.template.querySelector("lightning-input").value;
        window.postMessage(inpt, this.vfOrigin);
    }
}
```
## Lightning App
```javascript
<aura:application access="GLOBAL" extends="ltng:outApp">
  <aura:dependency resource="c:visualForceLwc"/> 
</aura:application>
```
## VF Page
```javascript
<apex:page >
    <apex:includeLightning />
    <div id="lwcDemo" />
    <p id="output1"></p>
    <script>
        window.addEventListener("message", function(event) {
            document.getElementById("output1").innerHTML = event.data;
        });
                
         $Lightning.use("c:VF_LWC_App", function() {
                $Lightning.createComponent("c:visualForceLwc", {},
                "lwcDemo",
                function() {}
               );
            });
        
    </script>
</apex:page>
```
