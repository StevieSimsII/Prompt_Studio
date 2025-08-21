# SharePoint Security Groups Configuration

## Groups to Create

### 1. AI Prompt Studio - Users
**Purpose**: General access for all employees
**Members**: All authenticated users (Everyone except external users)
**Permissions**: 
- Read access to approved prompts only
- Submit new prompts

**PowerShell Commands**:
```powershell
# Connect to SharePoint Online
Connect-SPOService -Url https://yourtenant-admin.sharepoint.com

# Create the group
New-SPOSiteGroup -Site "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -Name "AI Prompt Studio - Users" -PermissionLevels "Read"

# Add everyone except external users
Add-SPOSiteGroupMember -Site "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -Name "AI Prompt Studio - Users" -LoginName "c:0-.f|rolemanager|spo-grid-all-users/yourtenant.onmicrosoft.com"
```

### 2. AI Prompt Studio - IT Team
**Purpose**: IT department members with contribution rights
**Members**: IT department staff
**Permissions**: 
- All user permissions
- Edit own submissions
- View submission history

**PowerShell Commands**:
```powershell
# Create the IT team group
New-SPOSiteGroup -Site "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -Name "AI Prompt Studio - IT Team" -PermissionLevels "Contribute"

# Add IT team members (example)
Add-SPOSiteGroupMember -Site "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -Name "AI Prompt Studio - IT Team" -LoginName "itteam@yourtenant.onmicrosoft.com"
```

### 3. AI Prompt Studio - Reviewers
**Purpose**: Senior IT staff who can approve/reject prompts
**Members**: IT managers, senior engineers, team leads
**Permissions**: 
- All IT team permissions
- Approve/reject prompts
- Access approval dashboard
- Bulk operations

**PowerShell Commands**:
```powershell
# Create the reviewers group
New-SPOSiteGroup -Site "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -Name "AI Prompt Studio - Reviewers" -PermissionLevels "Full Control"

# Add reviewer members
Add-SPOSiteGroupMember -Site "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -Name "AI Prompt Studio - Reviewers" -LoginName "reviewer1@yourtenant.onmicrosoft.com"
Add-SPOSiteGroupMember -Site "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -Name "AI Prompt Studio - Reviewers" -LoginName "reviewer2@yourtenant.onmicrosoft.com"
```

### 4. AI Prompt Studio - Administrators
**Purpose**: System administrators and app owners
**Members**: IT administrators, Power Platform admins
**Permissions**: 
- All permissions
- Site administration
- Power Apps/Automate management
- Security configuration

**PowerShell Commands**:
```powershell
# Add administrators to site collection administrators
Set-SPOSite -Identity "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -Owner "admin@yourtenant.onmicrosoft.com"

# Add additional administrators
Add-SPOSiteCollectionAppCatalog -Site "https://yourtenant.sharepoint.com/sites/AIPromptStudio"
```

## List-Level Permission Configuration

### AI Prompts List Custom Permissions

**Create Custom Permission Level for List Access**:
```powershell
# Create custom permission level for prompt viewers
$permissions = [Microsoft.SharePoint.Client.PermissionKind]::ViewListItems, 
               [Microsoft.SharePoint.Client.PermissionKind]::OpenItems,
               [Microsoft.SharePoint.Client.PermissionKind]::ViewVersions,
               [Microsoft.SharePoint.Client.PermissionKind]::ViewFormPages

Add-SPOSitePermissionLevel -Site "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -Name "Prompt Viewer" -Permissions $permissions
```

### Row-Level Security Implementation

**View Filter for Regular Users** (Power Apps formula):
```powerapps
Filter('AI Prompts', Status.Value = "Approved")
```

**View Filter for Reviewers** (Power Apps formula):
```powerapps
Filter('AI Prompts', 
    Status.Value = "Pending" || 
    Status.Value = "Under Review" || 
    Status.Value = "Approved"
)
```

**My Submissions Filter** (Power Apps formula):
```powerapps
Filter('AI Prompts', 
    SubmittedBy.Email = User().Email
)
```

## Azure AD Security Groups Integration

