# Introduction

Welcome to the NoHassls API. You can use our API to access NoHassls API endpoints, which can provide information on lead prospects, requests, and insurance demands from customers, check your current billing, stats and other amazing things.

We have language bindings in Shell, PHP, and Javascript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs on the top right.

This documentation is organised to mimic the workflow of making a NoHassls API request.

The first step is to query for any new insurance profiles (QuoteRequest Object). These are the details of an insurance request made by a customer. This contains all the details they have supplied, allowing an anonymous insurance quotation to be provided. This does not contain any identifying information about the client.

Pinnacle Life will return a QuoteReply Object that contains information about the quote. This includes data such as the cost per month, annual cost, product details, availability and other data concerning the quote.

A notification is then sent to the customer that the Pinnacle quotation has been supplied and is available for scrutiny. 

If the customer decides to accept the quotation, then a ContactRequest Object will be added to the Pinnacle ContactRequestQueue API notifying you that a customer wishes to be contacted.  The ContactRequest Object will contain the preferred contact time and date supplied by the customer and any additional questions or information.

Pinnacle Life will create a ContactReply Object and post it to the ContactReplyQueue. The customer will then be notifed that the time slot requested is available and Pinnacle are ready to contact them.

The customer agrees to release his/her private information to Pinnacle and a CustomerProfileObject will be added to the CustomerProfileQueue for retrieval by Pinnacle Life.


The CustomerProfileObject will contain the ID of the original Insurance Pofile, the ID of the Pinnacle profile response and the ID of the User Profile record. A UserProfile record will contain the users contact information, name, address and any other private information including email address and telephone number.



Pinnacle Life now have all the information required to initiate contact with the customer and continue the application process.

