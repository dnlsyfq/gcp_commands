# Cloud Storage: Qwik Start - Cloud Console

## Create a Bucket
1. Navigation menu > Storage > Browser. Click Create Bucket:

## Share an Object Publicly
1.  To create a publicly accessible URL for the object, click the drop-down menu (three vertical dots).

2.  Select Edit permissions from the drop-down menu.

3.  In the dialog that appears, click the + Add item button.

4.  Add a permission for all users by entering in the following:
```
Select User for the Entity.
Enter allUsers for the Name.
Select Reader for the Access.
```
---

# Cloud IAM: Qwik Start

## Setup for two users
These represent identities in Cloud IAM, each with different access permissions allocated to them.

## View or reset the user in a browser tab
To view which user in signed into a browser tab, hover over your Avatar to view your username in that browser tab.


* To reset which user is signed into a browser tab:
1.  Click your Avatar and click Sign out to sign out.
2. In the Connection Details panel, click Open Google Console and sign back in using the appropriate Username and Password.

## IAM console and project level roles

Navigation menu > IAM & admin > IAM. You are now in the "IAM & admin" console.
Click +ADD button near the top of the page and explore the project roles associated with Projects by clicking on the "Select a role" dropdown menu


## Remove project access

* Remove Project Viewer for Username 2
Navigation menu > IAM & admin > IAM. Then click the pencil icon next to Username 2.
Remove Project Viewer access for Username 2 by clicking the trashcan icon next to the role name. Then click SAVE.

## Add Storage permissions
select Navigation menu > IAM & admin > IAM.

Click + ADD to then paste the Username 2 name into the New members field.

In the Roles field, select Storage > Storage Object Viewer from the drop-down menu.

Click SAVE.

## Verify access
```
gsutil ls gs://[YOUR_BUCKET_NAME]
```