### Create Azure AD Security Groups
```powershell
# Connect to Azure AD
Connect-AzureAD

# Create IT Team security group
New-AzureADGroup -DisplayName "AI Prompt Studio - IT Team" -MailEnabled $false -SecurityEnabled $true -MailNickName "AIPromptStudioITTeam"

# Create Reviewers security group
New-AzureADGroup -DisplayName "AI Prompt Studio - Reviewers" -MailEnabled $false -SecurityEnabled $true -MailNickName "AIPromptStudioReviewers"

# Create Administrators security group
New-AzureADGroup -DisplayName "AI Prompt Studio - Administrators" -MailEnabled $false -SecurityEnabled $true -MailNickName "AIPromptStudioAdmins"
```

### Add Members to Groups
```powershell
# Get group object IDs
$itTeamGroup = Get-AzureADGroup -Filter "DisplayName eq 'AI Prompt Studio - IT Team'"
$reviewersGroup = Get-AzureADGroup -Filter "DisplayName eq 'AI Prompt Studio - Reviewers'"
$adminsGroup = Get-AzureADGroup -Filter "DisplayName eq 'AI Prompt Studio - Administrators'"

# Add members to IT Team
$itMembers = @("user1@yourtenant.onmicrosoft.com", "user2@yourtenant.onmicrosoft.com")
foreach ($member in $itMembers) {
    $user = Get-AzureADUser -Filter "UserPrincipalName eq '$member'"
    Add-AzureADGroupMember -ObjectId $itTeamGroup.ObjectId -RefObjectId $user.ObjectId
}

# Add members to Reviewers
$reviewerMembers = @("reviewer1@yourtenant.onmicrosoft.com", "reviewer2@yourtenant.onmicrosoft.com")
foreach ($member in $reviewerMembers) {
    $user = Get-AzureADUser -Filter "UserPrincipalName eq '$member'"
    Add-AzureADGroupMember -ObjectId $reviewersGroup.ObjectId -RefObjectId $user.ObjectId
}

# Add members to Administrators
$adminMembers = @("admin1@yourtenant.onmicrosoft.com", "admin2@yourtenant.onmicrosoft.com")
foreach ($member in $adminMembers) {
    $user = Get-AzureADUser -Filter "UserPrincipalName eq '$member'"
    Add-AzureADGroupMember -ObjectId $adminsGroup.ObjectId -RefObjectId $user.ObjectId
}
```

## Power Apps Security Configuration

### Environment Security Roles
```powershell
# Connect to Power Platform
Add-PowerAppsAccount

# Assign security roles
Add-PowerAppRoleAssignment -AppName "your-app-id" -RoleName "CanEdit" -PrincipalObjectId $reviewersGroup.ObjectId -PrincipalType "Group"
Add-PowerAppRoleAssignment -AppName "your-app-id" -RoleName "CanView" -PrincipalObjectId $itTeamGroup.ObjectId -PrincipalType "Group"
```

### Data Connection Security
- SharePoint connector: Use service account with appropriate permissions
- Office 365 connector: Use delegated permissions for email
- Teams connector: Use application permissions for posting

## Conditional Access Policies

### Recommended Policies
1. **Require MFA**: For all AI Prompt Studio access
2. **Device Compliance**: Ensure devices are managed
3. **Location-Based**: Restrict access from untrusted locations
4. **Risk-Based**: Additional checks for high-risk sign-ins

### Policy Configuration
```json
{
    "displayName": "AI Prompt Studio - Require MFA",
    "state": "enabled",
    "conditions": {
        "applications": {
            "includeApplications": ["AI Prompt Studio App ID"]
        },
        "users": {
            "includeUsers": ["All"]
        }
    },
    "grantControls": {
        "operator": "AND",
        "builtInControls": ["mfa"]
    }
}
```

## Monitoring and Compliance

### Audit Log Configuration
```powershell
# Enable SharePoint audit logging
Set-SPOTenant -EnableAzureADB2BIntegration $true
Set-SPOSite -Identity "https://yourtenant.sharepoint.com/sites/AIPromptStudio" -DenyAddAndCustomizePages $false
```

### Regular Security Reviews
- **Weekly**: Review new user access
- **Monthly**: Audit permission changes
- **Quarterly**: Complete access review
- **Annually**: Security policy review

## Troubleshooting Common Issues

### Access Denied Errors
1. Check user group membership
2. Verify list permissions
3. Confirm Power Apps sharing
4. Review conditional access policies

### Permission Escalation
1. Use SharePoint permission reports
2. Monitor audit logs for changes
3. Implement approval workflow for permission changes

### Performance Issues
1. Review group membership size
2. Optimize Power Apps filters
3. Consider caching strategies
4. Monitor SharePoint throttling
