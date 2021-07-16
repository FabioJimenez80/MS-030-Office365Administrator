# Module 7 - Lab 7 - Exercise 2 - Configuring Email Protection

In this lab, you will continue in your role as Holly Dickson, Adatum’s Enterprise Administrator. Adatum has experienced a recent rash of malware infections. The company's CTO has asked Holly to investigate the various options that are available in Exchange Online to fortify Adatum’s messaging environment.

You will access the Exchange admin center for Exchange Online from your client computer and create a series of hygiene filters that are designed to protect Adatum’s messaging environment. You will create a malware filter, a connection filter, and a spam filter. Finally, you will enable Microsoft 365 Advanced Threat Protection, which will safeguard Adatum against malicious threats posed by email messages, links (URLs), and collaboration tools.

### Task 1 - Create a Malware Filter  

In this task, you will create a malware filter that checks for attachments that have a specific file type that indicate a possible malware attachment. If an attachment is found matching one of those file types and the recipient’s domain matches Adatum’s Microsoft 365 domain, then default notification text will be applied to the message.

1. You should still be logged into LON-CL1 as the **Administrator** with a password of **Pa55w.rd**. 

2. Your Edge browser should be open from the prior lab, with tabs open for the **Microsoft Office Home** page, the **Microsoft 365 admin center**, and the **Exchange admin center**. You should still be signed into Microsoft 365 as Holly Dickson.

3. Open a new tab in **Edge** and navigate to **https://security.microsoft.com**. In the **Email &amp; collaboration** section of the left-hand navigation, select **Policies &amp; rules**.

4. Select **Threat policies** and then select **Anti-malware**.

5. On the menu bar, select the **+ Create** icon to add a new malware filter.

6. In the **Name your policy** window, enter **Malware Policy** in the **Name** field.

7. In the **Description** field, enter **This policy has been created to protect the messaging environment.** and then select **Next**.

8. In the **Users and Domains** page, select the **Domains** field and then type **M365** and from the list of options, select the **M365xZZZZZZ.onmicrosoft.com** domain (where ZZZZZZ is your tenant suffix ID provided by your lab hosting provider). and then select **Next**.

9. In the **Protection settings** page, review the defaults and available options and then select **Next**.

9. In the **Review** page, select **Submit**.

10. On the **Created new anti-malware policy** page, select **Done**.

11. In the breadcrumb navigation at the top of the page, select **Thread policies**.


### Task 2 - Create a Connection Filter (Anti-spam policy)

In this task, you will modify the default connection filter to include an allowed IP address and a blocked IP address. Any messages originating from the allowed IP address will always be accepted, and any messages originating from the blocked IP address will always be blocked. 

1. You should still be logged into LON-CL1 as the **Administrator** with a password of **Pa55w.rd**. 

2. Your Edge browser should be open from the prior task, with tabs open for the **Microsoft Office Home** page, the **Microsoft 365 admin center**, and the **Exchange admin center**. You should still be signed into Microsoft 365 as Holly Dickson. 

	If you closed the Exchange admin center tab after the prior task, then in the **Microsoft 365 Admin center**, under **Admin Centers** in the left-hand navigation pane, select **Exchange**.

3. In the **Threat policies** tab, select **Anti-spam**.

4. In the list of policies, select the **Connection filter (default)** filter. Once the policy details are displayed, select **Edit connection filter policy**.

5. At this time, you will **NOT** be adding IP addresses to the allow or block lists. You can do this if you have a known IP address you would like to test against. However, it typically takes up to 1 hour to propagate the change within the system. For this lab, simply review the fact that you can create allowed and blocked lists of IP addresses.

6. Select the **Turn on safe list** check box at the bottom of the page. This is a best practice that enables for your tenant the most common third-party sources of trusted senders that Microsoft subscribes to. Selecting this check box skips spam filtering on messages sent from these senders, ensuring that they are never mistakenly marked as spam.

