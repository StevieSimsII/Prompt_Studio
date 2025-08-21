# AI Prompt Studio - Power App Solution

## Overview
A comprehensive Power Platform solution for IT organizations to submit, organize, and approve custom AI prompts for use with M365 Copilot and other Large Language Models.

## 🎬 Live Demo

![AI Prompt Studio Demo](Prompt_Studio_Demo.gif)

*Interactive demonstration showcasing the complete AI Prompt Studio interface - from browsing prompts to submission and approval workflows*

## Features
- **Prompt Submission**: Users can submit custom AI prompts with categorization
- **Organization**: Prompts organized by Task Type and IT Persona
- **Approval Workflow**: First-to-act approval/rejection system
- **Browse & Search**: View approved prompts with filtering capabilities

## Architecture
- **Data Layer**: SharePoint Lists
- **App Layer**: Power Apps Canvas App
- **Automation Layer**: Power Automate Flows
- **Security**: Role-based permissions

## Project Structure
```
├── Documentation/
│   ├── Setup-Guide.md
│   ├── User-Manual.md
│   └── Admin-Guide.md
├── SharePoint/
│   ├── List-Schemas/
│   └── Sample-Data/
├── PowerApps/
│   ├── App-Design/
│   └── Components/
├── PowerAutomate/
│   ├── Flows/
│   └── Templates/
└── Security/
    └── Permissions/
```

## Quick Start
1. [Set up SharePoint Lists](./SharePoint/README.md)
2. [Deploy Power App](./PowerApps/README.md)
3. [Configure Power Automate Flows](./PowerAutomate/README.md)
4. [Set up Security](./Security/README.md)

## Support
For issues and questions, refer to the documentation in each folder.
