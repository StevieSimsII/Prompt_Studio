# Power Apps Design Specification

## App Overview
**Name**: AI Prompt Studio  
**Type**: Canvas App  
**Target Devices**: Desktop, Tablet, Mobile-friendly  

## App Structure

### 1. Main Navigation
- **Home Screen**: Dashboard with quick stats and recent activity
- **Browse Prompts**: View and search approved prompts
- **Submit Prompt**: Form to submit new prompts
- **My Submissions**: View user's submitted prompts
- **Approval Dashboard**: For reviewers only (conditional visibility)

### 2. Screen Specifications

#### Home Screen (`HomeScreen`)
**Purpose**: Welcome users and provide overview
**Components**:
- Welcome message with user name
- Quick stats cards:
  - Total approved prompts
  - Pending approvals (if reviewer)
  - My submissions count
- Recent activity feed
- Quick action buttons (Submit New, Browse All)

#### Browse Prompts Screen (`BrowseScreen`)
**Purpose**: View and filter approved prompts
**Components**:
- Search bar with real-time filtering
- Filter dropdowns:
  - Task Type
  - IT Persona
  - Department
- Prompt gallery with cards showing:
  - Title
  - Description preview
  - Task Type and Persona badges
  - Submitted by
  - View count
- Detailed view modal for selected prompts

#### Submit Prompt Screen (`SubmitScreen`)
**Purpose**: Form for submitting new prompts
**Components**:
- Multi-step form:
  1. Basic Info (Title, Description)
  2. Categorization (Task Type, Persona)
  3. Prompt Details (Text, Use Case, Expected Output)
  4. Tags and Additional Info
- Form validation
- Save as draft functionality
- Submit confirmation

#### My Submissions Screen (`MySubmissionsScreen`)
**Purpose**: View user's submitted prompts and their status
**Components**:
- Status filter (All, Pending, Approved, Rejected)
- List of user's prompts with status indicators
- Edit/Delete options for pending submissions
- Resubmit option for rejected prompts

#### Approval Dashboard Screen (`ApprovalScreen`)
**Purpose**: For reviewers to approve/reject prompts
**Components**:
- Pending prompts queue
- Detailed prompt review panel
- Approve/Reject buttons with reason fields
- Bulk actions for multiple selections
- Review history log

#### Prompt Details Screen (`DetailsScreen`)
**Purpose**: Full view of a single prompt
**Components**:
- Complete prompt information
- Copy to clipboard functionality
- Usage tracking
- Related prompts suggestions
- Feedback/rating system

## Design System

### Color Palette
- **Primary**: #0078D4 (Microsoft Blue)
- **Secondary**: #106EBE
- **Success**: #107C10
- **Warning**: #FFB900
- **Error**: #D13438
- **Background**: #F3F2F1
- **Surface**: #FFFFFF
- **Text Primary**: #323130
- **Text Secondary**: #605E5C

### Typography
- **Headers**: Segoe UI, Bold, 20-24px
- **Subheaders**: Segoe UI, Semibold, 16-18px
- **Body**: Segoe UI, Regular, 14px
- **Caption**: Segoe UI, Regular, 12px

### Component Specifications

#### Navigation Bar
- **Height**: 60px
- **Background**: Primary color
- **Logo**: Company logo + "AI Prompt Studio"
- **Navigation items**: Icon + text for main sections
- **User menu**: Avatar, name, settings

#### Prompt Card
- **Size**: 320px x 200px
- **Border**: 1px solid #E1DFDD
- **Border Radius**: 8px
- **Shadow**: 0 2px 4px rgba(0,0,0,0.1)
- **Hover**: Elevation increase

#### Form Elements
- **Input fields**: 40px height, border radius 4px
- **Dropdowns**: Consistent with input styling
- **Buttons**: 
  - Primary: 40px height, primary color
  - Secondary: 40px height, transparent with border
  - Icon buttons: 32px x 32px

### Responsive Design
- **Desktop**: Full multi-column layout
- **Tablet**: Adjusted column count, maintained functionality
- **Mobile**: Single column, bottom navigation, simplified UI

## User Experience Flows

### Submit Prompt Flow
1. User clicks "Submit New Prompt"
2. Multi-step form with progress indicator
3. Form validation at each step
4. Preview before final submission
5. Confirmation message with tracking info

### Browse Prompts Flow
1. User lands on browse screen
2. Can immediately see all approved prompts
3. Use filters to narrow down results
4. Click card to view full details
5. Copy prompt text or save to favorites

### Approval Flow
1. Reviewer accesses approval dashboard
2. See queue of pending prompts
3. Click to review full details
4. Make approval decision with optional notes
5. System updates status and notifies submitter

## Security & Permissions
- **All Users**: Can browse approved prompts and submit new ones
- **Reviewers**: Additional access to approval dashboard
- **Administrators**: Full access including user management
- **Data**: All sensitive operations logged for audit