8. Select **Save** and then select **Close** once the changes are successfully saved.

9. Leave the **Anti-spam policies tab open**.


### Task 3 - Create a Spam Filter

For Microsoft 365 customers whose mailboxes are hosted in Microsoft Exchange Online, their email messages are automatically protected against spam and malware. Microsoft 365 has built-in malware and spam filtering capabilities that help protect inbound and outbound messages from malicious software and help protect you from spam.

As Adatum’s Enterprise Administrator, Holly doesn't need to set up or maintain the filtering technologies, which are enabled by default. However, she can make company-specific filtering customizations in the Exchange admin center. She has decided to test this out by configuring a spam policy to grant or deny an email by focusing on the language of the email and the location of the email's origin.

1. You should still be logged into LON-CL1 as the **Administrator** with a password of **Pa55w.rd**. 

2. Your Edge browser should be open from the prior task, with tabs open for the **Microsoft Office Home** page, the **Microsoft 365 admin center**, and the **Exchange admin center**. You should still be signed into Microsoft 365 as Holly Dickson. 

	If you closed the Exchange admin center tab after the prior task, then in the **Microsoft 365 Admin center**, under **Admin Centers** in the left-hand navigation pane, select **Exchange**.

3. Select your classic **Exchange admin center** tab, and then select the **protection** tab in the left-hand navigation pane, On this page, the **connection filter** tab should be displayed. Since you want to create a spam filter, select the **spam filter** tab at the top of the page.

4. In the list of spam filters, the **Default** filter is already selected by default. Select the **pencil (edit)** icon in the menu bar that appears above the filter list to edit this Default filter.

5. In the **Default** window, in the left-hand pane, select **spam and bulk actions**. 

	**Note:** In this section you will be presented a variety of options on how you would like spam to be handled and what rating will be triggered depending on the severity of the spam.

6. In the **spam and bulk actions** section, make the following selections:

	- Spam: **Move message to Junk Email folder**

	- High Confident Spam: **Prepend subject line with text**

7. In the **Bulk email** section, make the following selections:

	- Mark bulk email as spam: Leave this check box selected

	- Select the threshold: select the drop-down arrow and change the threshold to **5**

8. In the **Quarantine** section, make the following selections:

	- Retain spam for (days): **10**

	- Prepend subject line with this text: enter **QUARANTINED: This message contains potential spam**

9. In the left-hand navigation pane, select **international spam**. 

	**Note:** This section allows you to automatically tag messages as spam whose origins comes from countries/regions that are black listed, as well as messages written in a specific language.

10. Select the check box at the top of the page that says **Filter email messages written in the following languages**.

11. Select the **plus (+) sign** icon below this check box to add the languages being filtered.

12. In the **Select Language** window, hold down the **Ctrl** key and select the languages that you want to flag as spam. Then select the **add-&gt;** button, and then select **OK** to confirm your selection.

13. Below the list of languages that you selected, select the check box that says **Filter email messages sent from the following countries or regions**.

14. Select the **plus** (+) sign below this check box to add the countries or regions.

15. In the **Select Region** window, hold down the **Ctrl** key and select the countries or regions that you want to flag as being origins of spam. Then select the **add-&gt;** button, and then select **OK** to confirm your selection.

16. In the left-hand navigation pane, select **advanced options**. 

	**Note:** This section allows you to automatically tag messages as spam that have embedded URL’s with specific attributes or that have embedded HTML in the message.

17. Under the **Increase Spam Score** section, turn **On** the following options:

	- **URL redirect to other port**

	- **URL to .biz or .info websites**

18. Under the **Mark as Spam** section, turn **On** the following options:

	- **Empty messages**

	- **Conditional Sender ID filtering: hard fail**

19. Select **Save** and then select **OK** once the changes are successfully saved.

