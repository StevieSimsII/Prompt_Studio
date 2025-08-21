# SharePoint Lists Setup Guide

## Overview
This guide will help you create the necessary SharePoint lists for the AI Prompt Studio solution.

## Required Lists

### 1. IT Personas List
### 2. Task Types List  
### 3. AI Prompts List

## Setup Instructions

### Step 1: Create Site Collection
1. Navigate to SharePoint Admin Center
2. Create a new site collection or use existing team site
3. Recommended name: "AI Prompt Studio"

### Step 2: Create Lists
Follow the schemas in the `List-Schemas/` folder to create each list with the correct columns and settings.

### Step 3: Configure Permissions
- All authenticated users: Read access to view prompts
- IT Team members: Contribute access to submit prompts
- IT Approval Team: Full control to approve/reject

### Step 4: Add Sample Data
Use the sample data files in `Sample-Data/` folder to populate your lists.

## Next Steps
After creating the lists, proceed to Power Apps setup.
