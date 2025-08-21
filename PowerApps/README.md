# Power Apps Implementation Guide

## Setup Instructions

### Prerequisites
1. Power Apps license (Power Apps per app or Power Platform license)
2. SharePoint site with lists created (see SharePoint setup guide)
3. Appropriate permissions configured

### Step 1: Create New Canvas App
1. Go to [Power Apps](https://make.powerapps.com)
2. Select your environment
3. Click "Create" → "Canvas app from blank"
4. Choose tablet format (16:9) for best responsive experience
5. Name: "AI Prompt Studio"

### Step 2: Connect to Data Sources
1. In app designer, go to Data → Add data
2. Connect to SharePoint:
   - AI Prompts list
   - IT Personas list
   - Task Types list
3. Test connections by adding a gallery temporarily

### Step 3: Create App Variables
Add these global variables in App.OnStart:
```poweraps
// User context
Set(varCurrentUser, User());
Set(varUserEmail, User().Email);
Set(varIsReviewer, 
    User().Email in ["reviewer1@company.com", "reviewer2@company.com"]
);

// Navigation
Set(varCurrentScreen, "Home");

// Data collections for better performance
ClearCollect(colApprovedPrompts, 
    Filter('AI Prompts', Status.Value = "Approved")
);
ClearCollect(colTaskTypes, 
    Filter('Task Types', IsActive = true)
);
ClearCollect(colPersonas, 
    Filter('IT Personas', IsActive = true)
);

// Form variables
Set(varFormMode, FormMode.New);
Set(varSelectedPrompt, Blank());
```

### Step 4: Create Screens and Components

#### A. Home Screen Components
- **lblWelcome**: Label with text = "Welcome, " & varCurrentUser.FullName
- **galRecentPrompts**: Gallery showing latest 5 approved prompts
- **rectStatsCard**: Rectangle containers for statistics
- **btnSubmitNew**: Button to navigate to submit screen
- **btnBrowseAll**: Button to navigate to browse screen

#### B. Browse Screen Components
- **txtSearch**: Text input for search functionality
- **ddTaskType**: Dropdown connected to colTaskTypes
- **ddPersona**: Dropdown connected to colPersonas
- **galPrompts**: Gallery for displaying filtered prompts
- **btnResetFilters**: Button to clear all filters

#### C. Submit Screen Components
- **frmSubmitPrompt**: Form connected to 'AI Prompts' data source
- **txtTitle**: Text input for prompt title
- **txtPromptText**: Text input (multiline) for prompt content
- **ddTaskTypeForm**: Dropdown for task type selection
- **ddPersonaForm**: Dropdown for persona selection
- **txtDescription**: Text input for description
- **txtTags**: Text input for tags
- **btnSubmit**: Submit button
- **btnSaveDraft**: Save draft button

### Step 5: Implement Key Formulas

#### Search and Filter Logic (Browse Screen)
```powerapps
// Gallery Items formula
Filter(
    colApprovedPrompts,
    // Search in title and description
    (IsBlank(txtSearch.Text) || 
     txtSearch.Text in Title || 
     txtSearch.Text in PromptDescription) &&
    // Filter by task type
    (IsBlank(ddTaskType.Selected) || 
     TaskType.Value = ddTaskType.Selected.Title) &&
    // Filter by persona
    (IsBlank(ddPersona.Selected) || 
     ITPersona.Value = ddPersona.Selected.Title)
)
```

#### Submit Form Validation
```powerapps
// Submit button OnSelect
If(
    // Validation
    !IsBlank(txtTitle.Text) && 
    !IsBlank(txtPromptText.Text) && 
    !IsBlank(ddTaskTypeForm.Selected) && 
    !IsBlank(ddPersonaForm.Selected),
    
    // Submit
    Patch(
        'AI Prompts',
        Defaults('AI Prompts'),
        {
            Title: txtTitle.Text,
            PromptText: txtPromptText.Text,
            PromptDescription: txtDescription.Text,
            TaskType: ddTaskTypeForm.Selected,
            ITPersona: ddPersonaForm.Selected,
            SubmittedBy: {
                '@odata.type': "#Microsoft.Azure.Connectors.SharePoint.SPListExpandedUser",
                Claims: "i:0#.f|membership|" & varUserEmail,
                DisplayName: varCurrentUser.FullName,
                Email: varUserEmail
            },
            Tags: txtTags.Text,
            Status: {Value: "Pending"},
            Priority: {Value: "Medium"}
        }
    );
    
    // Show success message
    Notify("Prompt submitted successfully!", NotificationType.Success);
    
    // Reset form
    Reset(frmSubmitPrompt);
    
    // Navigate back
    Navigate(HomeScreen),
    
    // Show error
    Notify("Please fill in all required fields", NotificationType.Error)
)
```

### Step 6: Implement Navigation
Add navigation menu component with:
```powerapps
// Navigation OnSelect formula
Switch(
    Self.Text,
    "Home", Navigate(HomeScreen),
    "Browse", Navigate(BrowseScreen),
    "Submit", Navigate(SubmitScreen),
    "My Submissions", Navigate(MySubmissionsScreen),
    "Approvals", If(varIsReviewer, Navigate(ApprovalScreen))
)
```

### Step 7: Add Approval Dashboard (for reviewers)
- **galPendingPrompts**: Gallery filtered to pending prompts
- **lblPromptDetails**: Label showing selected prompt details
- **btnApprove**: Approve button
- **btnReject**: Reject button
- **txtRejectionReason**: Text input for rejection reason

#### Approval Logic
```powerapps
// Approve button OnSelect
Patch(
    'AI Prompts',
    galPendingPrompts.Selected,
    {
        Status: {Value: "Approved"},
        ApprovedBy: {
            '@odata.type': "#Microsoft.Azure.Connectors.SharePoint.SPListExpandedUser",
            Claims: "i:0#.f|membership|" & varUserEmail,
            DisplayName: varCurrentUser.FullName,
            Email: varUserEmail
        },
        ApprovedDate: Now()
    }
);

Notify("Prompt approved successfully!", NotificationType.Success);

// Refresh gallery
Refresh('AI Prompts');
```

### Step 8: Add Error Handling and Loading States
- Add loading spinners for long operations
- Implement try-catch patterns for data operations
- Add offline detection and messaging

### Step 9: Testing Checklist
- [ ] Submit new prompt
- [ ] Browse and filter prompts
- [ ] Approve/reject prompts (as reviewer)
- [ ] Search functionality
- [ ] Mobile responsiveness
- [ ] Data validation
- [ ] Error handling

### Step 10: Deployment
1. Save app with version notes
2. Publish app
3. Share with appropriate users/groups
4. Set up monitoring and analytics

## Performance Optimization Tips
1. Use collections for frequently accessed data
2. Implement pagination for large datasets
3. Use delegation-friendly formulas
4. Minimize data calls in galleries
5. Cache lookup data locally
