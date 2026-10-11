**USER MANUAL**

**EnergyMonitor**

_Home electrical consumption monitoring system_

<table>
<colgroup>
<col style="width: 26%" />
<col style="width: 73%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Field</strong></th>
<th><strong>Detail</strong></th>
</tr>
<tr class="odd">
<th><strong>Project</strong></th>
<th>EnergyMonitor – Electrical consumption monitor</th>
</tr>
<tr class="header">
<th><strong>Document type</strong></th>
<th>User manual</th>
</tr>
<tr class="odd">
<th><strong>Team members</strong></th>
<th><p>Juan Esteban Cortez Parra (Project Leader / DB Manager)</p>
<p>Miguel Ángel García Artunduaga (Analyst / UX-UI)</p></th>
</tr>
<tr class="header">
<th><strong>Training program</strong></th>
<th>Technologist in Software Analysis and Development (ADSO)</th>
</tr>
<tr class="odd">
<th><strong>Instructor</strong></th>
<th>Karol Dayana Correa</th>
</tr>
<tr class="header">
<th><strong>Group (Ficha) No.</strong></th>
<th>3145556</th>
</tr>
<tr class="odd">
<th><strong>Training center</strong></th>
<th>Sena Industria-Neiva</th>
</tr>
<tr class="header">
<th><strong>Date</strong></th>
<th>July 23, 2026</th>
</tr>
<tr class="odd">
<th><strong>Document version</strong></th>
<th>3.0</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **Version control**

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 14%" />
<col style="width: 53%" />
<col style="width: 19%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Version</strong></th>
<th><strong>Date</strong></th>
<th><strong>Description of change</strong></th>
<th><strong>Responsible</strong></th>
</tr>
<tr class="odd">
<th>1.0</th>
<th>07/09/2026</th>
<th>Initial creation of the EnergyMonitor user manual.</th>
<th>Juan Esteban Cortes Parra<br />
Miguel Angel Garcia Artunduaga</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# **Table of contents**

Version control

Introduction

Objective

Scope

Requirements to use the system

Registration

Account verification

Login

Password recovery

Recovery verification

Main panel

Consumption (project detail)

Profile

Settings

Favorites

Notifications

Log out

System messages

Frequently asked questions

Glossary

Appendices

# **Introduction**

EnergyMonitor was born from a simple idea: that anyone can know how much they are spending on electricity without having to wait for their service provider to deliver the final consumption figure. Behind the application there are PZEM-004T V3 current sensors, installed next to the home's power outlets and controlled by an ESP32 microcontroller, which send the information the system uses to create consumption dashboards and charts in near real time.

This document explains how to use the platform as it is currently available: create an account, verify it, log in, recover the password if it is forgotten, and navigate the main panel, the profile and the settings. The project is still under construction, so some features, such as consumption detail per appliance, do not appear here yet; they will be added to this manual as they become available in the frontend.

# **Objective**

For any user, without help from the development team, to be able to register, log in to the system and navigate with confidence through the screens that EnergyMonitor currently has available.

# **Scope**

This document covers what an end user needs to take their first steps on the platform: account registration and verification by email, login, password recovery, main panel, creation and management of households, roles within a household and their flows, personal profile management, settings and logout.

Here you will not find the internal architecture of the system, the source code, the database design or the modules that are still under construction, such as project consumption detail, device linking, alert thresholds or user administration within a household. Those screens will be added to this manual once they are implemented and available to end users.

# **Requirements to use the system**

To access EnergyMonitor you only need a computer or a mobile phone with an up-to-date browser (Chrome, Firefox, Edge or Safari), a stable internet connection and a valid email address to create the account. The system does not require installing anything additional: everything works directly from the browser.

The measurement hardware (the PZEM-004T V3 sensors and the ESP32) is installed and configured separately and is not part of this manual; here only the use of the web platform is explained.

## **Registration**

**Description: Form in which a person creates their EnergyMonitor account with name, email and password, or directly with their Google account.**

