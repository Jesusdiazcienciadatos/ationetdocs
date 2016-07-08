![ationetlogo](Content/Images/ATIOnetLogo_250x70.png) 
# ATIONet .NET SDK Development Guide
> **About:** This document describes and explains how to consume the .NET SDK for all Ationet APIs.	

</br>

<table>
	<thead>
		<tr>
			<td colspan="2" class="tablehead">Document Information</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td width="20%" class="rowhead" align="right">File:</td>
			<td>AN-SDK-Specs</td>
		</tr>
		<tr>
			<td align="right">Doc. Version:</td>
			<td>1.0</td>
		</tr>
		<tr>
			<td align="right">Release Date:</td>
			<td>02, September 2014</td>
		</tr>
		<tr>
			<td align="right">Author:</td>
			<td>ATIO International LLC</td>
		</tr>
	</tbody>
</table>

<table>
     <thead>
          <tr>
          	<td colspan="3">Change Log</td>
          </tr>
     </thead>
     <tbody>
        <tr>
          	<td>Ver.</td>
            <td>Date</td>
            <td>Change summary</td>
        </tr>
        <!-- Insert a table row like this for each version -->
        <tr>
          	<td>1.0</td>
          	<td>02/Sept/2014</td>
          	<td>Initial version</td>
        </tr>
        <!-- End of version table row -->
     </tbody>
</table>
</br>

<!--###Table of Content -->

## Contents

<!-- MarkdownTOC depth=3 -->

