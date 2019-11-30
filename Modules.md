# **@salesforce Modules**

Modules scoped with @salesforce add functionality to Lightning web components at runtime.

Modules that are imported without @salesforce, like lightning/uiRecordApi, contain resources that don't change and are universal to all orgs.

Some modules have a fixed set of module identifiers. For example, the @salesforce/i18n module supports many identifiers including @salesforce/i18n/dir and @salesforce/i18n/lang.

Some modules have a dynamic set of module identifiers. The set is defined by the organization’s metadata. For example, @salesforce/schema accesses an organization’s objects and fields.

```javascript

```

```javascript
@salesforce/apex
Import Apex methods from @salesforce/apex.
import apexMethod from '@salesforce/apex/Namespace.Classname.apexMethod';
@wire(apexMethod, { apexMethodParams })
propertyOrFunction;
// Example
import getAccounts from '@salesforce/apex/MyAccountController.getAccounts';

// Use the wire service to provision data to either a property or a function.
// This example provisions data to a property.
@wire(getAccounts)
accounts;

// This example provisions data to a function.
@wire(getAccounts)
wiredAccounts({ error, data }) {
        if (data) {
            this.accounts = data;
            this.error = undefined;
        } else if (error) {
            this.error = error;
            this.accounts = undefined;
        }
    }

```
import startRequest from '@salesforce/apexContinuation/SampleContinuationClass.startRequest';
apexMethodName—An imported symbol that identifies the Apex method.
apexMethodReference—The name of the Apex method to import.
Classname—The name of the Apex class.
Namespace—The namespace of the Salesforce organization. Specify a namespace unless the organization uses the default namespace (c), in which case don’t specify it.
See Make Long-Running Callouts with Continuations.

```javascript
@salesforce/client/formFactor
// Syntax
import formFactorPropertyName from @salesforce/client/formFactor
// Example
import CLIENT_FORM_FACTOR from @salesforce/client/formFactor
formFactorPropertyName—A name that refers to the form factor of the hardware running the browser. Possible values are:
Large—A desktop client.
Medium—A tablet client.
Small—A phone client.
Pass the form factor to the getRecordCreateDefaults wire adapter to get the default layout information and object information for creating a record.
@salesforce/contentAssetUrl
Import content asset files from @salesforce/contentAssetUrl.
// Syntax
import myContentAsset from '@salesforce/contentAssetUrl/contentAssetReference';
// Syntax for assets in managed packages
import myContentAsset from '@salesforce/contentAssetUrl/namespace__contentAssetReference';
// Example
import SALES_WAVE_LOGO from '@salesforce/contentAssetUrl/SalesWaveLogo';
myContentAsset—A name that refers to the asset file.
contentAssetReference—The name of the asset file.
An asset file name can contain only underscores and alphanumeric characters, and must be unique in your org. It must begin with a letter, not include spaces, not end with an underscore, and not contain two consecutive underscores.

namespace—If the asset file is in a managed package, this value is the namespace of the managed package.
See Access Content Asset Files.
@salesforce/i18n
Import internationalization properties from @salesforce/i18n.
// Syntax
import internationalizationPropertyName from @salesforce/i18n/internationalizationProperty
// Example
import LANG from '@salesforce/i18n/lang';
internationalizationPropertyName—An imported symbol that identifies the internationalization property.
internationalizationProperty—An internationalization property.
The internationalization properties are listed in Access Internalization Properties.
@salesforce/label
Import labels in your Salesforce organization from @salesforce/label.
// Syntax
import labelName from '@salesforce/label/labelReference';
// Example
import greeting from '@salesforce/label/c.greeting';
labelName—A name that refers to the label.
labelReference—The name of the label in your org in the format namespace.labelName. We use this format because it’s the same format used in managed packages, in Visualforce, and in other Salesforce technologies. You can use the same format to access labels, myns.labelName, regardless of where you’re accessing them.
See Access Labels.
@salesforce/messageChannel
Import a Lightning message channel that a component can use to communicate via the Lightning Message Service.
// Syntax
import channelName from '@salesforce/messageChannel/channelReference';
// Syntax for resources in a managed package
import channelName from '@salesforce/messageChannel/namespace__channelReference';
// Example
import SAMPLEMC from '@salesforce/messageChannel/SampleMessageChannel__c';
```