<table>
<colgroup>
<col style="width: 26%" />
<col style="width: 73%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Field</strong></th>
<th><strong>Detail</strong></th>
</tr>
<tr class="odd">
<th><strong>Role with access</strong></th>
<th><p>General public (without an</p>
<p>account created).</p></th>
</tr>
<tr class="header">
<th><strong>Access path</strong></th>
<th>Home page &gt; Register button.</th>
</tr>
<tr class="odd">
<th><strong>Screen components</strong></th>
<th>Name, Email and Password fields; checkbox to accept the terms of use and the privacy policy; Register button; Register with Google option; Already have an account? Log in link.</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

**Steps to use it:**

> 1\. From the home page, click Register.
>
> 2\. Fill in the Name, Email and Password fields, or choose the Google option.
>
> 3\. Accept the terms of use and the privacy policy.
>
> 4\. Confirm with the Register button.

**Expected result: The system creates the account and immediately redirects you to the email verification screen.**

**Notes: If the email is already registered, the system indicates it and suggests logging in.**

![alt text](../../assets/Mobile/image.png)

_Registration_

## **Account verification**

**Description: Screen that confirms that the email used during registration is valid. The system sends a 6-digit code that must be entered here to activate the account.**

| **Field**             | **Detail**                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------ |
| **Role with access**  | Newly registered user whose account has not yet been verified.                                   |
| **Access path**       | Opens automatically when registration is completed.                                              |
| **Screen components** | Field for the 6-digit code; Didn't receive the code? link; Already have an account? Log in link. |

**Steps to use it:**

> 1\. Check the inbox of the email used to register the account.
>
> 2\. Copy the 6-digit code received.
>
> 3\. Enter it in the verification field.
>
> 4\. Confirm to activate the account.

**Expected result: The account is verified and the user can now log in normally.**

**Notes: If the code does not arrive, you can request that it be resent from the Didn't receive the code? link.**

![alt text](../../assets/Mobile/image-1.png)

_Account verification_

## **Login**

**Description: Form to log in with an account that has already been created and verified.**

| **Field**             | **Detail**                                                                                                                                                  |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | General public with a created and verified account.                                                                                                         |
| **Access path**       | Home page \> Log in button.                                                                                                                                 |
| **Screen components** | Email and Password fields; Remember me checkbox; Forgot your password? link; Log in button; Log in with Google option; Don't have an account? Sign up link. |

**Steps to use it:**

> 1\. Open the EnergyMonitor URL.
>
> 2\. Click Log in.
>
> 3\. Enter the registered email and password.
>
> 4\. Confirm with the Log in button.

**Expected result: The system validates the credentials and takes you directly to the main panel.**

**Notes: If the email or password do not match, an error message appears and you cannot continue.**

![alt text](../../assets/Mobile/image-3.png)

_Login_

## **Password recovery**

**Description: Entry point to regain access when the password is forgotten. This screen only asks for the email used to create the account.**

| **Field**             | **Detail**                                                         |
| --------------------- | ------------------------------------------------------------------ |
| **Role with access**  | General public (with an already created account).                  |
| **Access path**       | Login \> Forgot your password? link.                               |
| **Screen components** | Email field; button to send the recovery code; Back to login link. |

**Steps to use it:**

> 1\. On the login screen, click Forgot your password?
>
> 2\. Enter the email used to create the account.
>
> 3\. Confirm the submission.

**Expected result: The system sends a 6-digit verification code to the email to continue with the recovery.**

**Notes: If the email entered is not registered in the system, check that it is spelled correctly before trying again.**

![alt text](../../assets/Mobile/image-4.png)

_Password recovery_

## **Recovery verification**

**Description: Step in which the code sent by email is entered to confirm that the real account holder is carrying out the recovery, and the new password is defined.**

| **Field**             | **Detail**                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------ |
| **Role with access**  | User in the process of recovering their password.                                          |
| **Access path**       | Opens automatically after requesting password recovery.                                    |
| **Screen components** | Field for the 6-digit verification code; fields for the new password and its confirmation. |

