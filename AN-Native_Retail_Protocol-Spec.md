![ationetlogo](Content/Images/ATIOnetLogo_250x70.png)
# ATIONet - Native Retail Protocol Specification v1.0

<table>
	<tr>
		<th colspan="2" align="left">
			Document Information
		</th>
	</tr>
	<tr>
		<td>
			File:
		</td>
		<td>
			ATIONet-Native_Retail_Protocol-Spec-v1
		</td>
	</tr>
	<tr>
		<td>
			Doc Version:
		</td>
		<td>
			1.0
		</td>
	</tr>
	<tr>
		<td>
			Release Date:
		</td>
		<td>
			20, March 2015
		</td>
	</tr>
	<tr>
		<td>
			Author:
		</td>
		<td>
			ATIO International LLC
		</td>
	</tr>
</table>

<table>
	<tr>
		<th colspan="3" align="left">
			Change Log
		</th>
	</tr>
	<tr>
		<td>
			Ver.
		</td>
		<td>
			Date
		</td>
		<td>
			Change Summary
		</td>
	</tr>
	<tr valign="top">
		<td>
			<p>1.0</p>
		</td>
		<td>
			<p>20/March/2015</p>
		</td>
		<td>
			<p>Initial version.</p>
		</td>
	</tr>
</table>

## Contents

<!-- MarkdownTOC depth=3 -->

- Overview
	- Introduction
	- Definitions
- ATIOnet Integration Documentation Scope
- Scope
	- Scope Details
	- Supported Transactions
- Data Security
- Message Structure
	- Request Format
	- Response
- Error Handling
- Field Descriptions
- Retail Transaction Request (RREQ) Message Format
- Retail Transaction Response (RRESP) Message Format
- Transaction Data Structures
	- Customer Data
- Reference Tables
	- Transaction Codes
	- Currency Codes
	- Authorization Codes

<!-- /MarkdownTOC -->

## Overview

### Introduction

This specification is intended to document ATIONet’s Native Protocol messaging format and related features required for the systems applying for integration with ATIONet. The following sections provide descriptions of the messages themselves, the expected behaviour for each supported transaction type and a common ground for the functionality of each relevant item.

### Definitions

#### Host
A computer system that is accessed by a user working at a remote location. In this document, Host is always the ATIONet Host.

#### Terminal
An electronic merchant card processing device responsible for transaction capture, display output to the cashier and/or to the cardholder on screen and/or print format.

#### Controller
A client system that can send or receive data to and from ATIONet’s Host. A Controller controls or includes one or more terminal. When there is only one Terminal connected to a Controller, Terminal and Controller are equivalent.

#### RREQ
Transaction Request.

#### RRESP
Transaction Response.

## ATIOnet Integration Documentation Scope

Third-party systems integrate with ATIOnet via a set of APIs (Application Programming Interfaces). Each ATIOnet’s API is described on a separate Protocol Specification. The complete documentation of ATIOnet API’s is comprised of:

#### ATIOnet Native Transactions Protocol Specification: 
Covers financial transactions for transaction capture systems (payment terminals, site controllers and point of sale systems), including sales and refunds.

#### ATIOnet Administrative Transactions Protocol Specification: 
Describes a set of functions complementing the transaction-capture business, for example Batch or Shift Close. These functions enhance the capabilities of the integration but their implementation is not mandatory.

#### ATIOnet Native Interface Protocol Specification: 
Covers system-to-system integration capabilities of ATIOnet, designed to interact with third-party back-end systems, for example downloading transactions data or sending current-accounts movements to ATIOnet. This API is reserved and requires ATIOnet and Subscriber permissions.

#### ATIOnet Maintenance Interface Protocol Specification: 
List a set of functions designed to help in the maintenance and support of a network of capture terminals, for example checking terminal’s status via a Keep-alive message. This API is designed to support ATIOnet’s own line of capture and gateway devices and thus is a reserved protocol.

In addition to one or more protocol specifications, Integration Projects must have an “Integration Scope Document” detailing the feature-set to be implemented by the capture system, which also defines the acceptance criteria for the project.

