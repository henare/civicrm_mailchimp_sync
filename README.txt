Mailchimp / CiviCRM Synchronisation Script:
===========================================



This is the beta version of a small script that will synchronise your groups and contacts between CiviCRM and Mailchimp Email Marketing software. The script uses the uses the MailChimp API to synchronise contacts with CiviCRM.



Installation Instructions:
==========================

Generate Mailchimp API Key:
===========================

To generate a Mailchimp api key, log in to Mailchimp, choose account > api keys and info, then click on the "Add Key" button.

An api key will be generated and displayed below, if you already have a key, it will be displayed on this page.  Simply copy and past the key into the cron script above.



Add a Mailing List to Mailchimp called "CiviCRM":
=================================================

This list will be what the script will use to synchronise all the contacts and groups with.



Storing in the API key in the script.
=====================================

In order for the script to communicate with your Mailchimp account, it needs an API key for your account.

To do this, edit the mailchimp_sync.php file, and on line 70, enter your api key where there is all zeros.

This is what the line of code looks like before edited:

$this->apikey = (isset($_GET['apikey'])) ? $_GET['apikey'] : "000000000000000000000000000";



Copy the script into your CiviCRM installation.
=====================================


To install the synchronisation script, copy it to the 'civicrm/bin/' directory, so that the path to the script is:

in Drupal: 
sites/all/modules/civicrm/bin/mailchimp_sync.php

in Joomla:
/administrator/components/com_civicrm/civicrm/bin/mailchimp_sync.php


Next, add a cron job entry for the script.
==========================================


If you are on a Linux server:
----------------------------

On the command prompt, type:

$ crontab -e

Append the following line to the end of the file:

@daily http://www.yourwebsite.com/path/to/mailchimp_sync.php

where "http://www.yourwebsite.com/path/to/" is the url of your mailchimp script.

 - for example: http://www.example.com/sites/all/modules/civicrm/bin/mailchimp_sync.php


If you are on Windows Server:
-----------------------------

Open your systems Task Scheduler (see http://support.microsoft.com/kb/308569), and browse to your systems web browser and add the script as a parameter:

 - for example: "C:\Program Files\Mozilla Firefox\firefox.exe" http://www.example.com/sites/all/modules/civicrm/bin/mailchimp_sync.php




What happens when the script runs?
==================================

The script will run daily, usually around midnight, and will synchronise all the day's activities in relation to mails and mailing lists.


1.	Adding new group to Mailchimp, via CiviCRM.

Any new group in CiviCRM that is of the type "Mailing List" will be picked up the script and added to Mailchimp automatically, (unless it already exists in Mailchimp).
This new group will have a sub-grouping in Mailchimp, called "default", and this is where all the contacts that are in that group will go.

The script does not yet add groups that are created in Mailchimp to CiviCRM.

2.	Adding contacts to group(s) in Mailchimp, via CiviCRM.

After the script adds CiviCRM groups to Mailchimp, it then adds contacts that are in those groups to Mailchimp. If the contacts already exist on Mailchimp, then the script ensures they are in the same mailing list groups on Mailchimp, as the ones they are a part of in CiviCRM, and vice versa.


3.	Unsubscribing users from Mailchimp / CiviCRM group.

In CiviCRM, any user that has the "Do not email" box checked under "Communication Preferences", or the "On Hold" box checked, will be unsubscribed from Mailchimp mailing lists.  Once this is done, that email address cannot be 're-subscribed' to Mailchimp.  Also, any users that un-subscrive themselves via Mailchimp, will have the "Do not email" flag set in their CiviCRM Communication Preferences.

4.	Recording undeliverable emails as a contact activity in CiviCRM.

This will be recorded as an activity in CiviCRM, (of type "email" with a status of "failed"), and will be visible under that contacts "Activities" tab.

5.	Recording bulk mailing activity as contact activities in CiviCRM.

Likewise, any emails sent from Mailchimp to contacts in a group / mailing list, will have this recorded as an activity under the "Activities" tab.

6.	Removing users from Mailchimp that are deleted from CiviCRM.

Users thaqt are deleted from CiviCRM, are unsubscribed from Mailchimp, (note that they cannot be 're-subscribed' once this is done).

7.	Removing contacts from groups in Mailchimp that are removed from groups in CiviCRM.

The groups are synchronised, so if a contact is removed from a group in Mailchimp, then the contact is also removed from the corresponding group in CiviCRM.

8.	Adding users who subscribe on Mailchimp as CiviCRM contact.

Sometimes will subscribe to your mailing list throgh a website etc. and these users will be added to CiviCRM as a contact within the corresponding mailing list group that they subscribed to.

9.	Updating names of groups on Mailchimp that are changed on CiviCRM.

If the name of a group is edited on CiviCRM, it is updated on Mailchimp.

10.	Updating names of contacts on Mailchimp that are changed on CiviCRM.

Likewise, if the name of a c is eontactdited on CiviCRM, it is updated on Mailchimp also.