**Steps to use it:**

> 1\. Check the email and copy the code received.
>
> 2\. Enter it on the verification screen.
>
> 3\. Set the new password and confirm it.
>
> 4\. Save the changes.

**Expected result: The password is updated and the system returns to the login screen.**

**Notes: For security, the code has a time limit for use; if it expires, you must request a new one from Password recovery.**

![alt text](../../assets/Mobile/image-5.png)

_Verify email_

## **Main panel**

**Description: It is the main panel, the first thing that appears after logging in. It shows the projects (homes or offices) that the user has registered or an invitation to create the first one if none exists yet.**

| **Field**             | **Detail**                                                                                                                                                                                                                                                                                                                                                                                                      |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Authenticated user.                                                                                                                                                                                                                                                                                                                                                                                             |
| **Access path**       | Opens automatically after logging in; it is also available from the Home icon in the side menu.                                                                                                                                                                                                                                                                                                                 |
| **Screen components** | Side menu with the Home icon and access to Settings; profile icon at the bottom of the menu; in the central area, cards of the registered projects or, if there are none yet, a guide message with the Create project button. The side menu includes other options (such as Favorites and Notifications) that correspond to modules still under construction and will be documented when they become available. |

**Steps to use it:**

> 1\. Log in to the system.
>
> 2\. The main panel appears automatically, without needing to look for it in the menu.
>
> 3\. If it is the first time, use the Create project button to register the first home or office.
>
> 4\. If projects already exist, click on one of their cards to access the detail (Consumption, History, Users, Devices, Thresholds and Project tabs).

**Expected result: The user can see the status of their projects at a glance and create a new one when needed.**

**Notes: The detail of each project is documented later in the Consumption (project detail) section of this manual.**

![alt text](../../assets/Mobile/image-6.png)

_Main panel with no registered projects_

![alt text](../../assets/Mobile/image-7.png)

_Main panel with registered projects_

## Form to register households:

**Description: This form allows any authenticated user to create a household in order to manage it.**

| **Field**             | **Detail**                                                                                                                                                                                                                                                     |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Authenticated user.                                                                                                                                                                                                                                            |
| **Access path**       | On the main panel there are two ways to access: the first appears in the center when logging in and arriving at the panel; the second is at the top right, in the button with the + sign.                                                                      |
| **Screen components** | In the form to create a household there are four fields to enter the name, the household type, the address and an optional description. There are also three buttons: one to cancel the process, another to register the household and an X to close the form. |

**Steps to use it:**

> 1\. The main panel appears automatically, without needing to look for it in the menu
>
> 2\. If it is the first time, use the Create project button to register the first household.
>
> 3\. If projects already exist, click the button with the + sign located at the top right.
>
> 4\. Fill in the form to register a household.

**Expected result: The user will see a card with the registered household.**

**Notes: A household can only be managed from within it.**

![alt text](../../assets/Mobile/image-8.png)

_Form to register households_

**Form to join a household:**

**Description: This form allows any registered person who has a household's access code to access it.**

| **Field**             | **Detail**                                                                                                                                                                                                                        |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Authenticated user.                                                                                                                                                                                                               |
| **Access path**       | On the dashboard we can find 2 access paths: the first can be found when we authenticate within the page and arrive at the dashboard, in the center, and the second can be found at the top right, on the button with the + sign. |
| **Screen components** | Within the form to join a home you can find a field where you can enter the access code, in addition to 3 buttons: one to cancel the process, register home, and an X to exit the form.                                           |

**Steps to use it:**

> 1\. The main panel appears automatically, without needing to look for it in the menu
>
> 2\. If it is the first time, use the button to join a household.
>
> 3\. If households already exist, click the button with the + sign located at the top right.
>
> 4\. Fill in the form to access a household.

**Expected result: The user will see a card with the household they have joined.**

**Notes: Within the household, the member can only view the household's information.**

![alt text](../../assets/Mobile/image-9.png)