## Scope

.

### Scope Details

Protocol: ATIONet Native Retail Protocol

Version: Version 1.0

API URI: native.ationet.com/v1/retail

### Supported Transactions

<table>
	<thead>
		<tr valign="center">
			<th rowspan="2" width="250" align="left">
				Name
			</th>
			<th colspan="2" align="center">
				Protocol Ver.
			</th>
			<th rowspan="2" align="left">
				Description
			</th>
		</tr>
		 <tr valign="top">
			  <th align="center">
					Initial
			  </th>
			  <th align="center">
					Change
			  </th>
		 </tr>
	</thead>
	<tbody>
		 <tr valign="top">
			<td>
				<p align="left">Sale Records Upload</p>
			</td>
			<td>
				<p align="center">1.0</p>
			</td>
			<td></td>
			<td>
				<p align="left">Used to post a sale by sale POS data.</p>
			</td>
		 </tr>
	</tbody>
</table>

## Data Security

To validate the source of transactions and data encryption, the ATIONet Native Transaction Protocol relies on a SSL connection between the Site’s Terminal or Site’s Controller and the ATIONet Host. The SSL connection is established for each request/response pair, using a certificate property of ATIONet, meaning that each request must include a system-type user and password on the Header. The user will be matched against the related ATIONet actor for each message.

Users to be used on the Transaction Protocol messaging will be created by authorized users via ATIONet Console, with the role “Controller/Terminal”.

At this time there is no provisioning to distribute or update certificates or thumbprints thru a system interface. This information will be provided at request of the Controller’s vendor during the integration project.

## Message Structure

All Transaction API messages share the same structure, what change from message to message are the Transaction Code, which indicates the actual transaction function, the value fields sent and received, and the HTTP action (POST, GET, REQUEST) which changes depending on the Transaction Code.

Both, requests and responses use a JSON format.

Only one request is accepted on each message.

### Request Format

*Header:*

	Accept-Encoding: gzip

	Authorization: Basic user:password

*Body:*
	{“Fieldname”:”StringValue”,”FieldName”:”StringValue”,”FieldName”:Value}

### Response

*Header:*

	Content-Type: application/json; charset=utf-8

*Body:*
	{“Fieldname”:”StringValue”,”FieldName”:”StringValue”,”FieldName”:Value}

Note: Alphanumeric fields, stated as Type “A/N” in record format tables below show the maximum possible length as the Size, although in JSON-formatted strings they will be represented with trailing spaces trimmed.

## Error Handling

Success/failure exits on the Native Transaction Protocol will be handled via HTTP status codes.

Successful request will get a HTTP 200 and the resulting response.

Transactions intended to post a command, for example Authorizations and Pre-Authorizations will return a single JSON-formatted item with the “Response Code” and “Response Text”. The body of these responses will never be empty.

Failure to process the request will be indicated by an HTTP 400’s range status code. The body will contain a single JSON-formatted item with the “ResponseCode”, “ResponseMessage” and “ResponseError” fields.

Refer to Response Codes Table in the Reference Tables section for a complete list of supported codes.

## Field Descriptions

This section details the purpose and expected behavior on the Controller system for relevant items on the protocol.

#### System Model and System Version
Brand/Model and Firmware/Software version of the client system. Format and content will be assigned for each vendor during the Integration project.

#### Terminal Identification
Terminal ID is a system-wide unique ID for the Controller or Terminal device on the capture side. Terminal ID should be configured on the client system during manual installation. The length of the TID’s code depends on the controller.

#### Device Type Identifier
A single digit field, informed by the Controller system, that identifies the type of capture device. (Manned/Unmanned, Indoor/Outdoor). In case the Controller system doesn’t have the capability to inform this distinction, “4 – Other self-service” should be always informed.

#### Transaction Sequence Number
The Transaction number is a fixed-length integer value from 1 to 999999 and it is assigned and incremented for each transaction sent to the Host, regardless of the result. It must be reset back to 1 every time it reaches the limit.

