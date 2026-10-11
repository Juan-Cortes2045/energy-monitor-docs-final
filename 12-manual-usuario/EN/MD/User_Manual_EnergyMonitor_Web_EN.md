**USER MANUAL**

**EnergyMonitor**

_Electrical consumption monitoring system for the home_

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
<th>EnergyMonitor – Electrical Consumption Monitor</th>
</tr>
<tr class="header">
<th><strong>Document type</strong></th>
<th>User Manual</th>
</tr>
<tr class="odd">
<th><strong>Team members</strong></th>
<th><p>Juan Esteban Cortez Parra (Project leader / DB Manager)</p>
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
<th><strong>Group (Ficha)</strong></th>
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
<th>2.0</th>
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

Landing Page

Registration

Account verification

Login

Password recovery

Recovery verification

Dashboard

Consumption (project detail)

Profile

Settings

Favorites

Notifications

Logout

System messages

Frequently asked questions

Glossary

Appendices

# **Introduction**

EnergyMonitor was born from a simple idea: that anyone can know how much they are spending on electricity without having to wait for their service provider to deliver the final consumption figure. Behind the application there are PZEM-004T V3 current sensors, installed next to the home's outlets and controlled by an ESP32 microcontroller, which send the information the system uses to build consumption dashboards and charts in near real time.

This document explains how to use the platform as it is available today: create an account, verify it, log in, recover the password when it is forgotten, and move around the main dashboard, the profile and the settings. The project is still under construction, so features such as consumption detail per appliance do not appear here yet; they will be added to this manual as they become available in the frontend.

# **Objective**

For any user, without help from the development team, to be able to register, enter the system and move around with confidence through the screens that EnergyMonitor currently has available.

# **Scope**

This document covers what an end user needs to take their first steps on the platform: registering an account and verifying it by email, logging in, password recovery, navigating the public landing page, the main dashboard (Dashboard), creation and management of households, roles found within the household and their flow, personal profile management, configuration settings and logout.

What you will not find here is the internal architecture of the system, the source code, the database design, or the modules that are still under construction, such as project consumption detail, device linking, alert thresholds or user administration within a household. Those screens will be added to this manual when they are implemented and available to end users.

# **Requirements to use the system**

To enter EnergyMonitor you only need a computer or a cell phone with an up-to-date browser (Chrome, Firefox, Edge or Safari), a stable internet connection and a valid email address to create the account. The system does not ask you to install anything additional: everything works directly from the browser.

The measurement hardware (the PZEM-004T V3 sensors and the ESP32) is installed and configured separately, and is not part of this manual; here only the use of the web platform is explained.

## **Landing Page**

**Description:** It is the entry point to the system: the public screen that anyone can see without needing an account. It presents what EnergyMonitor is about and gives direct access to Log in or Register.

| **Field**                | **Detail**                                                                                                                                                                                                                                         |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role that can access** | General public (no account required).                                                                                                                                                                                                              |
| **Access path**          | Main URL of the system (home page).                                                                                                                                                                                                                |
| **Screen components**    | Top navigation bar with the links Home, Benefits, How it works, About us and Support; language selector; button to change the visual theme; Log in and Register buttons; main section with the welcome message and the system's featured benefits. |

**Steps to use it:**

> 1\. Open the EnergyMonitor URL in the browser.
>
> 2\. Browse the page to learn what the system does, reviewing the Benefits and How it works sections.
>
> 3\. If you want more context about the project, go to the About us link.
>
> 4\. Click Register if it is your first time, or Log in if you already have an account.

**Expected result:** The person understands what the platform is for and can continue to registration or login.

**Remarks:** The language and visual theme chosen here are kept afterwards within the application.

![alt text](../../assets/Web/image.png)

_Landing Page_

The home menu takes us to How it works: This section presents how the flow within the application works in general terms, from registering or logging in to receiving smart alerts.

![alt text](../../assets/Web/image-1.png)

Within the home menu we can also find the Benefits section, a section with short cards that summarize why it is worth using the system: saving on the bill by identifying unnecessary consumption, receiving real-time alerts for peaks or unusual activity, reducing the environmental footprint with more efficient energy use, and having a simple setup that does not require technical knowledge to connect the devices.

