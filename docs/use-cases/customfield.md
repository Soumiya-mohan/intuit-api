---
layout: default
title: Custom Fields
nav_order: 18
parent: Use Cases
---

## Custom Fields

The APIs related to the Custom Fields allow you to manage and sync custom fields into QuickBooks Online. 
The Custom Fields API provides support for create, read, update, and disable operations. 
When creating a Custom Field, you can create associations with entities. 
You can also add custom fields to transactions and other entities by configuring the custom field definition ID while creating the transaction.

This page outlines - 
- The Create/Read operations for Custom Fields.
- The Create operation for Invoice (sales transaction) with Custom Fields. 
- The Create operation for Customer with Custom Fields.

Currently, custom fields are supported for the following transactions and entities.

- Supported Transactions -
    - Estimate
    - Invoice
    - Sales Receipt
    - Refund Receipt
    - Credit Memo
    - Bill
    - Expense
    - Purchase Order
    - Vendor Credit
   
- Supported entities -     
    - Customer
    - Vendor

### Integration Diagram

![](/intuit-api/assets/images/CustomField.png)


### Operations for Custom Fields entity

- [Read](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#read-custom-fields) - Query (POST)
- [Create](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#create-custom-field) - Mutation (POST)
- [Update](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#update-custom-field) - Mutation (POST)
- [Disable](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#disable-custom-field) -Mutation (POST)

### Scopes

-   app-foundations.custom-field-definitions.read: Allows access to read Custom field definitions data
-   app-foundations.custom-field-definitions: Allows access to read and write Custom field definitions data
-   com.intuit.quickbooks.accounting: For V3 Accounting REST API access


### Endpoints

-   GraphQL API:  https://qb.api.intuit.com/graphql 
-   V3 Accounting REST API: https://quickbooks.api.intuit.com/v3/company/{realmId}/{entityname}?minorversion=70&include=enhancedAllCustomFields

### Required headers

-   Content-type: **application/json**
-   Authorization: **Bearer access_token**

Note: Use tokens generated using scopes mentioned above in the authorization header for all other calls shown here.
 
### Use Cases




#### Use Case 1: Read Custom Fields Definitions
Use [Read Custom Field](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#read-custom-fields) GraphQL API to read Custom Fields Definitions. The output would list the custom fields that’s being set up in the company along with data types and the entities it’s for.

#### Use Case 2: Create Custom Field Definitions
Use GraphQL API to [Create Custom Field Definitions](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#create-custom-field). 

#### Use Case 3: Create transactions with custom fields

-   Transactions that support custom fields: `Estimate,Invoice,Sales Receipt, Credit Memo, Refund Receipt, Purchase Order, Expense, Bill, VendorCredit`
-   Use Accounting V3 Rest API to create transaction and send CustomField.DefinitionId with legacyIDV2 values obtained from Step 1 above

##### Step 1: Use [Create custom field definitions](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#create-custom-field)  GraphQL API

##### Step 2: [V3 Accounting Rest API Endpoint](https://intuitdeveloper.github.io/intuit-api/docs/use-cases/customfield/#endpoints) to create invoice with custom fields

API Request:

```
{
  "Line": [
    {
    "Amount": 200.00,
    "DetailType": "SalesItemLineDetail",
    "SalesItemLineDetail": {
        "ItemRef": {
        "value": "1",
        "name": "Services"
            }
       }
    }
  ],
  "CustomField": [
    {
        "DefinitionId": "1",
        "StringValue": "my custom value",
    }
    ],
  "CustomerRef": {
    "value": "1"
  }
}

```

API Response:

```
{
    "Invoice": {
        "AllowIPNPayment": false,
        "AllowOnlinePayment": false,
        "AllowOnlineCreditCardPayment": false,
        "AllowOnlineACHPayment": false,
        "domain": "QBO",
        "sparse": false,
        "Id": "801",
        "SyncToken": "0",
        "MetaData": {
            "CreateTime": "2024-05-29T23:22:07-07:00",
            "LastModifiedByRef": {
                "value": "9130347769252966"
            },
            "LastUpdatedTime": "2024-05-29T23:22:07-07:00"
        },
        "CustomField": [
          {
                "DefinitionId": "1",
                "Name": "sales1",
                "Type": "StringType",
                "StringValue": "my custom value"
          }
        ],
        "TxnDate": "2024-05-29",
        "CurrencyRef": {
            "value": "USD",
            "name": "United States Dollar"
        },
        "ExchangeRate": 1,
        "LinkedTxn": [],
        "Line": [
            {
                "Id": "1",
                "LineNum": 1,
                "Amount": 200.00,
                "DetailType": "SalesItemLineDetail",
                "SalesItemLineDetail": {
                    "ItemRef": {
                        "value": "1",
                        "name": "Sales"
                    },
                    "ItemAccountRef": {
                        "value": "1",
                        "name": "Sales"
                    },
                    "TaxCodeRef": {
                        "value": "NON"
                    },
                    "TaxClassificationRef": {
                        "value": "EUC-99990201-V1-00020000"
                    }
                }
            },
            {
                "Amount": 200.00,
                "DetailType": "SubTotalLineDetail",
                "SubTotalLineDetail": {}
            }
        ],
        "TxnTaxDetail": {
            "TxnTaxCodeRef": {
                "value": "8"
            },
            "TotalTax": 0,
            "TaxLine": [
                {
                    "Amount": 0,
                    "DetailType": "TaxLineDetail",
                    "TaxLineDetail": {
                        "TaxRateRef": {
                            "value": "4"
                        },
                        "PercentBased": true,
                        "TaxPercent": 1,
                        "NetAmountTaxable": 0
                    }
                }
          ]
        },
        "CustomerRef": {
            "value": "1",
            "name": "John"
        },
        "ShipFromAddr": {
            "Id": "1274",
            "Line1": "2500 Garcia Avenue",
            "Line2": "Mountain View, CA  94043 US"
        },
        "DueDate": "2024-06-28",
        "TotalAmt": 200.00,
        "HomeTotalAmt": 200.00,
        "ApplyTaxAfterDiscount": false,
        "PrintStatus": "NeedToPrint",
        "EmailStatus": "NotSet",
        "Balance": 200.00,
        "HomeBalance": 200.00,
        "TaxExemptionRef": {}
    },
    "time": "2024-05-29T23:22:06.843-07:00"
}

```

#### Use Case 4: Create entity with custom fields

-   Entities that support custom fields: Customer, Vendor
-   Use Accounting V3 Rest API to create a customer or vendor entity and send CustomFieldDefinitionId with values from Step 1 below

##### Step 1: Use [Create custom field definitions](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#create-custom-field) GraphQL API 


##### Step 2: [V3 Accounting Rest API Endpoint](https://intuitdeveloper.github.io/intuit-api/docs/use-cases/customfield/#endpoints) to create customer with custom fields

Sample Customer with Custom Field definition:
```
{
  "DisplayName": "Customer-01",
  "CustomField": [
    {
      "DefinitionId": "540344",
      "StringValue": "CF-CustomerType"
    }
  ]
}

```

Response:

```
{
    "Customer": {
        "Taxable": false,
        "Job": false,
        "BillWithParent": false,
        "Balance": 0,
        "BalanceWithJobs": 0,
        "CurrencyRef": {
            "value": "USD",
            "name": "United States Dollar"
        },
        "PreferredDeliveryMethod": "None",
        "IsProject": false,
        "domain": "QBO",
        "sparse": false,
        "Id": "4",
        "SyncToken": "0",
        "MetaData": {
            "CreateTime": "2024-06-29T19:12:52-07:00",
            "LastUpdatedTime": "2024-06-29T19:12:52-07:00"
        },
        "CustomField": [
            {
                "DefinitionId": "540344",
                "Name": "cf-05",
                "Type": "StringType",
                "StringValue": "CF-CustomerType"
            }
        ],
        "FullyQualifiedName": "Customer-01",
        "DisplayName": "Customer-01",
        "PrintOnCheckName": "Customer-01",
        "Active": true
    },
    "time": "2024-06-29T19:12:51.593-07:00"
}

```
