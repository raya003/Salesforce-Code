public with sharing class CasesController 
{   
    public Case caseObj{get;set;}
    private ApexPages.StandardSetController standardController;
    public List<Case> selectedCases;
    public CasesController(ApexPages.StandardSetController standardController)
    {    
        caseObj = new Case();
        caseObj.Status = 'Closed';
        this.standardController = standardController;           
    }
    
    public PageReference getSelectedValue(){    
        selectedCases = (List<Case>) standardController.getSelected();
         return null;      
    }
    
    public PageReference updateCases()
    { 
        try{
                
            for(Case selectedCase : selectedCases)
            {                
                selectedCase.Status = caseObj.Status;
                selectedCase.Request_Type_E2C__c = caseObj.Request_Type_E2C__c;
                selectedCase.Request_Category_E2C__c = caseObj.Request_Category_E2C__c;
                
            }
            if(!selectedCases.isEmpty()){       
                update selectedCases;
                Schema.DescribeSObjectResult result = Case.SObjectType.getDescribe();
                PageReference pageRef = new PageReference('/' + result.getKeyPrefix());
                pageRef.setRedirect(true);
                return pageRef;
            }
        } catch(Exception e){
             ApexPages.Message myMsg = new ApexPages.Message(ApexPages.Severity.ERROR,' '+e.getMessage()+' '+e.getStackTraceString());
             ApexPages.addMessage(myMsg);
        }
        return null;
    }
    
    public PageReference cancel()
    {       
      Schema.DescribeSObjectResult result = Case.SObjectType.getDescribe();
      PageReference pageRef = new PageReference('/' + result.getKeyPrefix());
      pageRef.setRedirect(true);
      return pageRef;
  
    }
}