![alt text](../../assets/Web/image-2.png)

_Benefits_

From that same menu you reach About us, a page that explains in more detail the problem EnergyMonitor seeks to solve, the project's mission and a brief explanation of the hardware that makes monitoring possible.

![alt text](../../assets/Web/image-3.png)

_About us_

##

## It closes with the Support menu, the space designed for when the user needs help: finding answers to frequently asked questions or having a direct contact channel with the EnergyMonitor team if something does not work as it should.

![alt text](../../assets/Web/image-4.png)

_Support_

##

## **Registration**

**Description:** Form where a new person creates their EnergyMonitor account, with name, email and password, or directly with their Google account.

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
<th><strong>Role that can access</strong></th>
<th><p>General public (without a created acc</p>
<p>ount).</p></th>
</tr>
<tr class="header">
<th><strong>Access path</strong></th>
<th>Landing Page &gt; Register button.</th>
</tr>
<tr class="odd">
<th><strong>Screen components</strong></th>
<th>Name, Email and Password fields; checkbox accepting the terms of use and privacy policy; Register button; Register using Google option; Already have an account? Log in link.</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

**Steps to use it:**

> 1\. From the Landing Page, click Register.
>
> 2\. Complete the Name, Email and Password fields, or choose the Google option.
>
> 3\. Accept the terms of use and the privacy policy.
>
> 4\. Confirm with the Register button.

**Expected result:** The system creates the account and immediately redirects to the email verification screen.

**Remarks:** If the email is already registered, the system indicates it and suggests logging in instead.

![alt text](../../assets/Web/image-5.png)

_Registration_

## **Account verification**

**Description:** Screen that confirms that the email used in registration is real. The system sends a 6-digit code that must be entered here to activate the account.

| **Field**                | **Detail**                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------------------------ |
| **Role that can access** | Newly registered user, with the account not yet verified.                                        |
| **Access path**          | Opens automatically when Registration is finished.                                               |
| **Screen components**    | Field for the 6-digit code; Didn't receive the code? link; Already have an account? Log in link. |

**Steps to use it:**

> 1\. Check the inbox of the email with which the account was registered.
>
> 2\. Copy the 6-digit code received.
>
> 3\. Enter it in the verification field.
>
> 4\. Confirm to activate the account.

**Expected result:** The account is verified and the user can now log in normally.

**Remarks:** If the code does not arrive, a resend can be requested from the Didn't receive the code? link.

![alt text](../../assets/Web/image-6.png)

_Account verification_

## **Login**

**Description:** Form to log in with an account that has already been created and verified.

| **Field**                | **Detail**                                                                                                                                                   |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Role that can access** | General public with a created and verified account.                                                                                                          |
| **Access path**          | Landing Page \> Log in button.                                                                                                                               |
| **Screen components**    | Email and Password fields; Remember me checkbox; Forgot your password? link; Log in button; Log in using Google option; Don't have an account? Sign up link. |

**Steps to use it:**

> 1\. Open the EnergyMonitor URL.
>
> 2\. Click Log in.
>
> 3\. Enter the registered email and password.
>
> 4\. Confirm with the Log in button.

**Expected result:** The system validates the credentials and takes you directly to the Dashboard.

**Remarks:** If the email or password do not match, an error message appears and you are not allowed to continue.

![alt text](../../assets/Web/image-7.png)

_Login_

## **Password recovery**

**Description:** Entry point to regain access when the password is forgotten. This screen only asks for the email with which the account was created.

| **Field**                | **Detail**                                                         |
| ------------------------ | ------------------------------------------------------------------ |
| **Role that can access** | General public (with an already created account).                  |
| **Access path**          | Login \> Forgot your password? link                                |
| **Screen components**    | Email field; button to send the recovery code; Back to Login link. |

**Steps to use it:**

> 1\. On the Login screen, click Forgot your password?
>
> 2\. Enter the email with which the account was created.
>
> 3\. Confirm the submission.

**Expected result:** The system sends a 6-digit verification code to the email to continue with the recovery.

