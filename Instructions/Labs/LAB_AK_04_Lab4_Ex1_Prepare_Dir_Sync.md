# Module 4 - Lab 4 - Exercise 1 - Preparing for directory synchronization

As in the previous lab exercises you will take on the role of Holly Dickson, Adatum Corporation’s Enterprise Administrator. Adatum has recently subscribed to Microsoft 365, and you have been tasked with deploying the application in Adatum’s virtualized lab environment. In this lab, you will perform the tasks necessary to manage your Microsoft 365 identity environment using both the Microsoft 365 admin center and Windows PowerShell. 

During this exercise you will set up and manage Azure AD Connect. You will create on-premises users and validate the sync process so that their identities are moved to the cloud. Some of the steps may feel familiar from previous exercises; however, in this case they are needed to validate the synchronization process.

### Task 1: Configure your UPN suffix

In Active Directory, the default User Principal Name (UPN) suffix is the DNS name of the domain where the user account was created. The Azure AD Connect wizard uses the UserPrincipalName attribute, or it lets you specify the on-premises attribute (in a custom installation) to be used as the user principal name in Azure AD. This is the value that is used for signing into Azure AD. 

If you recall, your VM environment was created by your lab hosting provider with an on-premises domain titled **adatum.com**. This domain included a number of on-premises user accounts, such as Holly Dickson, Laura Atkins, and so on. Then in the first lab in this course, you finished provisioning a custom domain which your lab hosting provider created for Adatum titled **xxxUPNxxx.xxxCustomDomainxxx.xxx** (where xxxUPNxxx was the unique UPN name assigned to your tenant, and xxxCustomDomainxxx.xxx was the name your lab hosting provider assigned to the custom domain).

In this task, you will use PowerShell to change the user principal name of the domain for the entire Adatum Corporation by replacing the originally established **adatum.com** domain with the custom **xxxUPNxxx.xxxCustomDomainxxx.xxx** domain. In doing so, you will update the UPN suffix for the primary domain and the UPN on every on-premises user account in AD DS with **@xxxUPNxxx.xxxCustomDomainxxx.xxx**. 

A company may change its domain name (also known as a vanity domain name) for a variety of reasons. For example, a company may purchase a new domain name, or a company may change its name and it wants its domain name to reflect the new company name, or a company may be sold and it wants its domain name to reflect the new parent company’s name. Regardless of the underlying reason, the goal of changing a domain name is typically to change the domain name on each user’s email address. 

For this lab, Adatum has purchased a new domain (provided by your lab hosting provider); therefore, it wants to change the domain name of all its users’ email addresses from @adatum.com to @xxxUPNxxx.xxxCustomDomainxxx.xxx.

1. Switch to **LON-DC1** where you should still be logged in as **ADATUM\Administrator** and password **Pa55w.rd**. 

2. You must now open **Windows PowerShell**. Select the magnifying glass (**Search**) icon on the taskbar at the bottom of the screen and type **powershell** in the Search box that appears. In the menu that appears, right-click on **Windows PowerShell** and select **Run as administrator** in the drop-down menu. 

