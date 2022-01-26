# Frequently Asked Questions (FAQ)
1. **Q:** The SDK could not be downloaded properly. What should I do?<br/>**A:** Check the GitHub **username** and **password** you are using in your gradle project settings file. If Two-Factor Authentication (TFA) is enabled in your GitHub Account, use a **personal access token** instead. When generating the token, ensure that you set it to **no expiry date** and you check the **read access** from a repository. To generate the token follow the steps in this link: **https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token**<br/><br/>
2. **Q:** I am getting this error when calling the **checkout** function: 
**PermissionDenied: rpc error: code = PermissionDenied desc = unauthorized for chosen environment** What should I do?<br/>**A:** Check the **isDebug** value you are passing via the **initialize** function (its default value is set to **false**). If you are to access the **sandbox** environment, the value should be set to **true**. If you are to access the **Production** environment, it should be set to **false**.<br/><br/>
3. **Q**: Can I just download the library manually?<br/>**A:** Yes. You can download the SDK's here: **https://github.com/orgs/brankas/packages?repo_name=core-sdk-android**









