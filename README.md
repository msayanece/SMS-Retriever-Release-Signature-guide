# SMS-Retriever-Release-Signature-guide
Guide to generate signature for SMS Retriever API if you have enabled Google App Signing in Play console.

## Prerequisites
You should know how to use a command prompt or bash terminal. Some knowledge of keytool and keystore is prefered to understand the below steps.

## Method to Generate Application Signature for SMS Retriever API
The following steps need to be taken for generating App signature for the Release APK if you have enabled App signing by Google Play in your Play console.

### STEP - 1
Download the deployment certificate from Google play console. 
> All Application>Select your Application> Release management> App signing> App signing certificate> Download certificate
### STEP - 2
Take a backup of your keystore (.jks/.keystore) file of your application project.
### STEP - 3
Install the git bash if you are using Windows OS for using the below bash command else just open your bash terminal.
### STEP - 4
Go to your java (or Android jre) bin directory> Right click> git bash here
### STEP - 5
Type (or copy whatever) this…

> ./keytool.exe -importcert -file ***your_deployment_cert.der_directory_with_file_name_extension*** -keystore ***your_keystore_file_of_the_app_directory_with_file_name_extension*** -alias use_a_new_alias_name

#### Note: Alias name should be different than the one generated while signing the App

#### Example:
```
 ./keytool.exe -importcert -file E:/s/temp/downloads/deployment_cert.der -keystore E:/s/openssl/bin/signaturenew.jks -alias newdesideals
```
When prompt -: 
**‘Enter keystore password:’** comes, use the old password of the keystore.
You should be able to see all the details of the certificate as a result. Match the details  (SHA1 etc) with the one in the Google Play Console (which you have downloaded earlier).


##### When prompt -:
**Trust this certificate? [no]:** comes, type ***‘y’*** and press Enter key.

You should be able to see the below message without any error…

Certificate was added to keystore 
### STEP - 6
Type the below command in the git bash after that.

> ./keytool.exe -exportcert -alias ***use_a_new_alias_name*** -keystore ***your_keystore_file_of_the_app_directory_with_file_name_extension*** | xxd -p | tr -d "[:space:]" | echo -n ***your_application_id_or_package_name*** `cat` | sha256sum | tr -d "[:space:]-" | xxd -r -p | base64 | cut -c1-11

#### Example:
```
./keytool.exe -exportcert -alias newdesideals -keystore E:/s/openssl/bin/signaturenew.jks | xxd -p | tr -d "[:space:]" | echo -n com.desideals.user `cat` | sha256sum | tr -d "[:space:]-" | xxd -r -p | base64 | cut -c1-11
```

## New SMS Format
Send the SMS containing the OTP from your server (using SMS Send API) like the following format... 
> <#> The OTP message lgS******0bg

