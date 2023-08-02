![](/images/ahlsbanner.png)

# A-HLS Epic Patient Search & Create Documentation 

## **Overview**

The Epic Patient Search and Create accelerator enables customers to search for a patient in their Epic system from the Salesforce Health Cloud console, and if the patient does not yet exist in Epic, create that Epic patient record from Salesforce. A user would enter the patient’s information to view resulting matches. The user has a choice of searching by Patient ID (SSN, FHIRID, or MRN), Name and Birthdate, or Name, Legal Sex, and Phone/Email. The user can then open the patient's record in Salesforce to view their detail. 

If the patient does not yet exist in Epic then the search results will return no matches and ask the user if they would like to create the patient. The user would then fill out the required fields as outlined in Epic's Patient.Create (R4) public FHIR API. 

The accelerator is compatible with both MuleSoft, MuleSoft Direct, or via a direct connection via FHIR APIs with Epic. After a few configuration steps, an organization is ready to start using the Patient Search & Create component in their workflows.

It is assumed that an organization has enabled their Patient.Create (R4) and Patient.Search (R4) FHIR API in their Epic system which is what supports the patient search capability.

![](/images/psimage1.png)
![](/images/psimage2.png)
![](/images/psimage8.png)

## **Business Objective**

This accelerator aims to reduce the cost of IT by providing a free, lightweight, integration point with a customer's Epic instance. 
It will also improve a user's experience by providing a single user interface to search for, find, create, and open a patient's record without needing to switch systems.

## **Business Value and Benefits**

* Increased Efficiency
* Improved User Experience
* Decreased IT Costs
* Decreased Time to Value

* * *

## **Industry Focus and Workflow**

### **Primary Industry:**

* Provider
* Payer

### **Primary User Persona:**

* Call Center Representatives
* Care Coordinators
* Patient Services Representatives

### **User Workflow:**

* Choose a search method
* Enter the patient’s information
* Click Search 
* Click on the correct patient to open their Salesforce record
* If no patient result is found, click on Create Patient
* Enter the patient's information and click Create
* The patient's Salesforce record will automatically open and be linked with the patient's Epic unique identifier

* * *

## **Package Includes:**

### **OmniScript (1)**

* EHR/EpicPatientSearch

### **Integration Procedures (3)**

* EHR/AuthAndSearch
* EpicFHIR/PatientSearch
* Patient/SearchCreate
* EpicFHIR/PatientCreate

### DataRaptors (4)

* EpicPatientSearchTransform
* EpicPatientSearchTransform2
* DRFindPersonAccount
* DRCreatePersonAccount

### Images (Logos) (4) - found in Images folder in this repo

* Epic logo transparent.png
* nurse.png
* clinical_fe.png
* phone.png

### FlexCards (1)

* EpicFHIRPatientSearchResults

### **Custom Apex Class (1)**

* ClientCredentialsJWT.cls

## Configuration Requirements

### 1. Pre-Install Configuration Steps:

#### EHR Pre-Installation Steps:

