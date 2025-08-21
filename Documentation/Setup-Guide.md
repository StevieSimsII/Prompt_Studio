# AI Prompt Studio - Complete Setup Guide

## Prerequisites
- Microsoft 365 subscription with Power Platform licensing
- SharePoint site collection admin rights
- Power Apps per app or Power Platform license
- Power Automate license for approval workflows

## Phase 1: SharePoint Foundation (30 minutes)

### Step 1: Create SharePoint Site
1. Go to SharePoint Admin Center
2. Create new site collection: "AI Prompt Studio"
3. Choose team site template
4. Set primary administrator

### Step 2: Create Lists Using Schemas
Navigate to your SharePoint site and create these lists using the provided schemas:

**A. IT Personas List**
```
Site Contents → New → List → From CSV
```
- Use: `SharePoint/List-Schemas/IT-Personas-Schema.json`
- Import sample data: `SharePoint/Sample-Data/IT-Personas-Sample.json`

**B. Task Types List**
- Use: `SharePoint/List-Schemas/Task-Types-Schema.json`
- Import sample data: `SharePoint/Sample-Data/Task-Types-Sample.json`

**C. AI Prompts List**
- Use: `SharePoint/List-Schemas/AI-Prompts-Schema.json`
- Import sample data: `SharePoint/Sample-Data/AI-Prompts-Sample.json`

### Step 3: Configure List Settings
For each list:
1. Enable versioning
2. Configure content approval (AI Prompts only)
3. Set up list validation rules
4. Create custom views per schema specifications

## Phase 2: Security Configuration (45 minutes)

### Step 1: Create Security Groups
Follow: `Security/Permissions/SharePoint-Security-Groups.md`

**Create these SharePoint groups:**
- AI Prompt Studio - Users (Read)
- AI Prompt Studio - IT Team (Contribute)
- AI Prompt Studio - Reviewers (Full Control)
- AI Prompt Studio - Administrators (Site Admin)

### Step 2: Configure Permissions
1. Break permission inheritance on AI Prompts list
2. Assign groups to appropriate permission levels
3. Test access with different user accounts

### Step 3: Set Up Audit Logging
```powershell
# Enable audit logging
Set-SPOSite -Identity "YOUR_SITE_URL" -EnableAudit $true
```

## Phase 3: Power Apps Development (2-3 hours)

### Step 1: Create Canvas App
1. Go to [make.powerapps.com](https://make.powerapps.com)
2. Select your environment
3. Create → Canvas app from blank
4. Choose Tablet format (16:9)
5. Name: "AI Prompt Studio"

### Step 2: Connect Data Sources
1. Add SharePoint connector
2. Connect to all three lists
3. Test connections

### Step 3: Build App Screens
Follow: `PowerApps/README.md`

**Create these screens:**
- Home Screen (Dashboard)
- Browse Prompts Screen
- Submit Prompt Screen
- My Submissions Screen
- Approval Dashboard Screen (Reviewers only)
- Prompt Details Screen

### Step 4: Implement Key Features
```powerapps
// Global Variables (App.OnStart)
Set(varCurrentUser, User());
Set(varUserEmail, User().Email);
Set(varIsReviewer, 
    User().Email in ["reviewer1@company.com", "reviewer2@company.com"]
);

// Load reference data
ClearCollect(colTaskTypes, Filter('Task Types', IsActive = true));
ClearCollect(colPersonas, Filter('IT Personas', IsActive = true));
```

### Step 5: Test App Functionality
- [ ] Submit new prompt
- [ ] Browse and filter prompts
- [ ] View prompt details
- [ ] Copy prompt text
- [ ] Approve/reject (as reviewer)

## Phase 4: Power Automate Workflows (1-2 hours)

### Step 1: Import Flow Templates
Import these JSON templates from `PowerAutomate/Flows/`:
1. `Prompt-Submission-Notification.json`
2. `Approval-Decision-Notification.json`
3. `Daily-Summary-Report.json`

### Step 2: Configure Connections
For each flow:
1. Update SharePoint site URL
2. Configure Office 365 email connection
3. Set up Teams notification (optional)
4. Update recipient email addresses

### Step 3: Update Email Templates
Customize email content in each flow:
- Replace placeholder URLs with actual app URLs
- Update company branding
- Modify notification frequency

### Step 4: Test Workflows
1. Submit test prompt
2. Verify notification emails
3. Test approval process
4. Check daily summary

## Phase 5: Deployment & Go-Live (30 minutes)

### Step 1: Final Testing
- [ ] End-to-end prompt submission
- [ ] Approval workflow
- [ ] Email notifications
- [ ] Mobile responsiveness
- [ ] Security permissions

### Step 2: App Publishing
1. Save app with version notes
2. Publish app
3. Create app icon and description
4. Share with pilot user group

### Step 3: User Communication
1. Send launch announcement
2. Schedule training sessions
3. Create quick start guide
4. Set up support channel

## Phase 6: Monitoring & Maintenance

### Daily Tasks
- Monitor approval queue
- Check error logs
- Review user feedback

### Weekly Tasks
- Review usage analytics
- Check system performance
- Update documentation

### Monthly Tasks
- Security permission review
- Content quality assessment
- User satisfaction survey

## Success Metrics

### Usage Metrics
- Number of active users
- Prompts submitted per week
- Approval turnaround time
- User satisfaction score

### Quality Metrics
- Approval rate
- Prompt reuse frequency
- User feedback ratings
- Time to find relevant prompts

## Troubleshooting Common Issues

### App Won't Load
1. Check SharePoint permissions
2. Verify data connections
3. Review Power Apps licensing
4. Check environment settings

### Users Can't Submit Prompts
1. Confirm Contribute permissions
2. Check form validation logic
3. Verify SharePoint list settings
4. Test with different browsers

### Approval Emails Not Sending
1. Check Flow run history
2. Verify email connection
3. Review SharePoint triggers
4. Test with manual flow run

### Performance Issues
1. Optimize data queries
2. Implement caching
3. Review delegation warnings
4. Consider data archiving

## Support and Maintenance

### Support Contacts
- **Technical Issues**: IT Helpdesk
- **App Questions**: Power Platform Team
- **Content Issues**: IT Knowledge Management

### Backup Strategy
- Export app package weekly
- Backup SharePoint data monthly
- Document configuration changes
- Maintain emergency contacts

### Future Enhancements
- Integration with other AI tools
- Advanced analytics dashboard
- Mobile app optimization
- Voice-to-text submission
- AI-powered prompt suggestions

## Conclusion
Your AI Prompt Studio is now ready to help your IT organization collaborate more effectively with AI! The system provides a structured way to share, discover, and improve AI prompts while maintaining quality through an approval process.

Remember to:
- Monitor usage and gather feedback
- Keep content fresh and relevant
- Maintain security and permissions
- Plan for scaling as adoption grows

For additional support, refer to the detailed documentation in each project folder.