#### Entry Method
The Entry Method code indicates whether the customer identification was manually typed-in, read from a card swipe or any other automatic identification mechanism.

#### Processing Mode
Indicates whether the Host must apply an alternative process to the request. Regular transactions must inform “1 = Host processing required”

#### Shift Number
Site's business period ID

The Controller application can manage the Shift Number and meaning as needed. It may be day’s shift number, weekly batches, split-batches, etc., although this is a fixed length field, therefore the format must be maintained.

#### Customer Data
Customer data on a RREQ contains extra information gathered from prompts to the Cardholder or Attendant.

#### Authorization Code
The Host will return the Authorization Code on all approved transactions.

## Retail Transaction Request (RREQ) Message Format

<table>
	<thead>
		<tr valign="top">
			<th align="left">
				Field Name
			</th>
			<th align="center">
				Size
			</th>
			<th align="left" width="200">
				Type
			</th>
			<th align="left">
				Condition
			</th>
			<th align="left">
				Descriptions/Field Value(s)
			</th>
		</tr>
	</thead>
	<tbody>
		<tr valign="top">
			<td>
				<p align="left">ApplicationType</p>
			</td>
			<td>
				<p align="left">4</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Always “LTY” Loyalty System</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ProcessingMode</p>
			</td>
			<td>
				<p align="left">1</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Always “0” = Host Capture Only in this version.</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">MessageFormatVersion</p>
			</td>
			<td>
				<p align="left">3</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Current Host Message Version = “1.0”</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">TerminalIdentification</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Terminal Identification</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">DeviceTypeIdentifier</p>
			</td>
			<td>
				<p align="left">1</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">“1” = Indoor Payment Terminal
				<br>“2” = Outdoor Payment Terminal
				<br>“3” = Card Reader in Dispenser
				<br>“4” = Other Self-Service</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">SystemModel</p>
			</td>
			<td>
				<p align="left">10</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Refer to System Model and System Version in Field Description section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">SystemVersion</p>
			</td>
			<td>
				<p align="left">10</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Refer to System Model and System Version in Field Description section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">TransactionSequenceNumber</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">int</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Refer to Transaction Sequence Number in Field Description section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">TransactionCode</p>
			</td>
			<td>
				<p align="left">3</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Refer to Transaction Codes in Reference Tables Section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">SiteCode</p>
			</td>
			<td>
				<p align="left">10</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Site ID. Does not need to match an ATIOnet Site in this version</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">RegisterCode</p>
			</td>
			<td>
				<p align="left">10</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Register or Point of Sale ID</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">TransactionTypeCode</p>
			</td>
			<td>
				<p align="left">3</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">“101” = Sale, taxable document
				<br>“101” = Sale, non-taxable document
				<br>“110” = Automatic receipt taxable
				<br>“111” = Automatic receipt non-taxable
				<br>“120” = Fuelings summary receipt taxable
				<br>“121” = Fuelings summary receipt non-taxable
				<br>“130” = Delivery Note taxable
				<br>“131” = Delivery Note non-taxable
				<br>“200” = Full refund of a taxable document
				<br>“210” = Partial Refund refund of a taxable document
				<br>“201” = Full refund of a non-taxable document
				<br>“211” = Partial Refund refund of a non-taxable document
				<br>“300” = Not-for-sale Internal use
				<br>“310” = Other non-sale</p>			
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">FullDocumentNumber</p>
			</td>
			<td>
				<p align="left">30</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Transaction's receipt complete numeration, including masking characters if any, as mandated by fiscal regulations</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ReferencedFullDocumentNumber</p>
			</td>
			<td>
				<p align="left">30</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Conditional</p>
			</td>
			<td>
				<p align="left">When transaction is a Refund, this field must contain the original sale or delivery note being voided, including masking characters if any, as mandated by fiscal regulations</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">CashierCode</p>
			</td>
			<td>
				<p align="left">10</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">Code of user or employee who ringed the sale</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">CustomerCode</p>
			</td>
			<td>
				<p align="left">10</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">Transaction's customer code matching a valid ATIOnet Company Code. When using ATIOnet Companies as customers, Company Data section is not mandatory.</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">TransactionAmount</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">decimal, signed</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">xxxxxxx.xx </br>Grand Total of the transaction</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">CurrencyCode</p>
			</td>
			<td>
				<p align="left">3</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">Refer to Currency Codes in Reference Tables Section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ShiftNumber</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">Refer to Shift Number in Field Description section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">LocalTransactionDate</p>
			</td>
			<td>
				<p align="left">8</p>
			</td>
			<td>
				<p align="left">int</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Local Transaction Date: yyyymmdd</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">LocalTransactionTime</p>
			</td>
			<td>
				<p align="left">6</p>
			</td>
			<td>
				<p align="left">int</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Local Transaction Time: hhmmss</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ItemsData</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">String < ItemsData ></p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Refer to Item Data  in the Transaction Data Structures section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">CompanyData</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">Dictionary<string, string></p>
			</td>
			<td>
				<p align="left">Conditional</p>
			</td>
			<td>
				<p align="left">Refer to Company Data in the Transaction Data Structures section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">LoyaltyData</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">Dictionary<string, string></p>
			</td>
			<td>
				<p align="left">Conditional</p>
			</td>
			<td>
				<p align="left">Refer to Loyalty Data in the Transaction Data Structures section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">CustomerData</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">Dictionary<string, string></p>
			</td>
			<td>
				<p align="left">Conditional</p>
			</td>
			<td>
				<p align="left">Refer to Customer Data in the Transaction Data Structures section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">DocumentData</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">Dictionary<string, string></p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Refer to Document Data in the Transaction Data Structures section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">TaxesData</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">Dictionary<string, string></p>
			</td>
			<td>
				<p align="left">Conditional</p>
			</td>
			<td>
				<p align="left">Refer to Taxes Data in the Transaction Data Structures section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">MoPData</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">Dictionary<string, string></p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">Refer to Method-of-Payment Data in the Transaction Data Structures section</p>
			</td>
		</tr>
	</tbody>