**Remarks:** If the email entered is not registered in the system, check that it is typed correctly before trying again.

![alt text](../../assets/Web/image-8.png)

_Password recovery_

## **Recovery verification**

**Description:** Step where the code sent by email is entered to confirm that the recovery is being done by the real owner of the account, and from where the new password is defined.

| **Field**                | **Detail**                                                                                 |
| ------------------------ | ------------------------------------------------------------------------------------------ |
| **Role that can access** | User in the process of recovering their password.                                          |
| **Access path**          | Opens automatically after requesting password recovery.                                    |
| **Screen components**    | Field for the 6-digit verification code; fields for the new password and its confirmation. |

**Steps to use it:**

> 1\. Check the email and copy the code received.
>
> 2\. Enter it on the verification screen.
>
> 3\. Define the new password and confirm it.
>
> 4\. Save the changes.

**Expected result:** The password is updated and the system returns to the login screen.

**Remarks:** For security, the code has a time limit for use; if it expires, you must request a new one from Password recovery

![alt text](../../assets/Web/image-9.png)

_Verify email_

## **Dashboard**

**Description:** It is the main panel, the first thing that appears after logging in. It shows the projects (homes or offices) that the user has registered, or an invitation to create the first one if none exists yet.

| **Field**                | **Detail**                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role that can access** | Authenticated user.                                                                                                                                                                                                                                                                                                                                                                                                        |
| **Access path**          | Opens automatically after login; it is also available from the Home icon in the side menu.                                                                                                                                                                                                                                                                                                                                 |
| **Screen components**    | Side menu with the Home icon and access to Settings; profile icon at the bottom of the menu; in the central area, the cards of the registered projects or, if there are none yet, a guide message with the Create Project button. The side menu includes other shortcuts (such as Favorites and Notifications) that correspond to modules still under construction and that will be documented when they become available. |

**Steps to use it:**

> 1\. Log in to the system.
>
> 2\. The Dashboard appears automatically, without needing to look for it in the menu.
>
> 3\. If it is the first time, use the Create Project button to register the first home or office.
>
> 4\. If projects already exist, click on the card of one to enter its detail (Consumption, History, Users, Devices, Thresholds and Project tabs).

**Expected result:** The user sees the status of their projects at a glance and can create a new one when needed.

**Remarks:** The detail of each project is documented in the Consumption (project detail) section, later in this manual.

![alt text](../../assets/Web/image-10.png)

_Dashboard with no registered projects_

![alt text](../../assets/Web/image-11.png)

_Dashboard with registered projects_

### **Form to register households:**

**Description:** This form allows any authenticated user to create a household in order to manage it.

| **Field**                | **Detail**                                                                                                                                                                                                                                         |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role that can access** | Authenticated user.                                                                                                                                                                                                                                |
| **Access path**          | On the dashboard there are 2 access paths: the first is found in the center when we authenticate on the page and reach the dashboard, and the second is found at the top right in the button with a + sign.                                        |
| **Screen components**    | Within the form to create a household we can find 4 fields where we can fill in name, household type, address and description as an optional field, as well as 3 buttons: one to cancel the process, register household and an X to exit the form. |

**Steps to use it:**

> 1\. The Dashboard appears automatically, without needing to look for it in the menu
>
> 2\. If it is the first time, use the Create Project button to register the first household.
>
> 3\. If projects already exist, click the button at the top right with a + sign.
>
> 4\. Fill in the form to register a household.

**Expected result:** The user sees a card with the registered household.

**Remarks:** The household can only be managed from within it.

![alt text](../../assets/Web/image-12.png)

_Form to register households_

### **Form to join a household:**

**Description:** This form allows anyone who is registered and has a household's access code to access it.

| **Field**                | **Detail**                                                                                                                                                                                                  |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role that can access** | Authenticated user.                                                                                                                                                                                         |
| **Access path**          | On the dashboard there are 2 access paths: the first is found in the center when we authenticate on the page and reach the dashboard, and the second is found at the top right in the button with a + sign. |
| **Screen components**    | Within the form to join a household you can find a field where you can enter the access code, as well as 3 buttons: one to cancel the process, register household and an X to exit the form.                |