_Form to register households_

### **Consumption**

**Description: It is the tab that opens by default when entering a project. It shows the status of electrical consumption in near real time: current power, the day's consumption, connected devices, the consumption curve for the last 24 hours, the distribution by device and progress with respect to the daily and monthly limits.**

| **Field**             | **Detail**                                                                                                                                                                                                                                                              |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Authenticated user, any member of the project.                                                                                                                                                                                                                          |
| **Access path**       | Main panel \> project card \> Consumption tab.                                                                                                                                                                                                                          |
| **Screen components** | Indicator cards for current power, the day's consumption and active devices; area chart of consumption for the last 24 hours; pie chart with the current consumption distribution; bar chart of consumption per device; progress bars for the daily and monthly limits. |

**Steps to use it:**

> 1\. Go to the main panel and click on the card of the project you want to check.
>
> 2\. The system opens the project detail on the Consumption tab.
>
> 3\. Review the indicator cards to see the general status.
>
> 4\. Scroll down to check the consumption charts by hour and by device, as well as the progress of the configured limits.

**Expected result: The user understands how the project's electrical consumption is doing at the time of the query.**

**Notes: The data shown depends on the information sent by the linked ESP32 devices; if a device is disconnected, its contribution is not reflected in real time.**

![alt text](../../assets/Mobile/image-10.png)

_Consumption_

### **History**

**Description: It stores the project's consumption over time, not just what happened today. It can be filtered by day, week, month or year and, in the monthly view, reviewed week by week.**

| **Field**             | **Detail**                                                                                                                                                                                                                                                                               |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Authenticated user, any member of the project.                                                                                                                                                                                                                                           |
| **Access path**       | Project detail \> History tab.                                                                                                                                                                                                                                                           |
| **Screen components** | Period selector (Day, Week, Month, Year); week selector within the month; bar chart with the consumption distribution per device during the period; consumption ranking table per device; cards with the total consumption for the period, the average and the highest-consuming device. |

**Steps to use it:**

> 1\. From the project detail, click the History tab.
>
> 2\. Choose the period you want to check (day, week, month or year).
>
> 3\. If you chose Month, select the specific week or the Show all option.
>
> 4\. Review the chart and the ranking table to identify which device consumed the most.

**Expected result: The user can compare consumption between different periods and identify which appliance has the greatest impact on the bill.**

![alt text](../../assets/Mobile/image-11.png)

_History_

### **Users (Owner)**

**Description: Shows who has access to the project and allows managing the household's members.**

| **Field**             | **Detail**                                                 |
| --------------------- | ---------------------------------------------------------- |
| **Role with access**  | Household owner (only the owner can invite).               |
| **Access path**       | Project detail \> Users tab.                               |
| **Screen components** | List of project members with their role (Owner or Member). |

**Steps to use it:**

> 1\. From the project detail, click the Users tab.
>
> 2\. Review the list of current members.
>
> 3\. If you have the owner role, you can share the access code to allow other members to join.

**Expected result: View the list of current members.**

**Notes: Only the project owner can remove members.**

![alt text](../../assets/Mobile/image-12.png)

_Users_

### **Users (Member)**

**Description: Shows who has access to the project.**

| **Field**             | **Detail**                                                 |
| --------------------- | ---------------------------------------------------------- |
| **Role with access**  | Member (only the owner can invite).                        |
| **Access path**       | Project detail \> Users tab.                               |
| **Screen components** | List of project members with their role (Owner or Member). |

**Steps to use it:**

> 1\. From the project detail, click the Users tab.
>
> 2\. Review the list of current members.

**Expected result: View the list of current members.**

**Notes: Only the project owner can remove members.**

![alt text](../../assets/Mobile/image-13.png)

_Users_

###

###

### **Devices (Owner)**

**Description: Shows the ESP32 devices linked to the project, their connection status and the consumption they report; it also allows linking more devices to the household.**

