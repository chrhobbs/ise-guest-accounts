# ise-guest-accounts
Create Guest Accounts on Cisco ISE using REST API (Postman).  If you are using an external application (e.g. PassagePoint) to create Guest wireless credentials on Cisco ISE to provide to visitors and want to test that you have correctly configured the API on ISE.

> **Notes**
>- ISE 2.3 sandbox on dcloud.cisco.com
>- Postman client to send REST API requests to ISE
>- Requires External RESTful Services (ERS) API
https://www.cisco.com/c/en/us/td/docs/security/ise/2-1/api_ref_guide/api_ref_book/ise_api_ref_ers1.html
>- Enabling ERS Example (bit old, but was able to use the initial information to enable ERS): https://communities.cisco.com/docs/DOC-66297
>- Check your json formatting with http://json.parser.online.fr/


# On Cisco ISE...#

>- Schedule dcloud http://dcloud.cisco.com if you don't have access to an ISE server to play with.  select, schedule and access “Cisco ISE 2.3 Mobility Sandbox v1”

Cisco Dcloud
![](/images/dcloud.png)

“Cisco ISE 2.3 Mobility Sandbox v1”
![](/images/dcloud-ise-sandbox.png)


Connect to ISE PAN
![](/images/ise-pan.png)

### Enable ERS API within ISE ###
- Navigate to **Administration** > **System** > **Settings** and select ERS Settings from the left panel.
- Enable the ERS APIs by selecting Enable ERS for Read/Write and Save
![](/images/ise-enable-ers.png)

>Note: The ERS APIs are disabled by default for security so you must enable it.
>**ERS is also disabled after ISE upgrades, so have to re-enable it.**


### Create ERS API Admin User ###
![](/images/ise-ers-admin.png)
>- You must create separate users (not admin) with the ERS Admin (Read/Write) or ERS Operator (Read-Only) roles to use the ERS APIs.

- Navigate to Administration > System > Admin Access
- Choose Administrators > Admin Users from the left pane
- Choose +Add > Create an Admin User to create a new ers-admin account (and optional ers-operator account).
- This account will be used to collect ERS API information.


### Create a user account that has rights to create guest accounts ###

- Navigate to **Administration** > **Identity Management** > **Identities**, then choose Users from the left pane
- Choose +Add > Create a User to create a new apisponsor account.
- Note: By default, users in the ALL_ACCOUNTS user identity group are members of the sponsor group and can manage all guest user accounts.


#### Allow Guest Accounts to be created through the API ###

- Navigate to **Work Centers** > **Guest Access** > **Portals & Components**, then from the left menu, select “Sponsor Groups” and ALL_ACCOUNTS
- Check the “Access Cisco ISE guest accounts using the programmatic interface (Guest REST API)”.


# Using Postman... # 
## Use REST Client to confirm Sponsor Portal “id” ##

Note: Open the ERS API SDK Guide ###

https://_ISE PAN IP Address_:9060/ers/sdk

GET https://_ISE PAN IP Address_:9060/ers/config/sponsorportal


```
**HTTP example:**
GET /ers/config/sponsoredguestportal HTTP/1.1
Host: <ISE PAN IP Address>:9060
Content-Type: application/json
Accept: application/json
Authorization: Basic xxxxxxxxxxxxxxx =
Cache-Control: no-cache
Postman-Token: xxxxxxxx-xxxx-xxxx-xxxx-xxxx
```

```
**cURL example:**
curl -X GET \
  https://<ISE PAN IP Address>:9060/ers/config/sponsoredguestportal \
  -H 'accept: application/json' \
  -H 'authorization: Basic xxxxxxxxxxxxxxx=' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'postman-token: xxxxxxxx-xxxx-xxxx-xxxx-xxxx'
```

## Use REST Client to Create a Guest User ##

- Use POSTMAN with a POST message to create the user account (URL listed below)   
- Note use of user and Sponsor Portal ID from previous step
- Please see API documentation in the External RESTful Services (ERS) Online SDK referenced previously. Select “Guest User” for required and optional fields.

POST https://_ISE PAN IP Address_:9060/ers/config/guestuser
  
```
HTTP example:
POST /ers/config/guestuser HTTP/1.1
Host: <ISE PAN IP Address>:9060
Content-Type: application/json
Accept: application/json
Authorization: Basic xxxxxxxxxx==
Cache-Control: no-cache
Postman-Token: xxxxxxxxxx

{
    "GuestUser": {
        "id": "123456789",
        "name": "dpool2",
        "guestType": "Contractor (default)",
        "status": "ACTIVE",
        "sponsorUserName": "api-sponsor",
		"sponsorUserId": "fb5335f5-1456-41d4-8bdb-70b91398ebc7",
        "guestInfo": {
            "userName": "dpool2",
            "firstName": "Dead2",
            "lastName": "Pool",
            "emailAddress": "deadpool@marvel.com",
            "phoneNumber": "1111111111",
            "password": "168",
            "creationTime": "11/14/2017 12:03",
            "enabled": true,
            "notificationLanguage": "English",
            "smsServiceProvider": "Global Default"
        },
        "guestAccessInfo": {
            "validDays": 1,
            "location": "San Jose"
        },
        "portalId" : "f10871e0-7159-11e7-a355-005056aba474",
        "customFields": {},
        "link": {
            "rel": "self",
            "href": "https://<ISE PAN IP Address>:9060/ers/config/guestuser/name/dpool2",
            "type": "application/xml"
        }
    }
}
```

or 

```
cURL example:
curl -X POST \
  https://<ISE PAN IP Address>:9060/ers/config/guestuser \
  -H 'accept: application/json' \
  -H 'authorization: Basic xxxxxxxxxx==' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'postman-token: xxxxxxxxxx' \
  -d '{
    "GuestUser": {
        "id": "123456789",
        "name": "dpool2",
        "guestType": "Contractor (default)",
        "status": "ACTIVE",
        "sponsorUserName": "api-sponsor",
		"sponsorUserId": "fb5335f5-1456-41d4-8bdb-70b91398ebc7",
        "guestInfo": {
            "userName": "dpool2",
            "firstName": "Dead2",
            "lastName": "Pool",
            "emailAddress": "deadpool@marvel.com",
            "phoneNumber": "1111111111",
            "password": "168",
            "creationTime": "11/14/2017 12:03",
            "enabled": true,
            "notificationLanguage": "English",
            "smsServiceProvider": "Global Default"
        },
        "guestAccessInfo": {
            "validDays": 1,
            "location": "San Jose"
        },
        "portalId" : "f10871e0-7159-11e7-a355-005056aba474",
        "customFields": {},
        "link": {
            "rel": "self",
            "href": "https://<ISE PAN IP Address>:9060/ers/config/guestuser/name/dpool2",
            "type": "application/xml"
        }
    }
}'
```

# Validate on ISE #

- Go to **Work Centers** > **Guest Access** > **Reports**
- Select Reports, Guest Access Reports and then Sponsor Login and Audit from the left menu.
