# External Integration API and SDK
Cam.TV API for accessing, authenticating and linking a user's Cam.TV account directly to the application domain. The complete documentation of the API together with call examples, can be consulted at the [Postman Documentation](https://documenter.getpostman.com/view/9304344/SW7Z48Z2).

The APIs allows login and registration through Cam.TV credentials, allows you to obtain the personal data of the user and of the relative sponsors. They also offer the possibility to assign the user a specific subscription or an upgrade of the same.
Finally, they offer the possibility to disconnect the user from the platform with respect to Cam.TV.

The user authentication process also involves the client part of the platform, as the initial request originates from this. Cam.TV provides a javascript script to be inserted in the page, which exports an initialization function of the user authentication flow to Cam.TV. The flow takes place in a pop-up window on Cam.TV domain, at the end the control returns to the calling window (the application site), together with a TemporaryAuthToken which allows the application server to complete the procedure and obtain the data user. The following figure illustrates the planned steps.

![alt authentication flow](https://bootcamp.r.worldssl.net/camtv_xnet_auth.jpg "authentication flow")

# Frontend Implementation
To add the login popup with Cam.TV, the following script must be inserted:
	
	<script type="text/javascript" src="https://www.cam.tv/assets/js/camtv_login.js"></script>
	<div class="wrapper">
		<script>
			var settings = {
				RedirectURI: "{{RedirectURI}}",
				ApiKey: "{{PrivateApiKey}}"
			}
		</script>
		<button onclick="CTVLogin.Login(settings);">Login with Cam.TV</button>
	</div>

## The PrivateApiKey and PublicApiKey keys
To authenticate the user and enable him to use the ExternalIntegration API, your platform must have the PublicApiKey and PrivateApiKey keys. These keys are essentially credentials that are provided by Cam.TV on request. These keys must not be shared with third parties, and for security reasons they cannot be provided again if lost. The keys, together with the TempAuthToken which is a temporary token generated at the user's login according to the two keys, are the only way to provide the third-party platforms with the data of the Cam.TV user who has authenticated himself, together with those of its sponsors.
	
## Version
API ExternalIntegration Cam.TV v2.0

## Requests
All API Requests includes the POST form-url-encoded method. APIs are intended server-to-server, authentication is done through header Authorization with Bearer consisting of a PublicApiKey provided by Cam.TV and only associated with the domain/IP of the application server.

### HEADERS
| Key           |               Value               |            Description |
|---------------|:---------------------------------:|-----------------------:|
| Content-Type  | application/x-www-form-urlencoded |                        |
| Authorization | Bearer {PublicApiKey}             | String given by Cam.TV |

### PARAMS
| Key           |               Value               |                                           Description |
|---------------|:---------------------------------:|------------------------------------------------------:|
| EMail         | application/x-www-form-urlencoded |                                                       |
| Password      | Bearer {PublicApiKey}             |                                String given by Cam.TV |
| RedirectURI   | {Your_Site}                       |     Where you will be redirected after authentication |
| TempAuthToken | {TempToken}                       | Unique string which will grant you access to the APIs |
| ProductCode   | {ABBO:NAME}                       |              Name of the subscription you want to buy |

## Responses
Each API Response is in JSON format. Here are a few examples:

https://www.cam.tv/api/externalintegration/v2/authorize 
```
{
    "Status": "Authorized",
    "UserData": {
		"FirstName": {{FirstName}},
		"LastName": {{LastName}},
		"AbboType": {{AbboType}},
		"CreationDateTime": {{CreationDateTime}},
		"IsFounder": {{IsFounder}},
		"CID": G1766,
		"ParentFirstName": {{ParentFirstName}},
		"ParentLastName": {{ParentLastName}},
		"ParentCID": {{ParentCID}},
		"VirtualParentFirstName": {{VirtualParentFirstName}},
		"VirtualParentLastName": {{VirtualParentLastName}},
		"VirtualParentCID": {{VirtualParentCID}}
	}
}
```

https://www.cam.tv/api/externalintegration/v2/purchase_abbo
```
{
	"Result": "Payment completed",
	"User": {
			"FirstName": {{FirstName}},
			"LastName": {{LastName}},
			"Abbo": {{AbboType}},
			"DataRegistrazione": {{DataRegistrazione}},
			"CID": G1766,
			"ParentFirstName": {{ParentFirstName}},
			"ParentLastName": {{ParentLastName}},
			"ParentCID": {{ParentCID}},
			"VirtualParentLastName": {{VirtualParentLastName}},
			"VirtualParentFirstName": {{VirtualParentFirstName}},
			"VirtualParentCID": {{VirtualParentCID}}
		}
}
```

https://www.cam.tv/api/externalintegration/v2/get_listing
```
{
    "Status": "Listing",
    "Articles": [
        {
            "FeeOnSalesPerc": 15,
            "CommissionDirectPerc": 20,
            "CommissionNetworkPerc": 6,
            "Name": "Basic",
            "CostEuro": 490000,
            "Period": 2,
            "Type": 13,
            "DaysDuration": 365,
            "FeePercent": 5
        },
        {
            "FeeOnSalesPerc": 15,
            "CommissionDirectPerc": 20,
            "CommissionNetworkPerc": 6,
            "Name": "Basic",
            "CostEuro": 49000,
            "Period": 1,
            "Type": 23,
            "DaysDuration": 30,
            "FeePercent": 5
        },
        ...
    ]
}
```
## Contacts
For information or requests for clarifications, write to:  [admin@cam.tv](mailto:admin@cam.tv)