| **Field**             | **Detail**                                                                                                                                  |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Household owner.                                                                                                                            |
| **Access path**       | Project detail \> Devices tab.                                                                                                              |
| **Screen components** | Link device button; list of devices with name, location, status (Online / Disconnected) and current consumption; option to delete a device. |

**Steps to use it:**

> 1\. From the project detail, click the Devices tab.
>
> 2\. Review the status of each linked device.
>
> 3\. To add a new one, use the Link device button.
>
> 4\. To remove one, use the delete icon located next to the device.

**Expected result: The user will know how many devices are active and will be able to link or remove them as needed.**

**Notes: A device appears as Disconnected when it stops sending data for a prolonged period.**

![alt text](../../assets/Mobile/image-14.png)

_Devices_

### **Devices (Member)**

**Description: Shows the ESP32 devices linked to the project, their connection status and the consumption they report.**

| **Field**             | **Detail**                                                                                                       |
| --------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Household member.                                                                                                |
| **Access path**       | Project detail \> Devices tab.                                                                                   |
| **Screen components** | Link device button; list of devices with name, location, status (Online / Disconnected) and current consumption. |

**Steps to use it:**

> 1\. From the project detail, click the Devices tab.
>
> 2\. Review the status of each linked device.

**Expected result: The user will know how many devices are active.**

**Notes: A device appears as Disconnected when it stops sending data for a prolonged period.**

![alt text](../../assets/Mobile/image-15.png)

_Devices_

### **Thresholds (Owner)**

**Description: Defines the project's daily and monthly consumption limits, which are used to generate alerts and recommendations.**

| **Field**             | **Detail**                                                                                                |
| --------------------- | --------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Home owner                                                                                                |
| **Access path**       | Project detail \> Thresholds tab.                                                                         |
| **Screen components** | Switch to use the system's default thresholds; Daily limit and Monthly limit fields; Save changes button. |

**Steps to use it:**

> 1\. From the project detail, click the Thresholds tab.
>
> 2\. Decide whether you want to use the default thresholds or customize them.
>
> 3\. If you want to customize them, turn off the switch and enter the new daily and monthly limits.
>
> 4\. Confirm with Save changes.

**Expected result: The new limits are saved and the system uses them to generate alerts when consumption approaches or exceeds them.**

**Notes: Thresholds are saved per project; changing them in one does not affect the user's other projects.**

![alt text](../../assets/Mobile/image-16.png)

_Thresholds_

### **Thresholds (Member)**

**Description: Allows viewing the daily and monthly consumption limits registered for the household, used to generate alerts and recommendations.**

| **Field**             | **Detail**                            |
| --------------------- | ------------------------------------- |
| **Role with access**  | Household member                      |
| **Access path**       | Project detail \> Thresholds tab.     |
| **Screen components** | Daily limit and Monthly limit fields. |

**Steps to use it:**

> 1\. From the project detail, click the Thresholds tab.
>
> 2\. Decide whether you want to use the default thresholds or customize them.

**Expected result: View the daily and monthly consumption limits registered for the household.**

**Notes: Thresholds are defined per household.**

![alt text](../../assets/Mobile/image-17.png)

_Thresholds_

### **Household information (Owner)**

**Description: Gathers the household's general information (name, type, address, description and creation date), its owner's data and the access code to share the household with other people.**

| **Field**             | **Detail**                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Household owner (only the person in charge can delete the project).                                                       |
| **Access path**       | Project detail \> Project tab.                                                                                            |
| **Screen components** | Block with the project information; owner block with their email; access code to share and invite; Delete project button. |

**Steps to use it:**

> 1\. From the project detail, click the Project tab.
>
> 2\. Review the general data and the owner's data.
>
> 3\. If you need to invite another person, copy and share the access code.
>
> 4\. If you are the owner and want to delete the project, use the Delete project button.

**Expected result: The user will have the household's administrative information at hand and will be able to share access or delete it, if applicable.**

**Notes: Deleting a project is a permanent action.**

![alt text](../../assets/Mobile/image-18.png)

_Project_