3. Using **Windows PowerShell**, you must replace the on-premises **adatum.com** domain with the **xxxUPNxxx.xxxCustomDomainxxx.xxx** domain (where you will replace xxxUPNxxx with the unique UPN name assigned to your tenant, and you will replace xxxCustomDomainxxx.xxx with the custom domain created by your lab hosting provider). In doing so, you will update the UPN suffix for the primary domain and the UPN on every user in AD DS with **@xxxUPNxxx.xxxCustomDomainxxx.xxx**.

	‎In the following Powershell command, the **Set-ADForest** cmdlet modifies the properties of an Active Directory forest, and the **-identity** parameter specifies the Active Directory forest to modify. To perform this task, run the following command to set the **UPNSuffixes** property for the **adatum.com** forest (remember to change xxxUPNxxx to your unique UPN name and xxxCustomDomainxxx.xxx to your lab hosting provider's custom domain name):
		
		Set-ADForest -identity adatum.com -UPNSuffixes @{replace="xxxUPNxxx.xxxCustomDomainxxx.xxx"}

4. You must then run the following command that changes all existing adatum.com accounts to the new xxxUPNxxx.xxxCustomDomainxxx.xxx domain (remember to change xxxUPNxxx to your unique UPN name and xxxCustomDomainxxx.xxx to your lab hosting provider's custom domain name):

	‎	Get-ADUser -Filter * -Properties SamAccountName | ForEach-Object { Set-ADUser $_  -UserPrincipalName ($_.SamAccountName + "@xxxUPNxxx.xxxCustomDomainxxx.xxx" )}

5. You will continue using PowerShell on LON-DC1 in the next task.


### Task 2: Prepare problem user accounts   

Integrating your on-premises Active Directory with Azure AD makes your users more productive by providing a common identity for accessing both cloud and on-premises resources. However, errors can occur when identity data is synchronized from Windows Server Active Directory (AD DS) to Azure Active Directory (Azure AD). 

For example, two or more objects may have the same value for the **ProxyAddresses** attribute or the **UserPrincipalName** attribute in on-premises Active Directory. There are a multitude of different conditions that may result in synchronization errors. Organizations can correct these errors by running Microsoft's IdFix tool, which performs discovery and remediation of identity objects and their attributes in an on-premises Active Directory environment in preparation for migration to Azure Active Directory. 

In this task, you will run a script that breaks various Adatum on-premises user accounts. As part of your Adatum pilot project, you are purposely breaking these identity objects so that you can run the IdFix tool in the next task to see how it fixes the broken accounts. 

1. On LON-DC1, in the Windows PowerShell window, run the following command to change the root source to **C:\labfiles** so that you can access any files from that location:

		CD C:\labfiles\ 

2. PowerShell's execution policy settings dictate which PowerShell scripts can be run on a Windows system. Setting this policy to **Unrestricted** enables Holly to load all configuration files and run all scripts. At the command prompt, type the following command, and then press Enter:

		Set-ExecutionPolicy -ExecutionPolicy Unrestricted

3. You will then be prompted to confirm the execution policy change. Type **A** and press Enter to select the **[A] Yes to All** option.

4. Enter the following command that runs a PowerShell script that creates problem user accounts. This script is stored in the **C:\labfiles** folder. The users that are included in this script purposely have issues with their user accounts; this will enable you to troubleshoot these accounts in the next task using the IdFix tool.

		.\CreateProblemUsers.ps1
	
	**Note:** Wait until the script has completed before proceeding to the next task. This Windows PowerShell script will make the following changes in AD DS:

	- **Klemen Sic**. Update the UserPrincipalName for Klemen to include an extra "@" character. 

	- **Lara Raisic**. Update the emailAddress attribute for Lara to **lara@adatum.com**. 


	- **Logan Boyle**. Update the emailAddress attribute for Logan to **lara@adatum.com**.

	- **Maj Hojski**. Update the emailAddress attribute for Maj to blank characters (“ “). 

	- **Holly Dickson**. Update the emailAddress attribute for Holly to **holly @adatum.com**. 

	

5. Close PowerShell.


### Task 3: Run the IdFix tool and fix identified issues 

In this task you will download and use the IdFix tool to fix the user accounts that were broken in the previous task. Running the IdFix tool will correct any user account errors prior to synchronizing identity data between your on-premises environment and Azure AD.

1. You should still be logged into **LON-DC1** as the **Administrator** from the prior task.

2. In your **Microsoft Edge** browser, you should still be logged into Microsoft 365 as the **MOD Administrator** (**admin@M365xZZZZZZ.onmicrosoft.com**). You should have tabs open for the **Microsoft Office Home** page and the **Microsoft 365 admin center**. Open a new instance of **Edge** if you don't have one open.


	In your **Edge** browser, open a new tab and then enter the following URL in the address bar to access the Office 365 page for the IdFix Directory Synchronization Error Remediation Tool:
	
	**https://microsoft.github.io/idfix/installation/**


3. On the **Microsoft - IdFix** window, under the **Installation** section at the top of the page, the instructions direct you to run **setup.exe** to install the IdFix application on your machine. Select **setup.exe** to download the file to LON-DC1. 

4. Once the **setup.exe** file is downloaded, it will appear in the notification bar in the upper right-hand corner of the **Edge** window. Select **Open file**.

5. In the **Do you want to run this file?** dialog box, select **Run**.

6. In the **Do you want to install this application?** dialog box, select **Install**.

7. In the **Do you want to run this file?** dialog box, select **Run**.

8. In the **IdFix Privacy Statement** message box, select **OK**. 

9. In the **IdFix** window that appears, on the menu bar at the very top of the screen, select **Query** to query the directory. After a short wait, you should see several errors. 

10. Select the **ERROR** column heading to sort the records by error in alphabetical error.

	‎**Note:** If any **topleveldomain** errors appear, then ignore them as they cannot be fixed by the IdFix tool.  

11. In the **Klemen Sic** row, select the drop-down arrow in the **ACTION** field and select **EDIT**. 

12. On the menu bar at the top of the window, select **Apply**. 

13. In the **Apply Pending** dialog box that appears, select **Yes**.

	‎**Note:** Notice that the value in the **Action** column changed from **EDIT** to **COMPLETE** for Klemen, this indicates that IdFix updated the user object and corrected the error. 

14. On the menu bar at the top of the window, select **Query** to refresh the query results. 


15. In the query results, note how Klemen no longer appears in the results.

16. Find the **Logan Boyle** row. Note how the **VALUE** for Logan was incorrectly entered as **Lara@adatum.com**, which resulted in a duplicate error because this is the same email address as Lara Raisic, which appears above it.

	To fix this email attribute for Logan, you must first select the **[E]Lara@adatum.com** value in the **UPDATE** column for Logan and then replace it by typing **logan@adatum.com**. Then select the drop-down arrow in the **ACTION** field and select **EDIT**. 

	>**Note** A bug exists in IdFix v2.3 that prevents IdFix from showing you all the duplicates. It only shows a single duplicate. Having run the CreateProblemUsers PowerShell script, you will need to change the emailAddress attribute for Logan via **Active Directory Users and Computers** or via **ADSIEdit**. Once you have made the update, select **Query** again and Lara should not appear. You will also not see **Maj Hojski** listed either. You should then proceed to **Step 19**.

17. On the menu bar at the top of the window, select **Apply**. 

18. In the **Apply Pending** dialog box that appears, select **Yes**.

	>‎**Note:** This will update the two user objects and correct their UPN. 

19. On the menu bar, select **Query**. In the query results, note how the users you just fixed no longer appear in the results.

	**Note:** If a dialog box appears indicating an unhandled exception has occurred, select **Continue**.

	As you can see, there are two users whose errors you have not fixed (**An Dung Dao** and **Ngoc Bich Tran**). We are purposely leaving these errors alone so that you can see what happens during the synchronization process using the Azure AD Connect tool in the next exercise when it processes users with these conditions.

	**Important:** When there are format and duplicate errors for distinguished names, the **UPDATE** column either contains the same string as the **VALUE** column (which is the case for these two final users), or the **UPDATE** column entry is blank. In either case, this means that IdFix cannot suggest a remediation for the error. You can either fix these errors outside IdFix, or manually remediate them within IdFix. You can also export the results and use Windows PowerShell to remediate many errors.  

20. Close the IdFix and File Explorer windows. 

21. Proceed to the next exercise. You are now ready to install the Azure AD Connect tool and enable synchronization. 
  

# Proceed to Lab 4 - Exercise 2
 