**Steps to use it:**

> 1\. The Dashboard appears automatically, without needing to look for it in the menu
>
> 2\. If it is the first time, use the button to join a household.
>
> 3\. If households already exist, click the button at the top right with a + sign.
>
> 4\. Fill in the form to access a household.

**Expected result:** The user sees a card with the household they have joined.

**Remarks:** Within the household, the member is only allowed to view the household's information.

![alt text](../../assets/Web/image-13.png)

_Form to register households_

### **Consumption**

**Description:** It is the tab that opens by default when entering a project. It shows the status of electrical consumption in near real time: current power, the day's consumption, connected devices, the consumption curve for the last 24 hours, the distribution by device and progress against the daily and monthly limits.

| **Field**                | **Detail**                                                                                                                                                                                                                                                                   |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role that can access** | Authenticated user, any member of the project.                                                                                                                                                                                                                               |
| **Access path**          | Dashboard \> project card \> Consumption tab.                                                                                                                                                                                                                                |
| **Screen components**    | KPI cards for current power, today's consumption and active devices; area chart with consumption for the last 24 hours; pie chart with the current consumption distribution; bar chart with consumption per device; progress bars for the daily limit and the monthly limit. |

**Steps to use it:**

> 1\. Go to the Dashboard and click on the card of the project you want to check.
>
> 2\. The system opens the project detail on the Consumption tab.
>
> 3\. Review the KPI cards to see the general status.
>
> 4\. Scroll down the screen to check the consumption-by-hour charts, per-device charts and progress toward the configured limits.

**Expected result:** The user understands how the project's electrical consumption is doing at the time of the query.

**Remarks:** The data shown depends on the information reported by the linked ESP32 devices; if a device is disconnected, its contribution is not reflected in real time.

![alt text](../../assets/Web/image-14.png)

### **History**

**Description:** It stores the project's consumption over time, not just what happened today. It can be filtered by day, week, month or year, and within a monthly view, reviewed week by week.

| **Field**                | **Detail**                                                                                                                                                                                                                                                   |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Role that can access** | Authenticated user, any member of the project.                                                                                                                                                                                                               |
| **Access path**          | Project detail \> History tab.                                                                                                                                                                                                                               |
| **Screen components**    | Period selector (Day, Week, Month, Year); week selector within the month; bar chart with the consumption distribution per device over the period; consumption ranking table per device; cards for total consumption of the period, average and top consumer. |

**Steps to use it:**

> 1\. From the project detail, click the History tab.
>
> 2\. Choose the period you want to check (day, week, month or year).
>
> 3\. If Month was chosen, select the specific week or the Show all option.
>
> 4\. Review the chart and the ranking table to identify which device consumed the most.

**Expected result:** The user can compare consumption between different periods and detect which appliance weighs the most on the bill.

![alt text](../../assets/Web/image-15.png)

_History_

### **Users (Owner)**

**Description:** Shows who has access to the project and manages the members belonging to the household.

| **Field**                | **Detail**                                                 |
| ------------------------ | ---------------------------------------------------------- |
| **Role that can access** | Household owner (inviting is only available to the owner). |
| **Access path**          | Project detail \> Users tab.                               |
| **Screen components**    | List of project members with their role (Owner or Member). |

**Steps to use it:**

> 1\. From the project detail, click the Users tab.
>
> 2\. Review the list of current members.
>
> 3\. If you have the owner role, you can share the access code to allow members to join.

**Expected result:** View the list of current members.

**Remarks:** Only the project owner can remove members.

![alt text](../../assets/Web/image-16.png)

_Users_

### **Users (Member)**

**Description:** Shows who has access to the project.

| **Field**                | **Detail**                                                 |
| ------------------------ | ---------------------------------------------------------- |
| **Role that can access** | Member (inviting is only available to the owner).          |
| **Access path**          | Project detail \> Users tab.                               |
| **Screen components**    | List of project members with their role (Owner or Member). |

**Steps to use it:**

> 1\. From the project detail, click the Users tab.
>
> 2\. Review the list of current members.

**Expected result:** View the list of current members.

**Remarks:** Only the project owner can remove members.