### **Household information (Member)**

**Description: Gathers the household's general information (name, type, address, description and creation date) and its owner's data.**

| **Field**             | **Detail**                                                                                          |
| --------------------- | --------------------------------------------------------------------------------------------------- |
| **Role with access**  | Household member (only the person in charge can delete the project).                                |
| **Access path**       | Project detail \> Project tab.                                                                      |
| **Screen components** | Block with the project information, owner block with their email and button to leave the household. |

**Steps to use it:**

> 1\. From the project detail, click the Project tab.
>
> 2\. Review the general data and the owner's data.

**Expected result: The user will have the household's administrative information at hand.**

**Notes: If you leave the household, you will not be able to enter again unless the owner shares the access code with you again.**

![alt text](../../assets/Mobile/image-19.png)

_Project_

## **Profile**

**Description: Gathers the account's personal data: name, email and option to update the password.**

| **Field**             | **Detail**                                                     |
| --------------------- | -------------------------------------------------------------- |
| **Role with access**  | Authenticated user.                                            |
| **Access path**       | Main panel \> profile icon, at the bottom of the side menu.    |
| **Screen components** | Account data (name and email) and option to edit the password. |

**Steps to use it:**

> 1\. From the main panel, click the profile icon.
>
> 2\. Review the registered data.
>
> 3\. Edit the data that needs to be updated.
>
> 4\. Save the changes.

**Expected result: The account data is updated.**

**Notes: This screen may have design adjustments in upcoming versions; the general editing flow will remain the same.**

![alt text](../../assets/Mobile/image-20.png)

_Profile_

## **Settings**

**Description: Gathers the account's personal preferences: the interface language and the system's visual theme.**

| **Field**             | **Detail**                                                                                            |
| --------------------- | ----------------------------------------------------------------------------------------------------- |
| **Role with access**  | Authenticated user.                                                                                   |
| **Access path**       | Side menu \> Settings.                                                                                |
| **Screen components** | Language selector (Spanish, English, Portuguese or French) and visual theme selector (light or dark). |

**Steps to use it:**

> 1\. Go to Settings from the side menu.
>
> 2\. Choose the language you prefer among the available options.
>
> 3\. Choose the system's visual theme.
>
> 4\. Confirm; the change is applied immediately, without reloading the page.

**Expected result: The preferences are saved and applied across the entire platform, not just on that screen.**

![alt text](../../assets/Mobile/image-21.png)

_Settings_

## **Log out**

**Description: Action to leave the system securely.**

| **Field**             | **Detail**                              |
| --------------------- | --------------------------------------- |
| **Role with access**  | Authenticated user.                     |
| **Access path**       | Profile icon \> Log out.                |
| **Screen components** | Log out option within the profile menu. |

**Steps to use it:**

> 1\. Click the profile icon, at the bottom of the side menu.
>
> 2\. Select the Log out option.

**Expected result: The system closes the active session and returns to the home page or to the login screen.**

![alt text](../../assets/Mobile/image-22.png)

_Log out_

## **Favorites**

**Description:** The Favorites module lets the user quickly access the projects they consult most frequently. Instead of looking for them on the main panel every time they log in, they can mark them as favorites to find them in a single place.

| **Field**             | **Detail**                                                                                         |
| --------------------- | -------------------------------------------------------------------------------------------------- |
| **Role with access**  | Authenticated user.                                                                                |
| **Access path**       | Side menu \> Favorites                                                                             |
| **Screen components** | Empty view if there are no favorite projects, or quick access to the projects through their cards. |

**Steps to use it:**

1.  Mark projects as favorites using the heart-shaped button located at the top right of their cards.

2.  Go to Favorites from the side menu.

**Expected result: To be able to find a specific project more easily if there are many linked or created projects.**

![alt text](../../assets/Mobile/image-23.png)

_Favorites_

## **Notifications**

