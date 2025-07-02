# Kargo RFP Automation - Implementation Guide

## 🚀 Quick Start Deployment

### Step 1: N8N Setup
1. **Import Workflow**
   ```bash
   # Upload kargo-rfp-automation-workflow.json to your N8N instance
   # Navigate to Workflows → Import from file
   ```

2. **Configure Credentials**
   - Gmail OAuth2: `https://console.developers.google.com`
   - Salesforce OAuth2: `https://[instance].lightning.force.com/setup/`
   - Slack OAuth2: `https://api.slack.com/apps`
   - Anthropic API: `https://console.anthropic.com`

### Step 2: Salesforce Configuration

#### Required Custom Fields
Create these custom fields in your Salesforce Opportunity object:

```sql
-- Campaign and Agency Information
Campaign_Type__c (Text, 255)
Agency__c (Lookup to Account)
Trading_Entity__c (Picklist: "Kargo Direct", "Kargo via Omnicom", "Kargo via Kinesso")

-- Campaign Details
RMN_Service_Type__c (Picklist: "Amazon DSP", "Sponsored Products", "Not Specified")
Tentpole_Event__c (Text, 100)
Budget_Tier__c (Picklist: "Essential", "Growth", "Premium")

-- Strategic Information
Primary_KPI__c (Text, 100)
Campaign_Objective__c (Picklist: "Brand Awareness", "Consideration", "Conversion", "Retention")
RFP_Source__c (Text, 255)

-- Workflow Management
Brainstorm_Required__c (Checkbox)
Email_Source_ID__c (Text, 255)
Priority_Level__c (Picklist: "Low", "Medium", "High")

-- Additional Fields
Target_Audience__c (Text, 255)
Creative_Mix__c (Text, 255)
Measurement_Requirements__c (Text, 255)
Competitive_Considerations__c (Text, 255)
Flight_Start__c (Date)
Flight_End__c (Date)
```

#### Account Mapping Verification
Ensure these Salesforce Account IDs exist and are correct:

```javascript
// Amazon Divisions
'0010h00001du5MCAAY' // Prime Video
'001Rl00000TRSkIIAX' // Alexa  
'001Rl00000703UyIAI' // Amazon Ads
'001Rl0000070ZRbIAM' // Amazon Business
'0010h00001bqDoyAAE' // Ring
'0010h00001i0vGdAAI' // AWS
'001Rl00000ZhPJyIAN' // Amazon Fresh
'0016Q00001wGx6eQAC' // Amazon Reputation
'0010h00001iIJvlAAG' // Amazon XCM

// Agencies
'0010h00001ZIT8BAAX' // Client Direct
'001E000000cVeb3IAC' // Kinesso
'001Rl00000exIQ5IAM' // Omnicom Media Group
```

### Step 3: Communication Setup

#### Slack Configuration
1. **Create App**: https://api.slack.com/apps
2. **OAuth Scopes**: `channels:read`, `chat:write`, `chat:write.public`
3. **Update Channel ID**: Replace `C01234567890` with your #sales-alerts channel ID
4. **Install App**: Add to workspace and copy OAuth token

#### Email Configuration
1. **SMTP Settings**: Configure corporate email server
2. **Distribution Lists**: 
   - `kargo-sales@kargo.com` (team notifications)
   - `executives@kargo.com` (high-priority alerts)
   - `sales-leadership@kargo.com` (executive escalation)

### Step 4: Security Configuration

#### Gmail Security
```javascript
// Authorized domains (update as needed)
const authorizedDomains = [
  'amazon.com',
  'kinesso.com', 
  'omnicommediagroup.com',
  'mbww.com',
  'rufusww.com'
];
```

#### API Rate Limiting
- **Gmail API**: 1 billion quota units per day
- **Salesforce API**: 15,000-200,000 calls per day (org dependent)
- **Anthropic API**: Configure rate limits in Claude node
- **Slack API**: Tier 3 (50+ requests per minute)

## 🔧 Advanced Configuration

### Claude Sonnet 4 Optimization

#### Custom Prompt Enhancement
```javascript
// Update system prompt in Advanced RFP Analyzer node
const systemPrompt = `
You are Ben Gillow, Kargo's expert sales strategist with deep knowledge of:
- Premium advertising formats and creative optimization
- Amazon ecosystem and division-specific strategies  
- Programmatic buying and retail media networks
- Competitive landscape analysis and positioning
- Budget allocation and format recommendations

Additional context for enhanced analysis:
- Recent market trends and seasonal considerations
- Kargo's unique format advantages and differentiators
- Client history and relationship management insights
- Technical feasibility and inventory considerations
`;
```

#### Analysis Framework Customization
```javascript
// Add custom analysis sections based on your needs
const customSections = [
  "## INVENTORY ANALYSIS",
  "**Available Formats:** [Current format availability]",
  "**Delivery Feasibility:** [Technical requirements assessment]", 
  "**Optimization Opportunities:** [Performance enhancement recommendations]"
];
```

### Workflow Performance Tuning

#### Parallel Processing Optimization
```javascript
// Enable parallel execution for non-dependent nodes
const parallelNodes = [
  'Team Communication Hub',
  'Slack Integration', 
  'Executive Alert'
];
```