![alt text](../../assets/Web/image-17.png)

_Users_

###

###

### **Devices (Owner)**

**Description:** Lists the ESP32 devices linked to the project, with their connection status and the consumption they are reporting, as well as allowing more devices to be linked to the household.

| **Field**                | **Detail**                                                                                                                                  |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role that can access** | Household owner.                                                                                                                            |
| **Access path**          | Project detail \> Devices tab.                                                                                                              |
| **Screen components**    | Link device button; list of devices with name, location, status (Online / Disconnected) and current consumption; option to delete a device. |

**Steps to use it:**

> 1\. From the project detail, click the Devices tab.
>
> 2\. Review the status of each linked device.
>
> 3\. To add a new one, use the Link device button.
>
> 4\. To remove one, use the delete icon next to the device.

**Expected result:** The user knows how many devices are active and can link or remove devices as needed.

**Remarks:** A device appears as Disconnected when it stops reporting data for a prolonged time.

![alt text](../../assets/Web/image-18.png)

_Devices_

### **Devices (Member)**

**Description:** Lists the ESP32 devices linked to the project, with their connection status and the consumption they are reporting.

| **Field**                | **Detail**                                                                                                       |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| **Role that can access** | Household member.                                                                                                |
| **Access path**          | Project detail \> Devices tab.                                                                                   |
| **Screen components**    | Link device button; list of devices with name, location, status (Online / Disconnected) and current consumption. |

**Steps to use it:**

> 1\. From the project detail, click the Devices tab.
>
> 2\. Review the status of each linked device.

**Expected result:** The user knows how many devices are active.

**Remarks:** A device appears as Disconnected when it stops reporting data for a prolonged time.

![alt text](../../assets/Web/image-19.png)

_Devices_

### **Thresholds (Owner)**

**Description:** Defines the project's daily and monthly consumption limits, used to generate alerts and recommendations.

| **Field**                | **Detail**                                                                                                |
| ------------------------ | --------------------------------------------------------------------------------------------------------- |
| **Role that can access** | Household owner                                                                                           |
| **Access path**          | Project detail \> Thresholds tab.                                                                         |
| **Screen components**    | Switch to use the system's default thresholds; Daily limit and Monthly limit fields; Save changes button. |

**Steps to use it:**

> 1\. From the project detail, click the Thresholds tab.
>
> 2\. Decide whether to use the default thresholds or to customize them.
>
> 3\. If customizing, turn off the switch and enter the new daily and monthly limits.
>
> 4\. Confirm with Save changes.

**Expected result:** The new limits are saved and the system uses them to generate alerts when consumption approaches or exceeds them.

**Remarks:** Thresholds are saved per project; changing them in one does not affect the user's other projects.

![alt text](../../assets/Web/image-20.png)

_Thresholds_

### **Thresholds (Member)**

**Description:** View the household's registered daily and monthly consumption limits, used to generate alerts and recommendations.

| **Field**                | **Detail**                            |
| ------------------------ | ------------------------------------- |
| **Role that can access** | Household member                      |
| **Access path**          | Project detail \> Thresholds tab.     |
| **Screen components**    | Daily limit and Monthly limit fields. |

**Steps to use it:**

> 1\. From the project detail, click the Thresholds tab.
>
> 2\. Decide whether to use the default thresholds or to customize them.

**Expected result:** View the household's registered daily and monthly consumption limits.

**Remarks:** Thresholds are defined per household.

![alt text](../../assets/Web/image-21.png)

_Thresholds_

### **Household information (Owner)**

**Description:** Gathers the household's general information (name, type, address, description, creation date), the household owner's data and the access code to share the household with other people.

| **Field**                | **Detail**                                                                                                 |
| ------------------------ | ---------------------------------------------------------------------------------------------------------- |
| **Role that can access** | Household owner (deleting the project is only available to the person in charge).                          |
| **Access path**          | Project detail \> Project tab.                                                                             |
| **Screen components**    | Project information block; owner block with email; access code to share and invite; Delete project button. |

**Steps to use it:**

