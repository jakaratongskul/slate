---
title: API Reference (Developer) | MISTER PRAKAN

language_tabs:
  - javascript

toc_footers:
  - <a href='https://misterprakan.com/developers/signup.aspx'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Mister Prakan (MP) API! You can use our API to access MP API endpoints, which can get information on various insurance campaigns, premiums, coverage details in our database.

We have language bindings in C# and more are coming! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

MP uses API keys to allow access to the API. You can register a new MP API key at our [developer portal](https://misterprakan.com/developers/Default.aspx).

MP expects for the API key to be included in all API requests to the server looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Motors(Car) API

## Get Makes

```javascript

$.ajax({
 type: "POST",
 contentType: "application/json; charset=utf-8",
 url: '/api/motor/mtquotes.asmx/getmakes',
 data: "{}",
 dataType: "json",
 success: function (data) {
   $(data.d).each(function (index, value) {
     //example
     $("#quotes").append("<tr><td>" + value.manu + "</td></tr>");
   });
 },
});
```
> The above command returns JSON structured like this:

```json
[
  {
    "carmanuid": "19",
    "manu": "TOYOTA",
    "country": "J"
  }
  {
    "carmanuid": "15",
    "manu": "HONDA",
    "country": "J"
  }
]
```

This endpoint retrieves the makes of the car (for insurance only).

### HTTP Request

`POST https://misterprakan.com/api/motor/mtquotes.asmx/getmakes`

### JSON Vars

Var | Description
--------- | -----------
carmanuid | ID of the premium
manu | Logo of the insurance company
country | Name of the insurance company in Thai

## Get Models

```javascript

var aData = ['TOYOTA'];
var jsonData = JSON.stringify({ aData: aData });

$.ajax({
 type: "POST",
 contentType: "application/json; charset=utf-8",
 url: '/api/motor/mtquotes.asmx/getmodels',
 data: jsonData,
 dataType: "json",
 success: function (data) {
   $(data.d).each(function (index, value) {
     //example
     $("#quotes").append("<tr><td>" + value.model + "</td></tr>");
   });
 },
});
```
> The above command returns JSON structured like this:

```json
[
  {
    "carmodelid": "4",
    "carmodel": "YARIS",
    "cargroup": "5",
    "amount": "599000"
  }
  {
    "carmodelid": "3",
    "carmodel": "VIOS",
    "cargroup": "J",
    "amount": "619000"
  }
]
```

This endpoint retrieves the models based on the car make (for insurance only).

### HTTP Request

`POST https://misterprakan.com/api/motor/mtquotes.asmx/getmodels`

### Query Parameters

Parameter | Mandatory | Values | Description
--------- | ------- | ------- | -----------
make | Yes |'TOYOTA' |Car's Make

### JSON Vars

Var | Description
--------- | -----------
carmodelid | ID of the model
carmodel | Car's model
cargroup | Car's insurance group
amount | Car's new amount (Red Plate)

## Get Quotes

```javascript
var aData = ['12345','2016','TOYOTA','YARIS',null,null,'1'];
var jsonData = JSON.stringify({ aData: aData });

$.ajax({
 type: "POST",
 contentType: "application/json; charset=utf-8",
 url: '/api/motor/mtquotes.asmx/getquotes',
 data: jsonData,
 dataType: "json",
 success: function (data) {
   $(data.d).each(function (index, value) {
     //example
     $("#quotes").append("<tr><td>" + value.premiumid + "</td></tr>");
   });
 },
});
```

> The above command returns JSON structured like this:

```json
[
  {
    "premiumid": 1,
    "logo": "vib.jpg",
    "company": "วิริยะ",
    "engcompany" : "Viriyah",
    "rating": 5,
    "rec": 1,
    "cargroup": 1,
    "type": 1,
    "condition": "อู่",
    "deduct": 5000,
    "netpremium": 12000,
    "totalpremium": 15000,
    "salepremium": 10000,
    "salecom": 5000,
    "owndamage": 500000,
    "fire": 500000,
    "theft": 500000,
    "one": 1,
    "two": 1,
    "three": 1,
    "four": 1,
    "five": 1,
    "six": 1,
    "seven": 1,
    "special" : "Free Starbuck Card!",
    "empid": "MP"
  }
]
```

This endpoint retrieves all car insurance premium campaigns.

### HTTP Request

`POST https://misterprakan.com/api/motor/mtquotes.asmx/getquotes`

### Query Parameters

Parameter | Mandatory | Values | Description
--------- | ------- | ------- | -----------
authcode | Yes |'authcode'| Your Authentication code
year | Yes |'2016' |Car's Year
make | Yes | 'TOYOTA' |Car's Make
model | Yes |'YARIS' |Car's Model
condition | No |'อู่' or 'ห้าง' |Car insurance condition. NULL to include ALL
city | No | 'กรุงเทพ' or 'ต่างจังหวัด'|Car's registered city. NULL if don't know
class | Yes | '1' or '2+' or '3+' or '3'| Insurance Class


### JSON Vars

Var | Description
--------- | -----------
premiumid | ID of the premium
logo | Logo of the insurance company
company | Name of the insurance company in Thai
engcompany | Name of the insurance company in English
rating | Rating of this insurance campaign. 0 to 5
rec | 1 or 0. 1 = Recommended, 0 = Not recommended
cargroup | Group of the car based on insurance company
type | Insurance Class. 1, 2+, 3+ or 3
deduct | Deductible amount
netpremium | Net premium amount (amount before VAT & Stamps)
totalpremium | Total premium amount (This amount is the full price)
salepremium | Premium after commission (amount after commission)
salecom | commission amount (if sold at full price)
owndamage | Own damage amount
fire | Fire damage amount
theft | Theft damage amount
one | Third-party : Death or Dismemberment / Person
two | Third-party : Death or Dismemberment / Time
three | Third-party : Public Liability
four | Personal Accident
five | Medical Expenses
six | Bail Bond
seven | Number of people covered in a car
special | Promotion
empid | Your id (needed when using the API to transfer of leads,sales,etc.)

## Send Lead/Sales
