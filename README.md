### **Rest Booking API Testing with Postman Newman**
This project demonstrates API testing using Postman, providing a collection of tests to validate various endpoints of the API. 

### **Features**

- Tests for GET, POST, PUT, DELETE requests
- Collection of tests covering different API endpoints
- Environment setup for easy switching between environments
- Pre-request scripts for data setup
- Test scripts for assertions and validations

## API Documentation:
https://documenter.getpostman.com/view/39630648/2sAYBSmE2s  
### **Technology used:**
- Postman
- Newman

### **Prerequisite:**
- Node Js
- Newman
- Newman Html Report Library

### **Installation**

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:
 ```console 
  git clone https://github.com/DM9933/API-Testing-for-Restful-Booker-with-Newman-Report.git
```
3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
5. Newman and Report Installation Process:
    - Newman Install Command:
     ```console 
      npm install -g newman
    ```
    - Newman Html Report Install Command:
     ```console 
      npm install -g newman-reporter-htmlextra
    ```
### **Usage**
1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.

## **Testing**

## Test Case Scenarios:

## _**1. Create New Booking**_

### Request URL: https://restful-booker.herokuapp.com/booking/
### Request Method: POST
### Pre-request Script:
```console 
   var firstName = pm.variables.replaceIn("{{$randomFirstName}}")
   console.log(firstName)
   pm.environment.set("FirstName", firstName)

   var lastName = pm.variables.replaceIn("{{$randomLastName}}")
   console.log(lastName)
   pm.environment.set("LastName", lastName)

   var totalPrice = parseInt(pm.variables.replaceIn("{{$randomPrice}}"))
   console.log(totalPrice)
   pm.environment.set("TotalPrice", totalPrice)

   var depositPaid = pm.variables.replaceIn("{{$randomBoolean}}");
   console.log(depositPaid);
   pm.environment.set("DepositePaid", depositPaid);

   const moment = require('moment')
   const today = moment()
   //console.log(today.add(1,'d').add(3,'M').format("YYYY-MM-DD"))
   var checkIn = today.add(1,'d').format("YYYY-MM-DD")
   console.log(checkIn)
   pm.environment.set("CheckIn",checkIn)

   var checkOut = moment(checkIn).add(7, 'd').format("YYYY-MM-DD");
   console.log(checkOut)
   pm.environment.set("CheckOut", checkOut);

   var additionalNeedsList = ["Breakfast", "Lunch", "Dinner"];
   var randomAdditionalNeed = additionalNeedsList[Math.floor(Math.random() * additionalNeedsList.length)];
   pm.environment.set("AdditionalNeeds",randomAdditionalNeed)

```
  **Request Body:** 
 ```console 
{
	"firstname" : "{{FirstName}}",
	"lastname" : "{{LastName}}",
	"totalprice" : {{TotalPrice}},
	"depositpaid" : {{DepositePaid}},
	"bookingdates" : {
    	"checkin" : "{{CheckIn}}",
    	"checkout" : "{{CheckOut}}"
	},
	"additionalneeds" : "{{AdditionalNeeds}}"
}

```
  **Response Body:**
 ```console 
{
    "bookingid": 5035,
    "booking": {
        "firstname": "Marilyne",
        "lastname": "O'Conner",
        "totalprice": 530,
        "depositpaid": false,
        "bookingdates": {
            "checkin": "2024-11-23",
            "checkout": "2024-11-30"
        },
        "additionalneeds": "Lunch"
    }
}
```
 ## _**2. Get Booking Details By ID**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Post-response Script:
```console 
var statusCode = pm.response.code
console.log(statusCode)

if(statusCode == 200){

    var jsonResponse = pm.response.json()

    pm.test("First Name Validation", function(){
        pm.expect(jsonResponse.firstname).to.eql(pm.environment.get("FirstName"))
    })

    pm.test("Last Name Validation", function(){
        pm.expect(jsonResponse.lastname).to.eql(pm.environment.get("LastName"))
    })

    pm.test("Total Price Validation", function(){
        pm.expect(jsonResponse.totalprice).to.eql(pm.environment.get("TotalPrice"))
    })

    pm.test("Deposit Paid Validation", function(){
        pm.expect(jsonResponse.depositpaid.toString()).to.eql(pm.environment.get("DepositePaid"));
    })

    pm.test("CheckIn Validation", function() {
        pm.expect(jsonResponse.bookingdates.checkin).to.eql(pm.environment.get("CheckIn"));
    })

    pm.test("CheckOut Validation", function() {
        pm.expect(jsonResponse.bookingdates.checkout).to.eql(pm.environment.get("CheckOut"));
    })

    pm.test("Additional Need Validation", function() {
        pm.expect(jsonResponse.additionalneeds).to.eql(pm.environment.get("AdditionalNeeds"))
    })

    } else if(statusCode == 404){
    p.test("Not Found")

    }else{
    pm.test("Something Went Wrong")

    }
```

### Response Body:
 ```console 
{
    "firstname": "Marilyne",
    "lastname": "O'Conner",
    "totalprice": 530,
    "depositpaid": false,
    "bookingdates": {
        "checkin": "2024-11-23",
        "checkout": "2024-11-30"
    },
    "additionalneeds": "Lunch"
}
```
## _**3. Create A Token For Authentication.**_
### Request URL: https://restful-booker.herokuapp.com/auth
### Request Method: POST
### Pre-request Script: None
### Request Body:
 ```console 
{
	"username": "admin",
	"password": "password123"
}
```
  **Response Body:**
 ```console 
{
    "token": "04626fdcd921865"
}
```

 ## _**4. Update the Booking Details**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: PUT