20. In the list of spam filters, the **Default** filter that you just edited is selected and a summary of the filter is now displayed in the right-hand pane. Scroll down in the right-hand pane and note how **End-user spam notifications** are disabled. Below this option, select **Configure end-user spam notifications**.

21. In the **edit end-user spam notifications** window, select the **Enable end-user spam notifications** check box, and then change the **Send end-user spam notifications every (days)** value to **5**.

22. Select **Save** and then select **OK** once the changes are successfully saved.

23. Leave the Exchange Admin Center open and proceed to the next exercise.


### Task 4: Enable Advanced Threat Protection and Create a Safe Attachments Policy

In this task, you will turn on Advanced Threat Protection (ATP) for SharePoint, OneDrive, and Microsoft Teams, and you will create an ATP Safe Attachments policy that will test email attachments for malware that are sent to recipients within Adatum's M365xZZZZZZ.onmicrosoft.com domain. You will configure the policy so that if an attachment is blocked, it will be removed from the email that is sent to the recipient, and a copy of the email will be redirected to Joni Sherman for additional review.

1. You should still be logged into LON-CL1 as the **Administrator** with a password of **Pa55w.rd**. 

2. In your Edge browser session, select your **Anti-spam policies** tab from an earlier task and in the breadcrumb navigation at the top of the page, select **Thread policies**.

4. Select the **Safe Attachments** link.

5. In the **Safe attachments** window, select **Global settings** on the menu bar.

6. In the **Global settings** window, at the top of the page under the **Protect files in SharePoint, OneDrive, and Microsoft Teams** section, select the **Turn on Defender for SharePoint, OneDrive and Microsoft Teams** toggle switch to **On** and then select **Save**.

7. On the **Safe attachments** page, select **+ Create** on the menu bar to add a new Safe Attachments policy. This will initiate a **New Safe Attachment Policy** wizard to create a new policy.

8. On the **Name your policy** page, enter **AttachmentPolicy1** in the **Name** field and then select **Next**.

9. On the **Users and domains** page, select the **Domains** field and then type **M365** and from the list of options, select the **M365xZZZZZZ.onmicrosoft.com** domain (where ZZZZZZ is your tenant suffix ID provided by your lab hosting provider). and then select **Next**.

9. On the **Settings** page, under the **Safe attachments unknown malware response** section, select the **Dynamic Delivery** option. This option will still send the email but will hold the attachment until it has been scanned and marked acceptable.

10. Under the **Redirect attachment on detection** section, select the **Enable redirect** check box.

11. In the **Send the attachment to the following email address** field, enter Joni Sherman's email address of **JoniS@M365xZZZZZZ.onmicrosoft.com** (where ZZZZZZ is your unique tenant ID provided by your lab hosting provider) and then select **Next**.

18. On the **Review** page, review your entries and if anything needs to be changed, select the appropriate **Edit** link. If everything appears correct, select **Submit** and then select **Done*.

19. Leave LON-CL1 and your **Edge** session open for the next exercise.

**NOTE:** Unfortunately, we are unable to create a training lab in which you can validate the ATP Safe Attachments policy that you just created. To do so, you must send an email that contains a malicious attachment. There are some common test viruses that are available, such as the EICAR test virus; however, with well-known test viruses such as EICAR, the messages in which they are attached get quarantined before they can be processed by Office 365 ATP. Since the ATP Safe Attachments functionality is meant to protect against unknown and zero-day viruses and malware, it is very difficult, and not recommended, to create such an attachment.

That being said, after you have defined ATP Safe Attachment policies in your real-world environment, one good way to see how the service is working is by viewing Advanced Threat Protection reports. For more information on using ATP reporting to validate your Safe Links and Safe Attachment policies, see [View reports for Office 365 Advanced Threat Protection](https://docs.microsoft.com/en-us/office365/securitycompliance/view-reports-for-atp).



**Results**: After completing this exercise, you should have configured anti-spam and anti-virus settings.

# Proceed to Lab 7 - Exercise 3