> 1\. From the project detail, click the Project tab.
>
> 2\. Review the general and owner data.
>
> 3\. If you need to invite someone else, copy the access code and share it.
>
> 4\. If you are the owner and want to delete the project, use the Delete project button.

**Expected result:** The user has the household's administrative information at hand and can share access or delete it if it is theirs to do.

**Remarks:** Deleting a project is a permanent action.

![alt text](../../assets/Web/image-22.png)

_Project_

### **Household information (Member)**

**Description:** Gathers the household's general information (name, type, address, description, creation date) and the household owner's data.

| **Field**                | **Detail**                                                                           |
| ------------------------ | ------------------------------------------------------------------------------------ |
| **Role that can access** | Household member (deleting the project is only available to the person in charge).   |
| **Access path**          | Project detail \> Project tab.                                                       |
| **Screen components**    | Project information block, owner block with email and button to leave the household. |

**Steps to use it:**

> 1\. From the project detail, click the Project tab.
>
> 2\. Review the general and owner data.

**Expected result:** The user has the household's administrative information at hand.

**Remarks:** If you leave the household you will not be able to enter unless the owner shares the access code with you again.

![alt text](../../assets/Web/image-23.png)
_Project_

## **Profile**

**Description:** Gathers the account's personal data: name, email and the option to update the password.

| **Field**                | **Detail**                                                     |
| ------------------------ | -------------------------------------------------------------- |
| **Role that can access** | Authenticated user.                                            |
| **Access path**          | Dashboard \> profile icon, at the bottom of the side menu.     |
| **Screen components**    | Account data (name and email) and option to edit the password. |

**Steps to use it:**

> 1\. From the Dashboard, click the profile icon.
>
> 2\. Review the registered data.
>
> 3\. Edit the data that needs to be updated.
>
> 4\. Save the changes.

**Expected result:** The account data is updated.

**Remarks:** This screen may have design adjustments in upcoming versions; the general editing flow will remain the same.

![alt text](../../assets/Web/image-24.png)

_Profile_

## **Settings**

**Description:** Gathers the account's personal preferences: the interface language and the system's visual theme.

| **Field**                | **Detail**                                                                                            |
| ------------------------ | ----------------------------------------------------------------------------------------------------- |
| **Role that can access** | Authenticated user.                                                                                   |
| **Access path**          | Side menu \> Settings.                                                                                |
| **Screen components**    | Language selector (Spanish, English, Portuguese or French) and visual theme selector (light or dark). |

**Steps to use it:**

> 1\. Go to Settings from the side menu.
>
> 2\. Choose the preferred language among the available options.
>
> 3\. Choose the system's visual theme.
>
> 4\. Confirm; the change is applied immediately, without reloading the page.

**Expected result:** The preferences are saved and are reflected across the entire platform, not just on that screen.

![alt text](../../assets/Web/image-25.png)

_Settings_

## **Logout**

**Description:** Action to leave the system securely.

| **Field**                | **Detail**                              |
| ------------------------ | --------------------------------------- |
| **Role that can access** | Authenticated user.                     |
| **Access path**          | Profile icon \> Log out.                |
| **Screen components**    | Log out option within the profile menu. |

**Steps to use it:**

> 1\. Click the profile icon, at the bottom of the side menu.
>
> 2\. Select the Log out option.

**Expected result:** The system closes the active session and returns to the Landing Page or to the login screen.

![alt text](../../assets/Web/image-26.png)

_Log out_

## **Favorites**

**Description:** The Favorites module lets the user quickly access the projects they consult most frequently. Instead of looking for them in the Dashboard every time they log in, they can mark them as favorites to find them from a single place.

| **Field**                | **Detail**                                                                                           |
| ------------------------ | ---------------------------------------------------------------------------------------------------- |
| **Role that can access** | Authenticated user.                                                                                  |
| **Access path**          | Side menu \> Favorites                                                                               |
| **Screen components**    | Empty view in case there are no favorite projects, or quick access to the projects through the cards |

**Steps to use it:**

1.  Mark projects as favorites using the heart button located at the top right of the project cards.

2.  Go to Favorites from the side menu.

**Expected result:** To be able to find a specific project much more easily in case we have too many linked or created projects

![alt text](../../assets/Web/image-27.png)