- [Introduction](#introduction)
- [Download / Installation](#download-installation)
- [Operation Types](#operation-types)
  -  [Auth](#auth)
  -  [FMS](#fms)
  -  [Interface](#interface)
  -  [Loyalty](#loyalty)
  -  [Retail](#retail)
- [Consuming the SDK](#consuming-the-sdk)
  -  [Operation Types methods](#operation-types-methods)
	  -  [Auth Methods](#auth-methods)
	  -  [FMS Methods](#fms-methods)
	  -  [Interface Methods](#interface-methods)
	  -  [Loyalty Methods](#loyalty-methods)
	  -  [Retail Methods](#retail-methods)

<!-- /MarkdownTOC -->

## Introduction
The ATIONet SDK (Software development Kit) helps any developer that wants to interact with ATIONet Platform. If you are integrating an existing software or want to extract data out of ATIONet, the SDK will make this much easier, taking care of all the HTTP communication, error handling and retry policies.
ATIONet SDK can be consumed from any .NET language.

## Download / Installation
ATIONet SDK is hosted in [www.nuget.org](http://www.nuget.org). NuGet is the package manager for the Microsoft development platform including .NET. The NuGet client tools provide the ability to produce and consume packages. The NuGet Gallery is the central package repository used by all package authors and consumers.

## Operation Types
ATIONet SDK is able to handle most of the ATIONet modules (Operation Types). This operations are very specific to the responsibility they have in the platform, like for example *Authorization*, *FMS (Fuel Management Systems)*, *Loyalty*, etc. Each of this Operation Type, has a corresponding  module in the SDK. the complete list of *Operations Types* are listed below.

### Auth
The Auth Operation Type is the one in charge of interacting with the authorization engine. The authorization engine is the component that receives authorizations requests and decides if the transaction is approved or not. This SDK Operation Type should be use if you are building a POS or a terminal that will be sending authorization requests to ATIONet. 

### FMS
The FMS Operation Type is the one in charge of interacting with the FMS module. ATIONet supports receiving *Inventory* and *Deliveries* transactions through this interface.

### Interface
The Interface Operation Type was design to be used by 3rd party software that need to get information from ATIONet. With this Operation Type you can download transactions (fleet, loyalty, retail, rejected, approved and exceptions), current account movements, current account balances and loyalty current account.

### Loyalty
The Loyalty Operation Type is the one in charge of interacting with the Loyalty module. Using this Operation Type you will be able to send Loyalty transactions to ATIONet among other features.

### Retail
The Retail Operation Type is the one in charge of interacting with the Retail module. Using this Operation Type you will be able to send Retail information (transactions, batch closes, etc) to ATIONet among other features.


## Consuming the SDK
For this example we will use Visual Studio 2013 and we will create a Console Application.

1.  Inside Visual Studio create a new Console Application
![ationetlogo](Content/Includes/AN-SDK-Reference/01.png)
1.  Once the project is created, open the *Solution Explorer*, right click the project and select *Manage NuGet Packages*. This will open a pop up.
![ationetlogo](Content/Includes/AN-SDK-Reference/02.png)

3.  Inside the NuGet pop up, in the left part select *Online*, then in the right top corner type *ationet*. This will bring the list of packages found with this criteria. In this case only 1, **Ationet SDK**. Click *Install*, this will install the Ationet SDK packages and dependencies. Please accept terms and conditions for dependencies packages. 
![ationetlogo](Content/Includes/AN-SDK-Reference/03.png)

1.  In the sample below you will see how to download transactions. Open the Program.cs and type the following code:

```javascript


var client = new Ationet.Sdk.Interface.InterfaceOperations("https://native.ationet.com/", "[YOUR-USERNAME]", "[YOUR-PASSWORD]");
var transactions = client.GetTransactions("XYZ", "", "", DateTime.Now.AddDays(-1));

foreach (var tran in transactions.Content)
{
    Console.WriteLine("{0} - {1} - {2}", tran.AuthorizationCode, tran.TerminalCode, tran.ProductAmountDispensed);
}


```

First, you need an instance of the *InterfaceOperations* class. The constructor of this class requires 3 parameters, the url, username and password.
Once you have an instance of the *InterfaceOperations* class you can start calling the different methods. Find below the list of all the methods by *Operation Type*:


## Operation Types methods
Each Operation Type class has multiple methods to perform specific operation against ATIONet. Each of this methods has a set of parameters, some of them to determine behaviour and others to filter data.

### Auth Methods
<table>
     <thead>
        <tr> 
			<td>Method</td>
			<td>Parameters</td>
			<td>Description</td>
		</tr>
		
     </thead>
     <tbody>
        <tr>
          	<td>SendConfirmation</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendConfirmationAsync</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendPreAuthorization</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendPreAuthorizationAsync</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendSale</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendSaleAsync</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
	</tbody>
</table>

### FMS Methods
<table>
     <thead>
        <tr> 
			<td>Method</td>
			<td>Parameters</td>
			<td>Description</td>
		</tr>
		
     </thead>
     <tbody>
        <tr>
          	<td>UploadNativeDeliveries</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>UploadNativeDelivery</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>UploadNativeInventories</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>UploadNativeInventory</td>
            <td></td>
            <td>loren impsum</td>
        </tr>

	</tbody>
</table>

### Interface Methods
<table>
     <thead>
        <tr> 
			<td>Method</td>
			<td>Parameters</td>
			<td>Description</td>
		</tr>
		
     </thead>
     <tbody>
        <tr>
          	<td>BalanceTransferContractToSubAccount</td>
            <td></td>
            <td>This method transfers money from the contract to a sub account</td>
        </tr>
        <tr>
          	<td>BalanceTransferSubAccountToContract</td>
          	<td></td>
          	<td>This method transfers money from the sub account to the contract</td>
        </tr>
        <tr>
          	<td>BalanceTransferSubAccountToSubAccount</td>
            <td></td>
            <td>This method transfers money between sub accounts</td>
        </tr>
        <tr>
          	<td>BalanceTransferToContract</td>
            <td></td>
            <td>This method transfers (deposit) money to a contract</td>
        </tr>
        <tr>
          	<td>BalanceTransferToSubAccount</td>
            <td></td>
            <td>This method transfers (deposit) money to a sub account</td>
        </tr>
        <tr>
          	<td>BalanceWithdrawFromContract</td>
            <td></td>
            <td>This method removes (Withdraw) money from a contract</td>
        </tr>
        <tr>
          	<td>BalanceWithdrawFromSubAccount</td>
            <td></td>
            <td>This method removes (Withdraw) money from a sub account</td>
        </tr>
        <tr>
          	<td>ContractBalanceDownload</td>
            <td></td>
            <td>This method downloads a contracts balances list</td>
        </tr>
        <tr>
          	<td>GetDeliveries</td>
            <td></td>
            <td>This method downloads a deliveries list</td>
        </tr>
        <tr>
          	<td>GetExceptions</td>
            <td></td>
            <td>This method downloads a transaction exceptions list</td>
        </tr>
        <tr>
          	<td>GetInventories</td>
            <td></td>
            <td>This method downloads an inventories list</td>
        </tr>
        <tr>
          	<td>GetMovements</td>
            <td></td>
            <td>This method downloads current account movements</td>
        </tr>
        <tr>
          	<td>GetRetailBatchCloses</td>
            <td></td>
            <td>This method downloads retail batch closes</td>
        </tr>
        <tr>
          	<td>GetRetailTransactions</td>
            <td></td>
            <td>This method downloads retail transactions</td>
        </tr>
        <tr>
          	<td>GetStatements</td>
            <td></td>
            <td>This method downloads the statements</td>
        </tr>
        <tr>
          	<td>GetTransactions</td>
            <td></td>
            <td>This method downloads fleet transactions</td>
        </tr>
        <tr>
          	<td>GetUncontrolledTransactions</td>
            <td></td>
            <td>This method downloads uncontrolled transactions</td>
        </tr>
        <tr>
          	<td>InsertFastTrackOrder</td>
            <td></td>
            <td>This method inserts fast tracks (trip orders)</td>
        </tr>
        <tr>
          	<td>SubAccountBalanceDownload</td>
            <td></td>
            <td>This method downloads the balance of the sub accounts</td>
        </tr>
     </tbody>
</table>

### Loyalty Methods
<table>
     <thead>
        <tr> 
			<td>Method</td>
			<td>Parameters</td>
			<td>Description</td>
		</tr>
		
     </thead>
     <tbody>
        <tr>
          	<td>SendLoyaltyAccumulation</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendLoyaltyAccumulationAsync</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendLoyaltyBalanceInquiry</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendLoyaltyBalanceInquiryAsync</td>
            <td></td>
            <td>loren impsum</td>
        </tr>

	</tbody>
</table>

### Retail Methods
<table>
     <thead>
        <tr> 
			<td>Method</td>
			<td>Parameters</td>
			<td>Description</td>
		</tr>
		
     </thead>
     <tbody>
        <tr>
          	<td>SendRetailBatchClose</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendRetailBatchCloseAsync</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendSaleRecordsUpload</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
        <tr>
          	<td>SendSaleRecordsUploadAsync</td>
            <td></td>
            <td>loren impsum</td>
        </tr>
	</tbody>
</table>