#### Memory Management
```javascript
// Optimize large attachment processing
const attachmentLimits = {
  maxSize: 25 * 1024 * 1024, // 25MB
  allowedTypes: ['.pdf', '.docx', '.xlsx', '.pptx'],
  processingTimeout: 30000 // 30 seconds
};
```

### Error Handling Enhancement

#### Retry Logic Configuration
```javascript
const retryConfig = {
  maxRetries: 3,
  retryDelay: 2000, // 2 seconds
  exponentialBackoff: true,
  retryConditions: [
    'network_error',
    'api_rate_limit',
    'temporary_service_unavailable'
  ]
};
```

#### Monitoring & Alerting
```javascript
const monitoringConfig = {
  healthCheckInterval: 300000, // 5 minutes
  errorThreshold: 5, // errors per hour
  performanceAlerts: {
    slowExecution: 120000, // 2 minutes
    highErrorRate: 10 // 10% error rate
  }
};
```

## 📊 Testing & Validation

### Unit Testing Checklist

#### Domain Security Testing
- [ ] Authorized domain emails process correctly
- [ ] Unauthorized domains are blocked completely
- [ ] Security logging captures all attempts

#### Agency Mapping Validation  
- [ ] All domain-to-agency mappings work correctly
- [ ] Amazon division detection is accurate
- [ ] Salesforce Account IDs are valid

#### Opportunity Creation Testing
- [ ] All required fields populate correctly
- [ ] Naming convention follows Kargo standards
- [ ] Duplicate prevention works properly

#### Communication Testing
- [ ] Email templates render correctly across clients
- [ ] Slack notifications display properly with buttons
- [ ] Executive alerts trigger for high-priority RFPs

### Integration Testing

#### End-to-End Flow
1. **Send Test RFP**: Create test email from authorized domain
2. **Monitor Processing**: Track execution through all nodes
3. **Verify Salesforce**: Confirm opportunity creation with correct data
4. **Check Communications**: Validate email and Slack notifications
5. **Review Logs**: Ensure proper audit trail creation

#### Performance Testing
```javascript
// Load testing parameters
const loadTest = {
  emailVolume: 50, // emails per hour
  concurrentProcessing: 5,
  executionTimeTarget: 120000, // 2 minutes
  successRateTarget: 95 // 95% success rate
};
```

## 🎛️ Production Deployment

### Pre-Production Checklist
- [ ] All credentials configured and tested
- [ ] Salesforce custom fields created and validated
- [ ] Email and Slack integrations working
- [ ] Claude Sonnet 4 analysis producing quality output
- [ ] Error handling and logging functional
- [ ] Performance metrics within acceptable ranges

### Go-Live Process
1. **Backup Current Process**: Document manual RFP handling procedures
2. **Gradual Rollout**: Start with limited email monitoring
3. **Monitor Closely**: Watch first 24-48 hours of execution
4. **Team Training**: Brief sales team on new automated process
5. **Feedback Loop**: Collect initial feedback and optimize

### Production Monitoring

#### Daily Checks
- Execution success rate and error logs
- Salesforce opportunity quality and accuracy
- Team notification delivery and engagement
- Claude analysis quality and relevance

#### Weekly Reviews  
- Performance metrics and optimization opportunities
- New agency domain additions or changes
- Communication template effectiveness
- Salesforce field mapping accuracy

#### Monthly Optimization
- Claude prompt refinement based on results
- Workflow performance tuning
- Security audit and credential rotation
- Process improvement based on team feedback

## 🔒 Security & Compliance

### Data Protection
- **PII Handling**: Email content temporarily processed, not stored
- **API Security**: OAuth2 with token refresh automation
- **Audit Trail**: Complete processing history maintained
- **Access Control**: Role-based permissions for workflow management

### Compliance Requirements
- **GDPR**: Right to deletion, data portability
- **SOC 2**: Security controls and monitoring
- **Industry Standards**: Email security and API best practices

### Security Monitoring
```javascript
const securityMonitoring = {
  unauthorizedAccessAttempts: 'immediate_alert',
  unusualProcessingPatterns: 'daily_report',
  credentialExpirationWarnings: '7_days_advance',
  apiQuotaUsage: 'weekly_summary'
};
```

## 📞 Support & Maintenance

### Troubleshooting Guide

#### Common Issues
1. **Gmail Authentication Fails**
   - Check OAuth2 token expiration
   - Verify API quotas and limits
   - Confirm Gmail API permissions

2. **Salesforce Creation Errors**
   - Validate custom field configurations
   - Check Account ID mappings
   - Verify user permissions

3. **Claude Analysis Issues**
   - Monitor API rate limits
   - Review prompt effectiveness
   - Check model availability

4. **Communication Failures**
   - Test SMTP server connectivity
   - Verify Slack app permissions
   - Check recipient email validity

### Maintenance Schedule
- **Daily**: Monitor execution logs and error rates
- **Weekly**: Review performance metrics and optimization opportunities  
- **Monthly**: Update agency mappings and credential rotation
- **Quarterly**: Comprehensive security audit and process review

### Contact Information
- **Technical Support**: dev-team@kargo.com
- **Salesforce Admin**: salesforce-admin@kargo.com  
- **Security Issues**: security@kargo.com
- **Process Questions**: sales-ops@kargo.com

---

**Implementation Support**: For detailed implementation assistance, contact the development team with your specific N8N instance details and organizational requirements.