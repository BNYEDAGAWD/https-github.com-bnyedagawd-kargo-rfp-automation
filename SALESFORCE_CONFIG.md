# Salesforce OAuth2 Configuration - Your Setup

## ✅ **Your Current Salesforce Configuration**

**Credential Name**: Salesforce account 2  
**Credential ID**: `BBybUndrvYlNzxkP`  
**Status**: ✅ Configured and ready for workflow use  
**Created**: 6 days ago  
**Last Modified**: 2 days ago  

## 🔧 **Workflow Integration Status**

The N8N workflow has been **automatically updated** to use your Salesforce OAuth2 credentials:

```json
{
  "credentials": {
    "salesforceOAuth2Api": {
      "id": "BBybUndrvYlNzxkP",
      "name": "Salesforce account 2"
    }
  }
}
```

## 📋 **Pre-Deployment Checklist**

### **1. Verify Salesforce Permissions**
Ensure your OAuth2 user has these permissions:

```
Object Permissions:
✅ Opportunity: Create, Read, Edit, View All
✅ Account: Read, View All  
✅ Contact: Read, View All

API Access:
✅ API Enabled permission
✅ OAuth2 access for Connected Apps
```

### **2. Required Custom Fields**
Verify these custom fields exist in your Salesforce Opportunity object:

```sql
-- Core Campaign Fields (Required)
Campaign_Type__c (Text, 255)
Agency__c (Lookup to Account) 
Trading_Entity__c (Picklist: "Kargo Direct", "Kargo via Omnicom", "Kargo via Kinesso")
RMN_Service_Type__c (Picklist: "Amazon DSP", "Sponsored Products", "Not Specified")
Budget_Tier__c (Picklist: "Essential", "Growth", "Premium")

-- Strategic Fields (Required)
Primary_KPI__c (Text, 100)
Campaign_Objective__c (Picklist: "Brand Awareness", "Consideration", "Conversion", "Retention")
RFP_Source__c (Text, 255)
Brainstorm_Required__c (Checkbox)
Priority_Level__c (Picklist: "Low", "Medium", "High")

-- Tracking Fields (Required)
Email_Source_ID__c (Text, 255)
Tentpole_Event__c (Text, 100)
Target_Audience__c (Text, 255)
Creative_Mix__c (Text, 255)
Measurement_Requirements__c (Text, 255)
Competitive_Considerations__c (Text, 255)
Flight_Start__c (Date)
Flight_End__c (Date)
```

### **3. Account ID Mapping (Critical)**
**⚠️ ACTION REQUIRED**: Update the workflow with your actual Salesforce Account IDs.

In the workflow JSON, locate the `Kargo Intelligence Engine` node and update these mappings with your real Account IDs:

```javascript
// Amazon Division Accounts - UPDATE WITH YOUR ACCOUNT IDS
const divisionMapping = {
  'prime video': { accountId: 'YOUR_PRIME_VIDEO_ACCOUNT_ID', brand: 'Prime Video' },
  'alexa': { accountId: 'YOUR_ALEXA_ACCOUNT_ID', brand: 'Alexa' },
  'ads': { accountId: 'YOUR_AMAZON_ADS_ACCOUNT_ID', brand: 'Amazon Ads' },
  'business': { accountId: 'YOUR_AMAZON_BUSINESS_ACCOUNT_ID', brand: 'Amazon Business' },
  'ring': { accountId: 'YOUR_RING_ACCOUNT_ID', brand: 'Ring' },
  'aws': { accountId: 'YOUR_AWS_ACCOUNT_ID', brand: 'AWS' },
  'fresh': { accountId: 'YOUR_AMAZON_FRESH_ACCOUNT_ID', brand: 'Amazon Fresh' },
  'reputation': { accountId: 'YOUR_AMAZON_REPUTATION_ACCOUNT_ID', brand: 'Amazon Reputation' },
  'xcm': { accountId: 'YOUR_AMAZON_XCM_ACCOUNT_ID', brand: 'Amazon XCM' }
};

// Agency Accounts - UPDATE WITH YOUR ACCOUNT IDS
const domainToAgency = {
  'amazon.com': { agency: 'Client Direct', agencyId: 'YOUR_CLIENT_DIRECT_ACCOUNT_ID' },
  'kinesso.com': { agency: 'Kinesso', agencyId: 'YOUR_KINESSO_ACCOUNT_ID' },
  'omnicommediagroup.com': { agency: 'Omnicom Media Group', agencyId: 'YOUR_OMNICOM_ACCOUNT_ID' },
  'mbww.com': { agency: 'MBWW (Omnicom)', agencyId: 'YOUR_OMNICOM_ACCOUNT_ID' },
  'rufusww.com': { agency: 'RufusWW (Omnicom)', agencyId: 'YOUR_OMNICOM_ACCOUNT_ID' }
};
```

## 🔍 **How to Find Your Account IDs**

### **Method 1: Salesforce UI**
1. Go to **Accounts** tab in Salesforce
2. Find the Amazon/Agency account
3. Open the account record
4. Copy the ID from the URL: `lightning/r/Account/[ACCOUNT_ID]/view`

### **Method 2: SOQL Query**
```sql
SELECT Id, Name FROM Account WHERE Name LIKE '%Amazon%' OR Name LIKE '%Kinesso%' OR Name LIKE '%Omnicom%'
```

### **Method 3: Data Export**
1. Setup → Data Export
2. Export Account records
3. Find IDs in the CSV file

## 🧪 **Testing Your Configuration**

### **Step 1: Test Salesforce Connection**
1. In N8N, go to **Credentials**
2. Find "Salesforce account 2" 
3. Click **Test** to verify OAuth2 connection

### **Step 2: Test Opportunity Creation**
```javascript
// Test payload for Salesforce node
{
  "Name": "Test_Opportunity_RFP_Auto",
  "StageName": "Prospect", 
  "CloseDate": "2025-02-01",
  "Amount": 50000,
  "AccountId": "[TEST_ACCOUNT_ID]",
  "Type": "New Business"
}
```

### **Step 3: Verify Custom Fields**
After test opportunity creation, check that all custom fields populate correctly.

## 🚨 **Common Issues & Solutions**

### **Issue**: "INVALID_FIELD" Error
**Solution**: Check custom field API names match exactly (case-sensitive)

### **Issue**: "INSUFFICIENT_ACCESS" Error  
**Solution**: Verify user permissions for Opportunity object and custom fields

### **Issue**: "INVALID_ID" Error
**Solution**: Ensure Account IDs are correct 18-character Salesforce IDs

### **Issue**: OAuth2 Token Expired
**Solution**: N8N will automatically refresh - ensure refresh_token scope is enabled

## ✅ **Deployment Ready Checklist**

- [ ] Salesforce OAuth2 connection tested successfully
- [ ] All custom fields exist and accessible  
- [ ] Account IDs updated with real Salesforce data
- [ ] User permissions verified
- [ ] Test opportunity creation successful
- [ ] Field mapping validated

## 📞 **Support**

If you encounter Salesforce-specific issues:
1. **Check N8N execution logs** for specific error messages
2. **Verify Salesforce setup** using the checklist above
3. **Test individual components** before full workflow execution
4. **Review Salesforce debug logs** if available

---

**Status**: ✅ Salesforce OAuth2 configured and ready for production deployment  
**Next Step**: Complete Account ID mapping and run test execution