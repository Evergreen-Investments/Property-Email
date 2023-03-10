# Property Emails 

The property email workflows and functions allow users to add to the related list of properties, and send an email automatically using an email template that was created. 

## Solution Creation
1. Go into Deals > Deal Record > Select Property > Choose Specific Property and add to the field > Send Email and select yes. The field will be automatically changed to no when the tab is refreshed. 

![image](https://user-images.githubusercontent.com/124835926/217658823-3d99df7b-75d7-44d8-a3c9-04a7b04c102c.png)

2. Find the APIs used for each field, so you are able to access that field in the IDE. Go to Settings > Developer Space > APIs > PropertyDatabase and find "Conservative Net to Seller" and "Seller's Desired Net" API names

![image](https://user-images.githubusercontent.com/124835926/217661510-ec6d4efc-4bcd-442d-ae47-9e75cf3136e0.png)
![image](https://user-images.githubusercontent.com/124835926/217661572-f643a7c4-f320-4b5b-8dac-94f7d6562d54.png)


3. Edit the arguments and made the “key” name and Property_ID. This would allow us in the first line to access all the information of each account by using "getRecordBy"
![image](https://user-images.githubusercontent.com/124835926/217657957-6f305653-640c-4887-ba43-a0d6d8d105c2.png)
```bash
Recs = zoho.crm.getRecordById("PropertyDatabase", Property_ID);
```
4.Create a variable “SellerDN” where we accessed the sellers desired net by using “get” and finding the corresponding APIs.
```bash
SellerDN = Recs.get("Asking_Price");
```
5.Repeat with Conservative Net to Seller data and Property Type 
```bash
ConNTS = Recs.get("MaxBidWholesalerRound");
PropType =  Recs.get("Property_Profile_Type");
```
7. Checks to see if Property Profile Type is Residential Income
```bash
if(PropType == "Residential Income"){
```
8. Compare them using a if else conditional.
```bash
if ( SellerDN <= ConNTS)
```
9.If the sellers desired net was less than the conservative net to seller value then it will update the status of the account as “Pass Analysis”, by using “Map()”, which allows us to take a variable with the new value, and put it into the “status” field in the account.
```bash
	Status_Update = Map();
	Status_Pass = "Pass Analysis";
	Status_Update.put("Status", Status_Pass);
```
9.For the Else aspect this is where the "Fail Analysis" is updated by using the same “put” function.
```bash
  Status_Update = Map();
	Status_Fail = "Fail Analysis";
	Status_Update.put("Status", Status_Fail);
```
10.Finally, make a variable outside of the condition to actually push the Update into the account, using "zoho.crm.UpdateRecord" along with already used the already made variables as the parameters, and allow the workflow to be triggered.
```bash
Update = zoho.crm.updateRecord("PropertyDatabase",Property_ID,Status_Update,{"trigger":{"workflow"}});
```

## Diagram
![image](https://user-images.githubusercontent.com/124835926/224370641-fe2eb6a1-5e56-4792-81b7-fad54e7588eb.png)
![image](https://user-images.githubusercontent.com/124835926/224372931-82759e6c-ef8e-47fd-bfb2-238e5cb78c51.png)



