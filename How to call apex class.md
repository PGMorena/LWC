# Apex Class
```javascript
public with sharing class dsiplaycases {
       @AuraEnabled(cacheable=true)
       public static List<Case> getCaseList(Integer v_Offset, Integer v_pagesize){ 
           return [select id, casenumber, subject from case limit :v_pagesize OFFSET :v_Offset];
       }
}
```