</table>	

## Retail Transaction Response (RRESP) Message Format

<table>
	<thead>
		<tr valign="top">
			<th align="left">
				Field Name
			</th>
			<th align="left">
				Size
			</th>
			<th align="left">
				Type
			</th>
			<th align="left">
				Condition
			</th>
			<th align="left">
				Descriptions/Field Value(s)
			</th>
		</tr>
	</thead>
	<tbody>
		<tr valign="top">
			<td>
				<p align="left">ApplicationType</p>
			</td>
			<td>
				<p align="left">3</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Echoed from LREQ</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ProcessingMode</p>
			</td>
			<td>
				<p align="left">1</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Echoed from LREQ</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">MessageFormatVersion</p>
			</td>
			<td>
				<p align="left">3</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Echoed from LREQ</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">TerminalIdentification</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Echoed from LREQ</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">DeviceTypeIdentifier</p>
			</td>
			<td>
				<p align="left">1</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Echoed from LREQ</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">TransactionCode</p>
			</td>
			<td>
				<p align="left">3</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Refer to Transaction Codes in Reference Tables Section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">TransactionSequenceNumber</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">int</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Echoed from LREQ</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">AuthorizationCode</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Conditional</p>
			</td>
			<td>
				<p align="left">Refer to Authorization Code in Field Description section</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ResponseCode</p>
			</td>
			<td>
				<p align="left">5</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">“0” = Authorized, !”0” = Not Authorized</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ResponseText</p>
			</td>
			<td>
				<p align="left">20</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Message from the Network</p>
			</td>
		</tr>
	</tbody>
</table>	

## Transaction Data Structures

<table>
	<tr>
		<td>lalala</td>
	</tr>
</table>





