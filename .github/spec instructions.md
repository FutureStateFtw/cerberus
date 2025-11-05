# Goal
Generate comprehensive specifications for Cerberus, a Rights Management application.  FutureState has several applications, and we need to track per user, which applications they have what rights for.  Typical roles for each application is 'user' and 'admin' 'docReader', 'metrics', but we need to have flexibility to add roles per application (not across all applications)

# Deliverables
- Complete vue.js front end application Specification Document
- .Net server functions (Azure) Specification Document
- MSSQL RDS Database Tables Specification Document

# Application Overview
The application presents two major ways to view information, either by drilling in by application and seeing who is listed and their rights.  Or the other way is to drill in by person and see the applications they are given rights to.

# Important Business Rules
- Application names must be unique across the system
- Roles do not include or assume other roles (a person may need both 'admin' and 'user' roles assigned separately)
- Roles are not editable; to change a role, delete it and add a new one
- When a new application is created, three default roles are automatically created: 'user', 'admin', and 'authorizer'
- The 'authorizer' role is required to add more users to an application
- Permissions for the Cerberus application itself are managed within Cerberus (it manages its own access)
- Only person_id needs to be stored; full person details are retrieved from an external directory
- Deletes are hard deletes (records are permanently removed)
- When adding a user role, the administrator must provide a reason/justification
- User role assignments must track: when added, who added them, and why (justification)
- Add Application and Add Role requires the Cerberus admin role

# User Stories
- Application Administrator logs into Cerberus and is shown a list of available applications to manage, and an Add Application button is visible.  There is also a way to switch this view to be user_centric rather than application_centric..  He clicks the Add Application to add a new Application to manage called CardPulse.  A v-dialog form comes up and he enters the applications name, adds the description of the application. He clicks save and the application is shown in the list of managed applications. When the application is created, three default roles are automatically created: 'user', 'admin', and 'authorizor'.

He clicks the application and is shown a new view that has the name, description, permissions description, and the list of users and their roles (which is empty). 

He then clicks Add to add a user to CardPulse.  A dialog popus up add using the personSearch.vue component a person is selected.  This component returns a person_id and name.  A v-select on the form allows the Administrator to select the role to be added.  When the role is selected, a description for what the role does is shown.  A text field is displayed for the Administrator to enter a justification for why this user is being granted this role.  Clicking Save adds this person with their role and justification to the application's user roles.

The Administrator needs a new role for the CardPulse App.  He clicks CardPulse from the list of managed applications.  Near the Add User button is the Add Role button.  Clicking this pops up a form and the Administrator can add a new role (a string value).  And a description for what the Role is used for in this one application.  He clicks Save.  Now when he clicks Add User, in the Add User form, the new role he created will be available.

- When switching to user-centric view, the Administrator first uses the personSearch.vue component to find a person.  Once a person is selected, the view shows all applications that person has access to and their roles for each application.

# Objects (JSON examples of data that might come from the APIs)
- Application
    {
        "id": 1,
        "name": "Roll Call",
        "description": "App for recording a persons presence at an event.  Includes a raffle function for events as well.  Ferpa Certification required to add students to event.",
    },

- Individual User Role
    {
        "userRoleId": 2, (serial PK)
        "applicationId": 1, (link to Application)
        "role": "admin",   
        "personId": "11502045",
        "justification": "Needs admin access to manage event configurations",
        "addedBy": "10045678",
        "addedAt": "2025-01-15T14:30:00Z"
    },  

- Application Roles (applicationId + role is unique)
    { 
        "applicationId": 1,
        "role": "admin",   
        "description": "Opens all aspects of the application"
    }

# Person Search (and existing component)
Person Search allows partial string entry of a name and returns a user object containing these properties
{
  personId: "123456",
  nameGiven: "John",
  nameMiddle: "A",
  nameFamily: "Doe",
  namePreferred: "Johnny",
  title: "IT Director" || null
  primaryAffiliation: "student",
  email: "john.doe@example.edu",
  active: true,
  username: "jdoe",
  addressStreet: "123 College Ave",
  addressCity: "Springfield",
  addressRegion: "IL",
  addressPostalCode: "62704",
  addressCountry: "USA",
  affiliations: {
    // Example structure
    student: true,
    employee: false
  },
  attributes: {  },
  campus: "Main",
  academicCareer: "Undergraduate",
  academicLevel: "Junior",
  academicMajor: "Computer Science",
  unitDisplay: "Engineering Department",
  departmentCode: "ENG",
  fte: 1.0000,
  jobClass: null,
  phone: "555-123-4567",
  primaryDeptName: "Computer Science",
  program: "B.S. Computer Science",
  creditHours: 15,
  enrollmentStatus: "Enrolled",
  termCode: "2025FA",
  createdAt: "2025-10-07T17:00:00Z",
  updatedAt: "2025-10-07T17:00:00Z"
}

# API Specification Requirements
The specification need only be concerned with naming the endpoints (i.e /applications) and the request body or string parameters, and the return bodies that come out of them.  The actual code will be written by another process.

Create Specifications for each endpoint needed for the Cerberus Application, specify their parameters, body schema and return schemas (success + error)

parameters and bodies will use camelCase for property names

# Database Specification
- Specify the tables needed for the MSSQL RDS database in a single .dbml file
- Use lowercase snake case for all column names
- use 'id' for the id of the table when needed
- use '<table>_id' for foriegn key relationships back to a parent table
- Use typical normalization

# Front end libraries
- vue.js
- vuetify
- vue router
- pinia
- do not use linter or typescript
- a template app with SSO and these libraries will be provided prior to further construction