**Description:** Gathers the alerts and recommendations that the system generates from the consumption of the user's projects. Notifications are divided into two types: alerts (notices about situations that require attention, such as an exceeded limit or a disconnected device) and recommendations (suggestions to optimize consumption). They can be filtered or marked as read from the same screen.

| **Field**             | **Detail**                                                                                                                                                                                                                                                                                                                                         |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role with access**  | Authenticated user.                                                                                                                                                                                                                                                                                                                                |
| **Access path**       | Side menu \> Notifications                                                                                                                                                                                                                                                                                                                         |
| **Screen components** | All, Alerts and Recommendations tabs, each with a pending counter; Mark all as read link; list of notification cards, each with an icon according to its type, title, brief description, related project and device, date and time, status label (New, Active or Resolved) and an action button (Resolve or Mark as read) or an icon to delete it. |

**Steps to use it:**

> 1\. From the side menu, click Notifications. Review the full list or filter by Alerts or Recommendations depending on what you want to check.
>
> 2\. Open a card to see the notification's details.
>
> 3\. Mark it as read or resolve it, as appropriate; you can also mark all as read at once.

**Expected result: The user receives timely information about important situations in their projects (such as an exceeded limit or a disconnected device) and can follow up on them or dismiss them once they have been handled.**

![alt text](../../assets/Mobile/image-24.png)

_Notifications_

# **System messages**

| **System message**                      | **Meaning**                                                       | **User action**                                        |
| --------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------ |
| Required field                          | A required piece of data in the form is missing.                  | Fill in the marked field before continuing.            |
| Invalid username or password            | The credentials entered do not match any account.                 | Check the email and password, or recover the password. |
| Email already registered                | The email entered is already associated with an existing account. | Log in or recover the password for that account.       |
| Invalid or expired verification code    | The code entered does not match or has expired.                   | Request that the code be resent and try again.         |
| Successful registration or verification | The account was created or verified correctly.                    | Continue with login.                                   |
| Password updated                        | The new password was saved correctly.                             | Log in with the new password.                          |

# **Frequently asked questions**

**What do I do if I forgot my password?**

Go to the login screen and click Forgot your password? The system sends a code by email to set a new password.

**What happens if the verification code does not arrive?**

Check the junk mail or spam folder. If it still does not appear, you can request that it be resent from the same screen.

**Can I change the language or theme whenever I want?**

Yes. Both can be adjusted from Settings and changed as many times as necessary.

**Can I register with my Google account?**

Yes. Both in Registration and in Login there is an option to access directly with Google.

**Does EnergyMonitor work without internet?**

No. The platform needs a connection to synchronize the data and show the information in real time.

**When will screens such as Favorites or Notifications be available?**

Those two screens are still under development and will be available in upcoming versions of the system; this manual will be updated when they are ready.

# **Glossary**

| **Term**            | **Description**                                                                                                                |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| ESP32               | Microcontroller used to capture and transmit the electrical consumption measurements of each appliance.                        |
| PZEM-004T V3 sensor | Current sensor installed next to the power outlet that measures electrical consumption in real time.                           |
| kWh                 | Kilowatt-hour, unit used to measure electrical energy consumption.                                                             |
| Verification code   | 6-digit code that the system sends by email to confirm the user's identity, both during registration and in password recovery. |
| Project             | Home, apartment, or office that a user registers in EnergyMonitor to monitor its electricity consumption.                      |
| Main panel          | Main panel that appears after logging in and shows the projects registered by the user.                                        |
| Session             | Period during which a user remains authenticated in the system, from logging in until logging out.                             |

# **Appendices**

Before delivering the final version, this section must be completed with real evidence from the system for the Favorites and Notifications screens, which are still under development.

This manual was prepared based on the Software Requirements Specification (SRS) and the Technical Report of the EnergyMonitor project, and was reviewed by comparing each documented screen with the current state of the frontend. The Favorites and Notifications modules were not fully included in this version because they are still under development; when the development team releases them, they must be documented following the same structure used in the other screens (description, role, path, components, steps, expected result and notes), in accordance with the ADSO program's guide for preparing software manuals.
