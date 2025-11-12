# Setting Up dbt project in Snowflake
## Prerequisite:
* Access to create dbt project in snowflake
* Access to this git repo
* Create a git classic token which will be used to connect to this repo

### Step 1: Create Git Secret
First, create a secret object to securely store your GitHub credentials. This should be created in a specific database and schema namespace.
```
USE DATABASE {{your_database}};
USE SCHEMA {{your_schema}};

CREATE OR REPLACE SECRET {{your_git_secret}}
  TYPE = password
  USERNAME = '{{your_github_username}}'
  PASSWORD = '{{your_github_personal_access_token}}';
```
### Step 2: Create API Integration
Create an API integration object that defines which Git repositories Snowflake can access.
```
CREATE OR REPLACE API INTEGRATION {{your_git_api_integration}}
  API_PROVIDER = git_https_api
  API_ALLOWED_PREFIXES = ('https://github.com/{{your_github_org}}/')
  ALLOWED_AUTHENTICATION_SECRETS = ('{{your_git_secret}}') -- ('dara_git_secret')
  ENABLED = TRUE;
```
### Step 3: Create Git Repository Object
Link Snowflake to your specific Git repository.
```
CREATE OR REPLACE GIT REPOSITORY {{your_git_repository}}
  API_INTEGRATION = {{your_git_api_integration}}
  ORIGIN = 'https://github.com/{{your_github_org}}/{{your_repo_name}}'
  GIT_CREDENTIALS = {{your_git_secret};
```
### Step 4: Create Workspace from Git Integration
Now that the Git integration is set up, you can create a workspace in Snowflake:

#### Navigate to Workspaces
In Snowsight (Snowflake's web interface), click on "Projects" in the left navigation menu
Select "Workspaces"

#### Create New Workspace
* Click the "+ Workspace" button
* Select "Create from Git Repository"
<img width="545" height="579" alt="image" src="https://github.com/user-attachments/assets/99b70b41-2a13-496f-ba0a-946130f81168" />

#### Configure Workspace

* Repository: Select {{your_git_repository}} from the dropdown
* Branch: Choose the branch you want to work with (typically main or master)
* Workspace Name: Give your workspace a descriptive name

#### Initialize Workspace

* Click "Create" to initialize your workspace
* Snowflake will clone the repository and set up your development environment

# dbt Project
* Please update `project.yml` according to your need.
