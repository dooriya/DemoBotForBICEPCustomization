# DemoBotForBICEPCustomization

Steps to reuse an existing Bot Web app:
1. Configure your existing bot web app info (subscription, resource group, resource name) in `azure.parameter.<env>.json` for a specific remote environment that you want to reuse an existing web app. 
	    
    ![image](https://user-images.githubusercontent.com/10163840/171567754-71590078-ea42-4ce4-a863-a1fa28d9a652.png)

1. Update bicep files to skip provision bot web app
    * Update `templates\azure\provision\bot.bicep`: remove the resource definition for 'WebApp', and add the following code to resue existing web app:
        ```
        // Web App that hosts your bot
        resource webApp 'Microsoft.Web/sites@2021-02-01' existing = {
          name: webAppName
          scope: resourceGroup(provisionParameters['botAppSubscription'], provisionParameters['botAppResourceGroup'])
        }
        ```
    * Update `templates\azure\config.bicep` to config the existing bot web app.

	  ![image](https://user-images.githubusercontent.com/10163840/171568474-16143003-28bd-48dd-8c77-09d948e4c521.png)

Note: if you don't have permission to update the existing bot web app, he can remove these part to skip configuring bot web app and do it manually instead.