* Confirm that your endpoint is configured such that the following Search and Create APIs are active - for full reference, please refer to the [fhir.epic.com](http://fhir.epic.com/) API documentation.
        1. api/FHIR/R4/Patient



#### **Salesforce Pre-Installation Steps:**

* Ensure your Salesforce Health Cloud org has Core OmniStudio installed. 
        1. To verify installation, please navigate to Setup > Installed Packages > OmniStudio.
    2. Enable Identity Provider according to these steps: https://help.salesforce.com/s/articleView?id=sf.identity_provider_enable.htm&type=5
![](/images/psimage3.png)
### 2. Install the Accelerator 

* Follow the download steps in the **"Download Now"** flow presented on the HLS Accelerators website for this Accelerator which downloads the following GitHub repository on your machine: https://hlsaccelerators.developer.salesforce.com/s/bundle/a9E5f000000PLAeEAO/epic-patient-search
* Unzip the resulting .zip file which is downloaded to your machine. 
* Open the **“OmniStudio”** folder
	* Open the folder titled "OmniStudio Version".
	* Install the DataPack into your org. 
		* In Salesforce, click on **App Launcher** → Search for **"OmniStudio DataPacks"** and click on it.
		* Click on **"Installed"** > Import > From File
		* When the window opens, select the .json file identified in the previous step. Click **"Open"** then click 'Next' 3 times.
		* When prompted, click **"Activate Now"**.

### 3. Integration Configuration

There are 3 different ways that organizations may configure the integration for the accelerator. Click on the method below that supports your organization's integration architecture:

* [MuleSoft Direct](#mulesoft-direct)
* [MuleSoft Anypoint Platform](#mulesoft-anypoint-platform)
* [Epic Direct Connection](#Epic-Direct-Connection)


## MuleSoft Direct


*Available starting June 5, 2023*

Required SKUs:

-   Health Cloud
-   Anypoint Starter or Base

### 1. Complete MuleSoft Direct Configuration:

-   Complete the Setup steps for the  [**Integrations Setup for Industry Integration Solutions**](https://help.salesforce.com/s/articleView?language=en_US&id=sf.industry_integration_solutions.htm&type=5)
    -   In the  **Generic FHIR Client Setup**, enter the following information for the Epic EHR credentials
        -   **Authentication Protocol**: JSON Web Token
        -   **Base URL**: your Epic FHIR server’s base URL
        -   **Token URL**: your Epic FHIR server’s authentication URL
        -   **Client ID**: 43b0500b-ea80-41d4-be83-21230c837c15
        -   **Private Key**  - import the Private Key which is included in the Accelerator download .zip folder
        -   **Non-PRD Test Information**:
            -   **Endpoint**:  [https://fhir.epic.com/interconnect-fhir-oauth/oauth2/token](https://fhir.epic.com/interconnect-fhir-oauth/oauth2/token)
            -   **Client ID**: fe0e3375-fffa-469b-8338-d5fe08f7d3b1
            -   Refer to the  **Epic FHIR App**: Salesforce Health Cloud - Clinical Summary

<h3>2. Configure the Accelerator for MuleSoft Direct</h3>

Update the **EpicFHIRPatientSearch** Integration Procedure to use the MuleSoft Direct connection instead of Epic FHIR APIs directly:

* App Launcher > Integration Procedures > EpicFHIRPatientSearch
	* Create a **New Version** of the Integration Procedures
	* For the **HTTP Action** element, make the following changes:
		* In the **Path** field, remove the “/FHIR/R4” portion of the path such that the Path = /api/Patient (for example)
		* Replace **Epic_Auth_JWT** with the name of the Named Credential resulting from the MuleSoft Direct setup above (e.g. **Health_generic_system_app**)
	* **Activate** your new Integration Procedure version

## MuleSoft Anypoint Platform

<h3>1. Configure MuleSoft</h3>

* Follow the [**Epic Administration System API Setup Guide**](https://anypoint.mulesoft.com/exchange/org.mule.examples/hc-accelerator-epic-us-core-administration-sys-api/minor/1.0/pages/edh-nhj/Setup%20Guide/)
* Create a new **Remote Site** (Setup > Remote Site Settings > New Remote Site) with the URL of the newly-created **MuleSoft app**.
* Create a **Named Credential** for your **MuleSoft app**:
	* Refer to **steps 5 - 8** in this article for the detailed setup: [https://sfdc247.com/2021/11/step-by-step-guide-on-integrating-mulesoft-and-salesforce.html](https://sfdc247.com/2021/11/step-by-step-guide-on-integrating-mulesoft-and-salesforce.html)
	* You do **NOT** need to complete the **External Services** configuration in the article above

<h3>2. Configure the Accelerator for MuleSoft</h3>

* App Launcher > Integration Procedures > EpicFHIRPatientSearch
	* Create a **New Version** of the Integration Procedure
	* For the **HTTP Action** element, make the following changes:
		* In the **Path** field, set it to the URL of the newly-created **MuleSoft app**.
		* In the Named Credential field, Replace **Epic_Auth_JWT** with the newly created **Named Credential** for your **MuleSoft app**

## Epic Direct Connection


Required SKUs:
* Health Cloud

<h3>1. EHR Pre-Configuration Steps:</h3>

* Ensure your EHR system's network is configured to accept traffic from your Health Cloud org.
* Install the Epic on FHIR App called “**Salesforce Health Cloud - Clinical Summary**” into your Epic organization. 
	* **Client ID:** 43b0500b-ea80-41d4-be83-21230c837c15

<h3>2. Salesforce Pre-Installation Steps:</h3>

<h4>Create a new Custom Metadata Type </h4>

* Setup > **Custom Metadata Types**
	* New Metadata Type - **must be named** as follows:  **ClientCredentialsJWT**
        * Add the following **Custom fields**. All fields should be **“Text”** fields with a length of **255** characters.
            * aud
            * callback uri
            * cert
            * iss
            * jti
            * sub
![](/images/fcimage2.png)
<h4>Install custom Apex Class</h4>

* Open the **“salesforce-sfdx”** folder in the .zip file that you downloaded for the Accelerator. 
* Use IDX or sfdx to install the files under the “salesforce-sfdx” folder. 
	* Click here to access the [**IDX workbench**](https://workbench.developerforce.com/login.php)
	* For more information regarding IDX, please review this [**Trailhead**](https://trailhead.salesforce.com/content/learn/modules/omnistudio-developer-tools)
* Alternatively, you may create a new Apex Class manually by performing the following:
	* Setup > Apex Classes > New
	* Copy the entire code of the Apex Class in the salesforce-sfdx folder and paste in the new Apex Class editor and Save.
* Import the keystore **FHIRDEMOKEYSTORE.jks** from .zip file. 
	* Setup > **Certificates and Key Management** > Import from **Keystore**
	* **Password**: salesforce1


<h4>Create a new Authentication Provider</h4>

* Setup > Auth Providers
    * Create a New Authentication Provider
        * Provider Type: **ClientCredentialJWT**
        * Name: Epic_JWT_Auth
        * iss: 43b0500b-ea80-41d4-be83-21230c837c15
        * sub: 43b0500b-ea80-41d4-be83-21230c837c15
        * aud: set this to the API endpoint for authentication - either the MuleSoft API or Epic FHIR API - e.g., https://fhir.epic.com/interconnect-fhir-oauth/oauth2/token
        * jti: salesforce
        * cert: fhirdemo_cert
        * callback uri: https://YOURDOMAIN/services/authcallback/Epic_JWT_Auth
        * Execute Registration As: your system administrator User
![](/images/fcimage3.png)
<h4>Configure Remote Site Settings</h4>

* Setup > Remote Site Settings > New
    * Give the Remote Site a name and paste the domain of the API endpoint into the URL field.
    * Click Save.

<h4>Create a new Named Credential</h4>

* Setup > **Named Credential** > **New Legacy**
	* **Name**: **Must be** the following: **Epic Auth JWT**
	* **URL**: the URL of the endpoint you are going to connect to. For example, https://fhir.epic.com/interconnect-fhir-oauth/ 
	* **Identity Type**: Named Principal
	* **Authentication Protocol**: OAuth 2.0
	* **Authentication Provider**: the name of your Authentication Provider above
	* Check the **“Run Authentication Flow on Save”** box

![](/images/fcimage4.png)

![](/images/psimage5.png)

## Configure Accelerator

After completing your respective integration configuration, please complete the following steps:

### 1.  FlexCard Configuration
* Click on **App Launcher →** Search for “**FlexCards**”
    * Open the **EpicPatientSearchResults** Flexcard
    * Deactivate and re-Activate the FlexCard
    * Choose the following **Publish Options**:
        * App Page
        * Home Page
        * Record Page

![](/images/psimage6.png)

### 2. Activate Integration Procedures
* Click on **App Launcher** >> Search for "Integration Procedures"
    * Open the **EHR/AuthAndSearch** Integration Procedure. 
    * Click **"Activate"** at the bottom of the Procedure Configuration screen if the integration procedure is not already active.
    * Close the Integration Procedure.
    * Repeat the steps above for the Integration Procedures titled **"EpicFHIR/PatientSearch"**, **"EpicFHIR/PatientCreate"** and **"Patient/SearchCreate"**.

### 3. Configure OmniScript 
* Click on **App Launcher** → Search for “OmniScripts”
    * Navigate to the recently installed OmniScript in the list view - **EHR/EpicPatientSearch**
        * Deactivate the OmniScript
        * If the images for the 3 search options do not appear, do the following:
	        * Open up the **Step 1** Element and click on the selection boxes which hold the three search options. 
	        * On the **Properties** pane, click on the hyperlink name of one of the option titles, and re-upload the image using the files downloaded in the git repo
          	* Click the Save button.
        * **Activate** the OmniScript. Be sure to activate the FlexCard in the previous step before re-activating the OmniScript.
        * Click on the drop-down arrow next to "Active" and select **"Deploy Standard Run Time Compatible LWC"**.
    * For more information regarding activating Omniscripts, please see this article: https://help.salesforce.com/s/articleView?id=sf.os_activating_omniscripts.htm&type=5

### 4. Configure DataRaptor
* Click on **App Launcher** → Search for “DataRaptors” 
    * Open the **DRCreatePersonAccount** DataRaptor
    * Navigate to the **Formulas** tab
    * Replace the value in the left hand pane to your org’s **Record Type ID** value for the **Person Account Record Type** in your Salesforce org. 
	    * To find this value: Setup > Object Manager > Person Account > Record Types > click on the hyperlink name for the Person Account record type you use. 
	    * In the URL in your browser, copy the alphanumerical string value. This is the record type ID.
![](/images/psimage7.png)

### 5. Add OmniScript to Lightning Page
* Add the installed OmniScript to the App Home Page of your choosing. 
    * Refer to this article for more information regarding adding [**OmniScripts to a Lightning Page**](https://help.salesforce.com/s/articleView?id=sf.os_add_a_standard_omniscript_component_to_a_lighting_page_20263.htm&type=5)



## **Assumptions**

1. A customer has licenses for Health Cloud with Core OmniStudio installed. These solutions have all been installed and are functional. 
2. A customer is assuming Salesforce Lightning Experience — not Classic.
3. Data Model elements that are part of Health Cloud with OmniStudio are all available.
4. The Accelerator uses the Lightning Design System standards and look. Customers may want to apply their own branding which can be achieved.
5. A customer has an administrator/developer who is familiar with IDX and OmniStudio.
6. A customer is live with Epic EHR and has their FHIR R4 server active.
7. Epic is a trademark of Epic Systems Corporation.
8. This tool is intended to provide capabilities for Customers to configure and optimize use of their implemented Salesforce Services. Customers should ensure that their use of this tool meets their own use case needs and compliance requirements (including any applicable healthcare and privacy laws, rules, and regulations).
9. Customers that use software, APIs, or other products from Epic may be subject to additional terms and conditions, including, without limitation, Epic's open.epic API Subscription Agreement terms.



## **Revision History**

* **Revision Short Description (Month Day, Year)**
    * January 4, 2023 - Initial draft
    * January 23, 2023 - Updated documentation
    * April 7, 2023 - Updated documentation
    * May 4, 2023 - updated with configuration steps for MuleSoft customers
    * May 23, 2023 - updated documentation for easier navigation and MuleSoft Direct instructions
    * August 2, 2023 - updated with the new Create capabilities






