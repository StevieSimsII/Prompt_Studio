# Power Automate Flows for AI Prompt Studio

## Overview
This folder contains automated workflows to support the approval process and notifications for the AI Prompt Studio system.

## Required Flows

### 1. Prompt Submission Notification Flow
**Trigger**: When a new prompt is submitted (Status = Pending)  
**Actions**: Notify reviewers via email and Teams

### 2. First-to-Act Approval Flow
**Trigger**: When prompt status changes to Approved or Rejected  
**Actions**: 
- Update approval fields
- Notify submitter
- Cancel other pending approvals

### 3. Daily Summary Flow
**Trigger**: Daily schedule  
**Actions**: Send summary of pending prompts to reviewers

### 4. Prompt Analytics Flow
**Trigger**: When prompt is viewed  
**Actions**: Update view count and usage analytics

## Setup Instructions
1. Import flow templates from JSON files
2. Configure connections to SharePoint and email
3. Update recipient lists and email templates
4. Test flows with sample data
5. Enable flows in production

## Flow Configurations
Each flow includes:
- Detailed trigger conditions
- Error handling
- Logging for audit trails
- Dynamic content for personalization