### Pre-request Script:
```console 
   	var updated_firstName = pm.variables.replaceIn("{{$randomFirstName}}")
	console.log(updated_firstName)
	pm.environment.set("Updated_FirstName", updated_firstName)

	var updated_lastName = pm.variables.replaceIn("{{$randomLastName}}")
	console.log(updated_lastName)
	pm.environment.set("Updated_LastName", updated_lastName)

	var updated_totalPrice = parseInt(pm.variables.replaceIn("{{$randomPrice}}"))
	console.log(updated_totalPrice)
	pm.environment.set("Updated_TotalPrice", updated_totalPrice)

	var updated_depositPaid = pm.variables.replaceIn("{{$randomBoolean}}");
	console.log(updated_depositPaid);
	pm.environment.set("Updated_DepositePaid", updated_depositPaid);

	const moment = require('moment')
	const today = moment()
	// Use today.clone() to avoid modifying the original `today` moment object
	var updated_checkIn = today.add(2,'d').format("YYYY-MM-DD")
	console.log(updated_checkIn)
	pm.environment.set("Updated_CheckIn", updated_checkIn)

	// Now calculate checkOut using updated_checkIn, ensure `updated_checkIn` is valid
	var updated_checkOut = moment(updated_checkIn).add(2, 'd').format("YYYY-MM-DD");
	console.log(updated_checkOut)
	pm.environment.set("Updated_CheckOut", updated_checkOut);

	var updated_additionalNeedsList = ["Breakfast", "Lunch", "Dinner"];
	var updated_randomAdditionalNeed = updated_additionalNeedsList[Math.floor(Math.random() * updated_additionalNeedsList.length)];
	pm.environment.set("Updated_AdditionalNeeds", updated_randomAdditionalNeed)
```
  **Request Body:** 
 ```console 
{
	"firstname" : "{{Updated_FirstName}}",
	"lastname" : "{{Updated_LastName}}",
	"totalprice" : {{Updated_TotalPrice}},
	"depositpaid" : {{Updated_DepositePaid}},
	"bookingdates" : {
    	"checkin" : "{{Updated_CheckIn}}",
    	"checkout" : "{{Updated_CheckOut}}"
	},
	"additionalneeds" : "{{Updated_AdditionalNeeds}}"
}

```
  **Response Body:**
 ```console 
{
    "firstname": "Alejandrin",
    "lastname": "Reichert",
    "totalprice": 203,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-11-24",
        "checkout": "2024-11-26"
    },
    "additionalneeds": "Breakfast"
}
```
## _**5. Get Verify After Booking Details By ID**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Post-response Script:
```console 
	var jsonResponse = pm.response.json()

	pm.test("Updated First Name Validation", function(){
    	pm.expect(jsonResponse.firstname).to.eql(pm.environment.get("Updated_FirstName"))
	});

	pm.test("Updated Last Name Validation", function(){
    	pm.expect(jsonResponse.lastname).to.eql(pm.environment.get("Updated_LastName"))
	});

	pm.test("Updated Total Price Validation", function(){
    	pm.expect(jsonResponse.totalprice).to.eql(pm.environment.get("Updated_TotalPrice"))
	});

	pm.test("Updated Deposit Paid Validation", function(){
    	pm.expect(jsonResponse.depositpaid.toString()).to.eql(pm.environment.get("Updated_DepositePaid"));
	});

	pm.test("Updated CheckIn Validation", function() {
    	pm.expect(jsonResponse.bookingdates.checkin).to.eql(pm.environment.get("Updated_CheckIn"));
	});

	pm.test("Updated CheckOut Validation", function() {
    	pm.expect(jsonResponse.bookingdates.checkout).to.eql(pm.environment.get("Updated_CheckOut"));
	});

	pm.test("Updated Additional Need Validation", function() {
    	pm.expect(jsonResponse.additionalneeds).to.eql(pm.environment.get("Updated_AdditionalNeeds"))
	});
```

### Response Body:
 ```console 
{
    "firstname": "Alejandrin",
    "lastname": "Reichert",
    "totalprice": 203,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-11-24",
        "checkout": "2024-11-26"
    },
    "additionalneeds": "Breakfast"
}
```
## _**6. Delete Booking Record**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: DELETE
### Response Body: None 

## Run Command:  
- Run Command for Console: 
```console 
newman run APITesting.postman_collection.json -e APITesting.postman_environment.json 
```
- Run Command for Report: 
```console 
newman run APITesting.postman_collection.json -e APITesting.postman_environment.json -r htmlextra
```

## Newman Summary Report :
![Newman Summary Report](./Images_Of_Newman_Report/Newman%20Summary%20Report.png)
## Newman Total Request Report:
![Newman Total Request Report](./Images_Of_Newman_Report/Newman%20Total%20Request%20Report.png)
## Newman Failed Report :
![Newman Failed Report](./Images_Of_Newman_Report/Newman%20Failed%20Report.png)
## Newman Skipped Report :
![Newman Skipped Report](./Images_Of_Newman_Report/Newman%20Skipped%20Report.png)