> _Favorites_

## **Notifications**

**Description:** Gathers the alerts and recommendations that the system generates from the consumption of the user's projects. Notifications are divided into two types: alerts (notices about something that requires attention, such as an exceeded limit or a disconnected device) and recommendations (suggestions to optimize consumption), and can be filtered or marked as read from the same screen.

| **Field**                | **Detail**                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Role that can access** | Authenticated user.                                                                                                                                                                                                                                                                                                                                              |
| **Access path**          | Side menu \> Notifications                                                                                                                                                                                                                                                                                                                                       |
| **Screen components**    | All, Alerts and Recommendations tabs, each with a pending counter; Mark all as read link; list of notification cards, each with an icon according to its type, a title, a brief description, the related project and device, the date and time, a status label (New, Active or Resolved) and an action button (Resolve or Mark as read) or an icon to delete it. |

**Steps to use it:**

> 1\. From the side menu, click Notifications. Review the full list or filter by Alerts or Recommendations depending on what you want to check.
>
> 2\. Open a card to see the notification's detail.
>
> 3\. Mark it as read or resolve it, as appropriate; you can also mark all as read at once.

**Expected result:** The user finds out in time about important situations in their projects (such as an exceeded limit or a disconnected device) and can follow up on them or dismiss them once they have been handled.

![alt text](../../assets/Web/image-28.png)

_Notifications_

# **System messages**

| **System message**                      | **Meaning**                                                       | **User action**                                        |
| --------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------ |
| Required field                          | A required piece of data in the form is missing.                  | Fill in the marked field before continuing.            |
| Invalid username or password            | The credentials entered do not match any account.                 | Check the email and password, or recover the password. |
| Email already registered                | The email entered is already associated with an existing account. | Log in or recover the password for that account.       |
| Invalid or expired verification code    | The code entered does not match or has already expired.           | Request the code to be resent and try again.           |
| Successful registration or verification | The account was created or verified correctly.                    | Continue with login.                                   |
| Password updated                        | The new password was saved correctly.                             | Log in with the new password.                          |

# **Frequently asked questions**

**What do I do if I forgot my password?**

Go to the Login screen and click Forgot your password? The system sends a code by email to define a new one.

**What happens if the verification code does not reach me?**

It is worth checking the spam or junk mail folder. If it still does not appear, you can request that it be resent from the same screen.

**Can I change the language or theme whenever I want?**

Yes. Both are adjusted from Settings, and can be changed as many times as needed.

**Can I register with my Google account?**

Yes, both in Registration and in Login there is an option to enter directly with Google.

**Does EnergyMonitor work without internet?**

No. The platform needs a connection to synchronize the data and show the information in real time.

**When will screens such as Favorites or Notifications be available?**

Those two screens are still under development and will be released in upcoming versions of the system; this manual will be updated when they are ready.

# **Glossary**

| **Term**            | **Description**                                                                                                            |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| ESP32               | Microcontroller used to capture and transmit the electrical consumption measurements of each appliance.                    |
| PZEM-004T V3 sensor | Current sensor installed next to the outlet that measures electrical consumption in real time.                             |
| kWh                 | Kilowatt-hour, unit used to measure electrical energy consumption.                                                         |
| Verification code   | 6-digit code that the system sends by email to confirm the user's identity, both at registration and in password recovery. |
| Project             | Home, apartment or office that a user registers within EnergyMonitor to monitor its electrical consumption.                |
| Dashboard           | Main panel that appears after logging in, with the projects registered by the user.                                        |
| Session             | Period of time during which a user remains authenticated within the system, from logging in until logging out.             |

# **Appendices**

Before delivering the final version, this section still needs to be completed with real evidence from the system for the Favorites and Notifications screens, which are still under development.

This manual was written based on the SRS and the Technical Report of the EnergyMonitor project, and was reviewed by comparing each documented screen against the current state of the frontend. The Favorites and Notifications modules were not fully included in this version because they are still under development; when the development team releases them, they must be documented following the same structure used in the other screens (description, role, path, components, steps, expected result and remarks), in accordance with the guide for preparing software manuals of the ADSO program.