<table>
	<thead>
		<tr valign="top">
			<td align="left">
				Field Name
			</td>
			<td align="left">
				Size
			</td>
			<td align="left">
				Type
			</td>
			<td align="left">
				Condition
			</td>
			<td align="left">
				Descriptions/Field Value(s)
			</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td colspan="5" align="left">
				Product List section (one per product in transaction)
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ServiceCode</p>
			</td>
			<td>
				<p align="left">1</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>	
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Always "0" in this version</p>
			</td>
		</tr>
		<tr valign="top">
			<td>    
				<p align="left">ItemCode</p>
			</td>
			<td>	
				<p align="left">14</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">Code used to ring the item (be it a PLU or a Barcode)</p>
			</td>
		</tr>
		<tr valign="top">
			<td>    
				<p align="left">SKU</p>
			</td>
			<td>	
				<p align="left">14</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">SKU related to the item sold</p>
			</td>
		</tr>
		<tr valign="top">
			<td>    
				<p align="left">ItemDesc</p>
			</td>
			<td>	
				<p align="left">40</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">Item description as shown on the ticket, truncated to 40 chars if longer</p>
			</td>
		</tr>
		<tr valign="top">		
			<td>	
				<p align="left">RegularPrice</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">decimal</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">xxxx.xxx</p>
			</td>
		</tr>
		<tr valign="top">		
			<td>	
				<p align="left">ActualSalePrice</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">decimal</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">xxxx.xxx</p>
			</td>
		</tr>
		<tr valign="top">		
			<td>	
				<p align="left">ActualSalePrice</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">decimal</p>	
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">xxxx.xxx</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ProductNetAmount</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">decimal</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">xxxxxxx.xx</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ProductTaxes</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">Dictionary<string, decimal></p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">”[Tax Description]”, [Tax Value]</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ProductAmount</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">decimal</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">xxxxxxx.xx</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">ProductQuantity</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">decimal</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">xxxxxxx.xx</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">UnitCode</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Optional</p>
			</td>
			<td>
				<p align="left">Refer to Measurement Unit Codes in Reference Tables Section</p>
			</td>
		</tr>
		<tr>
			<td colspan="5" align="left">
				Method-of-Payment List section (one per MoP in transaction)
			</td>
		</tr>
		<tr valign="top">
			<td>    
				<p align="left">MoPCode</p>
			</td>
			<td>	
				<p align="left">4</p>
			</td>
			<td>
				<p align="left">string</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">“0”-“9999”</p>
			</td>
		</tr>
		<tr valign="top">		
			<td>	
				<p align="left">Amount</p>
			</td>
			<td>
				<p align="left">Var</p>
			</td>
			<td>
				<p align="left">decimal</p>
			</td>
			<td>
				<p align="left">Required</p>
			</td>
			<td>
				<p align="left">xxxx.xx</p>
			</td>
		</tr>
	</tbody>
</table>	

### Customer Data

*Prompt elements*
<table>
	<thead>
		<tr valign="top">
			<th align="left">
				Field Name
			</th>
		</tr>
	</thead>
	<tbody>
		<tr valign="top">
			<td>
				<p align="left">PromptOdometer</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">PromptMiscellaneous</p>
			</td>
		</tr>
	</tbody>
</table>	

## Reference Tables

This section brings together the code tables and reference values used in messaging.

### Transaction Codes

<table>
	<thead>
		<tr valign="top">
			<th align="left">
				Code
			</th>
			<th align="left">
				Message
			</th>
			<th align="left">
				Description
			</th>
		</tr>
	</thead>
	<tbody>
		<tr valign="top">
			<td>
				<p align="left">“610”</p>
			</td>
			<td>
				<p align="left">RREQ</p>
			</td>
			<td>
				<p align="left">Sale Records Upload REQ</p>
			</td>
		</tr>
		<tr valign="top">
			<td>
				<p align="left">“611”</p>
			</td>
			<td>
				<p align="left">RRESP</p>
			</td>
			<td>
				<p align="left">Sale Records Upload RESP</p>
			</td>
		</tr>
	</tbody>
</table>

### Currency Codes
 
Refer to ISO 4217 Currency Codes standard (<http://en.wikipedia.org/wiki/ISO_4217>)

### Authorization Codes

Refer to AN-Native_Response_Codes-Spec document for a complete list of ATIOnet Host Response Codes and Texts.

