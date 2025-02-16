---

copyright:

  years: 2021

lastupdated: "2021-11-29"

keywords: trusted profile, federated users, granting access, update trusted profile, compute resource, IAM trusted profile, trust relationship, establish trust, trust policy, trusted entity, assume access, apply access

subcollection: account

---

{{site.data.keyword.attribute-definition-list}}

# Updating trusted profiles
{: #trusted-profile-update}

You can update the permissions or redefine the trust relationship of the trusted profiles at any time by using either the console or the [IAM Identity Services](/apidocs/iam-identity-token-api#create-claim-rule-request) API. 
{: shortdesc}

To update trusted profiles, you must be assigned the administrator, operator, or editor role within the account, or on the IAM Identity Service.
{: note}

## Updating trusted profiles in the console
{: #updating-tp-console}
{: ui}

To update trusted profiles, go to **Manage** > **Access (IAM)** in the console and select **Trusted profiles**. Then, select the name of the trusted profile that you want to update. 

### Updating the description of your profile
{: #description}

Click the name of the trusted profile that you want to update, and select **Actions** > **Edit**. Enter the new name and description, and click **Apply**.

### Redefining the trust relationship
{: #trust}

After the trusted profile is created, you can build trust with both federated users and compute resources in the same trusted profile.

1. Click the name of the trusted profile that you want to update.
2. Click **Add** to add new condition to the existing trust relationship. To edit an existing condition, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Edit** next to the trust relationship you want to update.
   * Click **Add new condition** and repeat as needed to add more conditions.
   * To remove a condition, click the **Close** icon ![Close icon](../icons/close-icon.svg "Close") next to the existing condition.
3. Click **Save** to apply all added conditions to your trusted profile.
  
### Assigning access policies
{: #access-policies}

1. Click the name of the trusted profile that you want to update.
2. Click the **Access policies** tab.
3. To assign new access policies, click **Assign access**. To edit existing access policies, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Edit** next to the access policy you want to update.

   You can select your resources based on resource attributes and any combination of roles to assign.

### Updating session duration
{: #session-duration-tp}

1. Click the name of the trusted profile that you want to update.
2. In the federated users section, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the identity provider (IdP) row that you want to update.
3. In hours, add or subtract how long federated users can use this profile before their session expires.
4. Click **Save**. 

## Updating trusted profiles by using the CLI
{: #updating-tp-cli}
{: cli}

You can update a trusted profile from your account by using the CLI. For more information, see the [IBM Cloud CLI](https://github.com/IBM-Cloud/ibm-cloud-cli-release/releases). 

1. Log in, and select the account.

   ```bash
   ibmcloud login
   ```
   {: codeblock}
   
1. Check the list of trusted profiles for the current account and select the one that you want to update. The following command shows the list of trusted profiles for your {{site.data.keyword.Bluemix_notm}} account:

   ```bash
   ibmcloud iam trusted-profiles
   ```
   {: codeblock}
   
1. If you'd like to check the details of a trusted profile, use the `ibmcloud iam trusted-profile` command. Specify the ID or the name of the trusted profile that you would like to check. 

   ```bash
   ibmcloud iam trusted-profile <IDorName>
   ```
   {: codeblock}

1. Run the following command to get an overview about the different options.

   ```bash
   ibmcloud iam trusted-profile-update
   ```
   {: codeblock}
   
1. Update the trusted profile by running the following command. Specify the ID or the name of the trusted profile that you would like to update and rename. 

   ```bash
   ibmcloud iam trusted-profile-update <IDorName> -n <NewName> ... 
   ```
   {: codeblock}   
   
For example, the following command updates the name `Test trusted profile` to `New test trusted profile`. 

   ```bash
   ibmcloud iam trusted-profile-update <Test trusted profile> -n <New test trusted profile> ...
   ```
   {: codeblock}   

### Assigning access policies
{: #access-policies-cli}

You can assign new access policies to your trusted profile by using the CLI. 

1. Log in, and select the account.

   ```bash
   ibmcloud login
   ```
   {: codeblock}
   
1. Check the list of trusted profiles for the current account and select the one that you want to assign new access policies to. The following command shows the list of trusted profiles for your {{site.data.keyword.Bluemix_notm}} account:

   ```bash
   ibmcloud iam trusted-profiles
   ```
   {: codeblock}
   
1. Assign new access policies by running the following command:

   ```bash
   ibmcloud iam trusted-profile-policy-create
   ```
   {: codeblock}
    
To check the details of the access policy for a trusted profile, run the following command:

   ```bash
   ibmcloud iam trusted-profile-policy
   ```
   {: codeblock}
   
For checking the list of access policies for a trusted profile, you can use the `ibmcloud iam trusted-profile-policies` command:
   
   ```bash
   ibmcloud iam trusted-profile-policies
   ```
   {: codeblock}
   
   
You can easily update existing access policies by running the `ibmcloud iam trusted-profile-policy-update` command:
 
   ```bash
   ibmcloud iam trusted-profile-policy-update
   ```
   {: codeblock}
 
If you'd like to remove an access policy for a trusted profile, you can use the `ibmcloud iam trusted-profile-policy-delete` command:
 
   ```bash
   ibmcloud iam trusted-profile-policy-delete
   ```
   {: codeblock}

## Updating trusted profiles by using the API
{: #updating-tp-api}
{: api}

For more information, see the [IAM Identity Services API](/apidocs/iam-identity-token-api). 

### Updating the name or description
{: #update-tp-desc-api}

To update the name or description of an existing trusted profile, call the following. Enter your updated `name` and `description` attributes. 
   ```bash
   curl -X PUT 'https://iam.cloud.ibm.com/v1/profiles/PROFILE_ID' -H 'Authorization: Bearer TOKEN' -H 'If-Match: <value of etag header from GET request>' -H 'Content-Type: application/json' -H 'Accept: application/json' -d '{
     "name": "My Profile updated",
     "description": "My updated desc"
   }'
   ```
   {: codeblock}

### Updating the conditions of the trust relationship
{: #trust-api}

After the trusted profile is created, you can build trust with both federated users and compute resources in the same trusted profile.

   ```bash
   curl -X PUT 'https://iam.cloud.ibm.com/v1/profiles/PROFILE_ID/rules/CLAIM_RULE_ID' 
   -H 'Authorization: Bearer TOKEN' 
   -H 'If-Match: <value of etag header from GET request>' 
   -H 'Content-Type: application/json' 
   -H 'Accept: application/json' 
   -d '{
      "type": "Profile-SAML",
      "realm_name": "https://w3id.sso.ibm.com/auth/sps/samlidp2/saml20",
      "expiration": 10000,
      "conditions": [
     {
   "claim": "groups",
   "operator": "CONTAINS",
   "value": "\"cloud-docs-ops\""
     }
     ] 
   }'
   ```
   {: codeblock}

### Assigning access policies
{: #access-policies-api}

To assign new access policies, call the following:

   ```bash
   curl -X PUT 'https://iam.cloud.ibm.com/v1/policies' 
   -H 'Authorization: Bearer $TOKEN' 
   -H 'Content-Type: application/json' 
   -H 'If-Match: $ETAG' 
   -d '{
   "type": "access",
   "description": "Viewer role for for all instances of SERVICE_NAME in the account.",
   "subjects": [
      {
         "attributes": [
         {
            "name": "iam_id",
            "value": "IBMid-123453user"
         }
         ]
      }'
   ],
   "roles":[
      {
         "role_id": "crn:v1:bluemix:public:iam::::role:Viewer"
      }
   ],
   "resources":[
      {
         "attributes": [
         {
            "name": "accountId",
            "value": "$ACCOUNT_ID"
         },
         {
            "name": "serviceName",
            "value": "$SERVICE_NAME"
         }
         ]
      }
   ]
   }'
   ```
   {: codeblock}

For more information, see the [IAM Policy Management API](/apidocs/iam-policy-management#update-policy).

### Updating session duration
{: #session-duration-tp-api}

To update the session duration for federated users, call the following:
   ```bash
   curl -X PUT 'https://iam.cloud.ibm.com/v1/profiles/PROFILE_ID/rules/CLAIM_RULE_ID' 
   -H 'Authorization: Bearer TOKEN' 
   -H 'If-Match: <value of etag header from GET request>' 
   -H 'Content-Type: application/json' 
   -H 'Accept: application/json' 
   -d '{
      "type": "Profile-SAML",
      "realm_name": "https://w3id.sso.ibm.com/auth/sps/samlidp2/saml20",
      "expiration": 10000,
      "conditions": [
     {
   "claim": "groups",
   "operator": "CONTAINS",
   "value": "\"cloud-docs-ops\""
     }
     ] 
   }'
   ```
   {: codeblock}
   
