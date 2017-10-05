# Contoso Clinic Demo Application 

Sample application with database that showcases Auditing and Threat Detection of Azure SQL DB (V12). 
I have modified the original sample in order to limit the demo to Auditing and Threat Detection of Azure SQL DB. For Original repo Check
[azure-sql-security-sample ](https://github.com/Microsoft/azure-sql-security-sample)

## About this sample
- **Applies to:**  Azure SQL Database, Azure Web App Service
- **Programming Language:** .NET C#, T-SQL
- **Authors:** Daniel Rediske [daredis-msft]

## Contents
1. [Prerequisites](#prerequisites) 
2. [Estimated Cost of Deployed Resources](#estimated-cost-of-deployed-resources)
3. [Setup](#setup) 
	* Deploy to Azure
4. [Azure SQL Security Features](#azure-sql-security-features) 
	* Auditing & Threat Detection
5.  [Application Notes/Disclaimer](#application-notes)


## Prerequisites
+ Azure Subscription with resource creation permissions

## Estimated Cost of Deployed Resources
The following table is an estimation of the cost of deploying the Demo as of 5/9/2016. 

 Resource | Cost/Month | Cost/Hr 
 --- | --- | ---  
[S1 SQL Database ](https://azure.microsoft.com/en-us/pricing/details/sql-database/) |  $30  | $0.04
[B1 App Service Plan](https://azure.microsoft.com/en-us/pricing/details/app-service/) | $55.80  | $0.075
[Storage Plan](https://azure.microsoft.com/en-us/pricing/details/storage/) | ~$0 | $0.0036/transaction
**Monthly Total** | $85.80/mo | ~$0.115/hr

## Setup
### Deploy to Azure 
Click the Deploy to Azure Button and fill out the fields to deploy the demo to your Azure Subscription.

Note on Passwords: Please use only characters and numbers [a-z A-Z 0-9]. Because of certain implementation decisions made in development of this demo, other characters *may cause deployment issues*.  

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/)

## Azure SQL Security Features 
### Auditing & Threat Detection
#### Set up and Test Auditing & Threat Detection 

+ Auditing and Threat Detection should have been turned on during deployment 
+ You can verify this in the [Azure Portal](https://portal.azure.com) by viewing the Database Settings (under **Auditing & Threat Detection**)
	- Auditing should be 'ON'
	- Threat Detection should be 'ON'
	- For shared accounts, unselect the **Email Service and Co-Administrators** box and place your own email address in the box. 
		* This will avoid alarming your Service Admins and Subscription Co-Admins with an Alert Email, should your account have them. 
+ Execute a SQL injection to show Threat Detection on the /patients page
	- We left the box at the top of the page vulnerable on purpose, but you ought to take precautions to prevent attacks on your apps. 
	- Here's a simple injection that just reorders the results. (Simply copy the following code and paste it into the textbox at the top of the patients page)
	```SQL 
	' ORDER BY SSN -- 
	```
	- Worth saying again: **You _must_ protect against SQL Injection in your app code.** [Learn more about SQL Injection and protecting against it from OWASP.](https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet) 
	- Note: This injection will cause an error instead of reordering results IF Always Encrypted is enabled. 
+ Check your inbox for a Threat Detection email 
	- From *Microsoft Azure Security Alerts* <security-alerts-noreply@mail.windowsazure.com> 

#### How did that work? 
Threat Detection is designed to detect suspicious database activity- which may indicate malicious access, a breach, or an exploit attempt on the Database. This is designed around machine learning algorithms that look for anomalous database activities over historical data and normal behavior of databases. Because SQL injection is a leading exploit vector for unauthorized access to data, it is flagged by Threat Detection as abnormal behavior. 


## Application Notes
The code included in this sample is only intended to provide a simple demo platform for users to enable and gain experience with Azure SQL Database (V12) security features; the demo web app is not intended to hold sensitive data and should not be used as a reference for applications that use or store sensitive data.Please take adequate steps to securely develop your application and store your data.  
