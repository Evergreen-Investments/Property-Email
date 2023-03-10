# Property Emails 

The property email workflows and functions allow users to add to the related list of properties, and send an email automatically using an email template that was created. 

## Solution Creation
1. Go into Deals > Deal Record > Select Property > Choose Specific Property and add to the field > Send Email and select yes. The field will be automatically changed to no when the tab is refreshed. 

![image](https://user-images.githubusercontent.com/124835926/224374285-f97836bc-6bf5-4390-808d-f683e799dc13.png)

![image](https://user-images.githubusercontent.com/124835926/224374113-15f0462c-7c70-4faa-ac05-5899ae29120c.png)


2. This will then trigger workflows that will then create a record in the DealsxProperties module. Duplicate records will not be created because a unique ID that combines the property id and deal id is created so it will not send duplicate emails or records. 

![image](https://user-images.githubusercontent.com/124835926/224375033-c76f8cda-4f10-4b94-97a2-f6f0494652ab.png)

![image](https://user-images.githubusercontent.com/124835926/224374833-b2d9882f-c6e1-4ab5-85ad-999627aae136.png))


3. Once the record is created, a workflow is triggered that sends an email out to the contact email of the deal.
4. 
![image](https://user-images.githubusercontent.com/124835926/217657957-6f305653-640c-4887-ba43-a0d6d8d105c2.png)

![image](https://user-images.githubusercontent.com/124835926/224375369-acc78576-6a52-4c76-9cfd-f39bf047292b.png)
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

## Diagrams
![image](https://user-images.githubusercontent.com/124835926/224370641-fe2eb6a1-5e56-4792-81b7-fad54e7588eb.png)
![image](https://user-images.githubusercontent.com/124835926/224372931-82759e6c-ef8e-47fd-bfb2-238e5cb78c51.png)