![](http://nohassls.co.nz/static/images/qrs.png)


# Authentication

> To authorize, use this code:

```ruby
require 'NoHassls'

api = NoHassls::APIClient.authorize!('PINNACLE-KEY')
```

```php
import NoHassls

api = NoHassls.authorize('PINNACLE-KEY')
```

```Javascript
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: PINNACLE-KEY"
```

> Make sure to replace `PINNACLE-KEY` with your API key.

NoHassls uses API keys to allow access to the API. These keys are generated and maintained by the NoHassls Development Team. For more information about your key please visit our [developer portal](http://developers.nohassls.co.nz).

NoHassls expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: PINNACLE-KEY`

Any request that does not contain this key will be refused with a 403 FORBIDDEN return code.
<aside class="notice">
You must replace <code>PINNACLE-KEY</code> with your Pinnacle API key.
</aside>



# Versioning

> If you fail to add this header to any call, or pass an unknown version value an error will be returned


NoHassls requires you to pass an Accept Header with a value in the following format:

`Accept: application/vnd.nohassls.v1+json`

In the example shown above, this would result in the V1 version of the endpoint being called. 



<aside class="notice">
Please ensure you pass the version header with all calls you make to the NoHassls API
</aside>









# Insurance Requests



### HTTP Request



## General Query Parameters supported on all GET requests

Parameter | Default | Description
--------- | ------- | -----------
query     | NULL  | Filter. e.g. col1:val1,col2:val2 (id:1) or [postcode:2205,pool:no] to retrieve all quote requests that have a postcode value of 2205 and the value no for a pool.
fields    | NULL  | Fields returned. e.g. Id,Pool will return only those fields. NOTE the fields are case sensitive, passing the incorrect value will result in an error. See the schema definitions for field naming information.
sortby    | NULL  | Sorted-by fields. e.g. col1,col2 (Must be used with order param. See Below)
order     | NULL  | Order corresponding to each sortby field, if single value, apply to all sortby fields. e.g. desc,asc  e.g &sortby=Id,Pool&order=asc,desc
limit     | NULL  | Limit the size of result set. Must be an integer
offset    | NULL  | Start position of result set. Must be an integer


## Insurance Endpoints

Shown below is are the endpoints for Insurance functions. Life Insurance is shown in the example. Below is a listing of the basic querying endpoints for insurance requests.
{insurance_type} parameters are explained in the Insurance Type Parameters section below.

Type    | Returns  | Endpoint | Description
------- |--------- | -------  | ------------
GET     | List     | http://pinnacle.nohassls.co.nz:8080/{insurance_type}/getall | Returns a list of {insurance_type} insurance records.
GET     | Single   | http://pinnacle.nohassls.co.nz:8080/{insurance_type}/get?id= | Returns record that matches passed id.
POST     | Single     | http://pinnacle.nohassls.co.nz:8080/{insurance_type}/post | Creates a new record.
PUT     | Single     | http://pinnacle.nohassls.co.nz:8080/{insurance_type}/put | Updates an existing record.





### Insurance Type Parameters
Most are quite obvious and can easily be remembered. Rather than a cryptic string or number reference, we use a simple string value in the paramater to retrieve quotation requests of that type. We dont worry about adding the word insurance, as it saves a few bytes and shortens the URL.

Parameter | Description
--------- | ------- |
life      | Life Insurance
funeral   | Funeral Insurance
mortgage  | Mortgate Insurance


#### The following insurance types are also provided by NoHassls, and can be made available to Pinnacle Life upon request.

Parameter | Description
--------- | ------- |
car       | Car Insurance
bike      | Motorcycle Insurance
boat      | Boat Insurance
caravan   | Caravan Insurance
collectors| Collector Car Insurance
health    | Health Insurance
home      | Home & Contents Insurance
income    | Income Insurance
landlord  | Landlord Insurance
pet       | Pet Insurance
travel    | Travel Insurance

<aside class="success">
Remember — all requests must include the Accept and Authorisation Headers as explained above. An authenticated user is a happy user.
</aside>



# Quote Request Resources

## Life Insurance

<aside class="info">
NoHassls API uses Ints to represent true and false values. 0 for a false value and 1 for a true value. These are displayed below as Int (bool).
</aside>

An example of the JSON response maybe found after the below table.


| Column     | Value  | Desc
|--------|----------|----------|
|id|           Int   | The ID of the resource |
|batch_id|     Int	 | Internal Use Only |
|gender|      String (2)| M, F or T |
|smoker|       Int (bool)   | Is the applicant a smoker |
|last_smoked|      Int| No of months since last smoked |
|age|      Int      | The age of the applicat |
|cover_amount|      Int    | Cover amount requested |
|country_residence|      String (45)| The country of residence of the applicat     |
|residency_status|  String (45)    |  The residency status, ie, visa, permanent, etc     |
|height|  Int    | The height of the applicant in cm's     |
|weight|  Int    | The weight of the applicant in kilograms    |
|cancer|  Int (bool)   | Has the applicant had cancer.     |
|diabetes| Int (bool)    | Has the applicant ever had diabetes.     |
|diabetes_complications| Int (bool)     | Additional health complications suffered due to diabetes?     |
|blood_disorder| Int (bool)    | Has the applicant suffered a blood_disorder.     |
|blood_disorder_type|  String (255)    |  Details concering the type of blood disorder suffered.    |
|cancer_type|  String (255)    | Details about the cancer type suffered.     |
|cancer_spread| Int (bool)     | Has the cancer spread to other areas of the body?     |
|cancer_clear_10| Int (bool)     | Has the applicant been free of cancer for 10 years or more?     |
|blood_pressure| Int (bool)     | Does the applicant suffered from high blood pressure     |
|blood_pressure_treatment|  String (255)    | Details of treatments used for high blood pressure.     |
|cholesterol|  Int (bool)    | Does the application suffer from high cholesterol?     |
|cholesterol_treatment| String (255)     |  Details of the treament for Cholesterol.     |
|cholesterol_check_12|  Int (bool)    | Has the applicant had a cholesterol check in the last 12 months?|
|heart_problem| Int (bool)     | Has the applicant previously experienced heart problems?     |
|heart_problem_details| String (255)     | Details of the treatment for Heart Problems      |
|kidney_problem| Int (bool)     | Has the applicant previously suffered from Kidney related problems?     |
|kydney_bladder_problem_details| String (255)     | Details of the Kidney Bladder problems experienced.     |
|gastro_intestinal|  Int (bool)    | Has the applicant experienced any gastro intestinal related illness?     |
|gastro_problem_details| String (255)     | Details of the gastro, intestinal illness      |
|gastritis_episodes| Int (bool)     | The number of gastritis episodes suffered a week     |
|bladder_problem| Int (bool)     | Has the applicant experienced any bladder related illness?     |
|lung_problem|  Int (bool)    | Has the applicant experienced any lung or respitory illess?     |
|hernia_further_problems| Int (bool)     | Has the applicant suffered from any further problems due to a hernia?     |
|gallstones_removed|  Int (bool)    | Have the gallstones been removed?     |
|gallstones_ongoing|  Int (bool)    | Does the applicant suffer from ongoing gallstones?     |
|ulcer_hospitialised|  Int (bool)    |  Has the applicant been hospitalised due to ulcers?    |
|gastro_other_treatment| Int (bool)     | Has the applicant utilised other treatments for gastro?     |
|gastro_problems_5y| Int (bool)     | Has the applicant experienced any gastro intestinal problems in the last 5 years?      |
|lung_problem_details|  String (255)     |  Details of the lung problems experienced.     |
|neurological|  Int (bool)    | Has the applicant experienced any neurological problems?     |
|mental_health_5y|  Int (bool) |  Has the applicants experienced any mental health issues in the previous 5 years?     |
|neurological_problem_details| String (255)     | Details of the Neurological problems experienced.     |
|muscular_skeletal| int (bool)     |  Has the applicant experienced any Muscular or Skeletal Problems?    |
|muscular_skeletal_problem_details|  String (255)    |  Details of the Muscular or Skeletal problems experienced.     |
|muscular_skeletal_neck_back| Int (bool)     | Has the applicant experienved any muscular or skeletal neck or back problems or injuries?     |
|muscle_injury_strain_symptoms_2y| Int (bool)      | Has the applicant experienved any muscular injury strain symptoms in the last 2 years?     |
|fracture_back_neck_skull| Int (bool)     | Has the applicant ever experienved a fracture to the neck, back or skull?      |
|fracture_any_problems_2y| Int (bool)     |  Has the applicant suffered any problem due to the nexk, back or skull injury in the last 2 years.     |
|arthritis_details|  String (255)    | Details concerning the arthritis experienced by the applicant.      |
|gout_tendonitis_tenosynovitis_2y|      |      |
|muscular_skeletal_sports_injury|      |      |
|muscular_skeletal_joint_neck_back|      |      |
|mental_health_details|      |      |
|alcohol_28_week|      |      |
|illegal_drugs_5y|      |      |
|hiv_positive|      |      |
|alcohol_professional_help|      |      |
|alcohol_detox_hospital|      |      |
|illegal_drug_use_details|      |      |
|other_medical_conditions|      |      |
|family_member_cancer_60|      |      |
|seeking_medical_advice_currently_details|      |      |
|thyroid_problem_2y|      |      |
|thryoid_surgery_radio_6m|      |      |
|current_medical_treatment_professions|      |      |
|family_member_illness_details|      |      |
|family_2_or_more_heart_disease|      |      |
|family_2_or_more_stroke|      |      |
|kidney_disease_polycystic|      |      |
|family_2_or_more_diabetes|      |      |
|cancer_type_diagnosed|      |      |
|family_count_colon_rectal_60|      |      |
|colon_rectal_before_50|      |      |
|family_count_prostate_cancer_60|      |      |
|prostate_cancer_before_50|      |      |
|family_2_more_same_cancer_60|      |      |
|cancer_before_50|      |      |
|family_count_testicular_cancer_|      |      |
|testicular_cancer_before_50|      |      |
|family_count_ms_60|      |      |
|family_count_parkinsons_60|      |      |
|family_count_motor_neurone_60|      |      |
|risky_occupation|      |      |
|recreational_activities|      |      |
|type_of_cancer_when_diagnosed|      |      |
|cancert_medicated_treated|      |      |
|cancer_state_remission|      |      |
|diabetes_first_diagnosed|      |      |
|diabetes_medication_control|      |      |
|diabetes_tests_12m|      |      |
|blood_disorder_diagnosed|      |      |
|blood_disorder_medication|      |      |
|blood_disorder_tests_12m|      |      |
|blood_pressure_high_diagnosed|      |      |
|blood_pressure_medication|      |      |
|blood_pressure_test|      |      |
|cholesterol_diagnosed|      |      |
|cholestrol_medication|      |      |
|cholesterol_test|      |      |
|heart_condition_diagnosed|      |      |
|heart_condition_medication|      |      |
|heart_condition_test_12m|      |      |
|gastro_intestinal_diagnosed|      |      |
|gastro_intestinal_medication|      |      |
|gastro_intestinal_current_state|      |      |
|kidney_bladder_diagnosed|      |      |
|kidney_bladder_medication|      |      |
|kidney_bladder_current_state|      |      |
|lung_problem_diagnosed|      |      |
|lung_problem_medication|      |      |
|lung_problem_current_state|      |      |
|neurological_diagnosed|      |      |
|neurological_medication|      |      |
|neurological_current_state|      |      |
|muscular_skeletal_diagnosed|      |      |
|muscular_skeletal_medication|      |      |
|muscular_skeletal_current_status|      |      |
|mental_health_diagnosed|      |      |
|mental_health_caused_event_illness|      |      |
|mental_health_medication|      |      |
|alcohol_typical_use|      |      |
|alcohol_consulted_professional|      |      |
|alcohol_alcoholism_or_counselling|      |      |
|illegal_drugs_use|      |      |
|illegal_drugs_consulted_professional|      |      |
|illegal_drugs_have_stopped_period|      |      |
|current_medical_advice_symptoms|      |      |
|current_medical_advice_profession|      |      |
|current_medical_advice_tests|      |      |
|family_members_illness_list|      |      |
|family_members_medical_tests_you_had_related|      |      |





`json

[
{
    "Id": 1,
    "Postcode": 2205,
    "ProposedStartDate": "2015-12-01T00:00:00+11:00",
    "EstimatedYearConstruction": 2010,
    "NumberStoreys": 7,
    "NumberBathrooms": 2,
    "NumberBedrooms": 3,
    "BedroomSize": "30",
    "HomeDesc": "nice unit",
    "ExteriorWalls": "concrete",
    "RoofMaterial": "another unit",
    "HomeOccupied": "yes",
    "StrataPlan": "yes",
    "Pool": "no",
    "Name": "our home",
    "DobOldestPolicyHolder": "2015-10-01T00:00:00+10:00",
    "EntitledNoClaim": 1,
    "AnyBelowGround": 1,
    "SecurityDevices": "security building",
    "DoorSecurityDevices": "locks",
    "WindowSecurityDevices": "7 floor drop",
    "Alarm": 1,
    "LandLargerNormal": 0,
    "HadPolicyCancelled": 0,
    "NumTimesClaimDeclined": 0,
    "NumClaimsTotal": 0,
    "NumConvictions": 1,
    "CoverStormDmgGatesFences": 1,
    "CoverAccidentalDmg": 1,
    "Over50Discount": 1,
    "Carports": 2,
    "BalconiesDecks": 2,
    "Verandahs": 2,
    "EuroKitchenAppliances": 1,
    "GraniteMarbleTiling": 1,
    "LargeGlazedAreas": 1,
    "PlantationShutters": 0,
    "CurvedWalls": 0,
    "DuctedAircon": 1,
    "TennisCourt": 0,
    "IngroundPool": 0,
    "Watertight": 1,
    "NewHomeUnderConstruction": 0,
    "UnderRenovation": 0,
    "HomeUse": "home",
    "Mortgate": 1,
    "InsureNameOrCompany": "no current insurer",
    "NumOwnersNamedPolicy": 2,
    "Comments": "some comments",
    "Status": 0
  }
]


`




This endpoint retrieves all  insurance quotation requests.

<aside class="success">
Remember — all requests must include your key. 
</aside>


## Get a Specific Insurance Request

```ruby
require 'nohassls'

api = nohassls::APIClient.authorize!('meowmeowmeow')
api.insurancerequest.get(2)
```

```python
import nohassls

api = nohassls.authorize('meowmeowmeow')
api.insurancerequest.get(2)
```

```bash
curl "http://nohassls.co.nz/api/insurance/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
    "Id": 1,
    "Postcode": 2205,
    "ProposedStartDate": "2015-12-01T00:00:00+11:00",
    "EstimatedYearConstruction": 2010,
    "NumberStoreys": 7,
    "NumberBathrooms": 2,
    "NumberBedrooms": 3,
    "BedroomSize": "30",
    "HomeDesc": "nice unit",
    "ExteriorWalls": "concrete",
    "RoofMaterial": "another unit",
    "HomeOccupied": "yes",
    "StrataPlan": "yes",
    "Pool": "no",
    "Name": "our home",
    "DobOldestPolicyHolder": "2015-10-01T00:00:00+10:00",
    "EntitledNoClaim": 1,
    "AnyBelowGround": 1,
    "SecurityDevices": "security building",
    "DoorSecurityDevices": "locks",
    "WindowSecurityDevices": "7 floor drop",
    "Alarm": 1,
    "LandLargerNormal": 0,
    "HadPolicyCancelled": 0,
    "NumTimesClaimDeclined": 0,
    "NumClaimsTotal": 0,
    "NumConvictions": 1,
    "CoverStormDmgGatesFences": 1,
    "CoverAccidentalDmg": 1,
    "Over50Discount": 1,
    "Carports": 2,
    "BalconiesDecks": 2,
    "Verandahs": 2,
    "EuroKitchenAppliances": 1,
    "GraniteMarbleTiling": 1,
    "LargeGlazedAreas": 1,
    "PlantationShutters": 0,
    "CurvedWalls": 0,
    "DuctedAircon": 1,
    "TennisCourt": 0,
    "IngroundPool": 0,
    "Watertight": 1,
    "NewHomeUnderConstruction": 0,
    "UnderRenovation": 0,
    "HomeUse": "home",
    "Mortgate": 1,
    "InsureNameOrCompany": "no current insurer",
    "NumOwnersNamedPolicy": 2,
    "Comments": "some comments",
    "Status": 0
  }
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve
