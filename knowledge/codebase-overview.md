# Codebase Overview
- For responsibility, only describe main methods that need to be explained and help to me understand the main purpose of the class.
- Only describe special or complex arguments that need explanation. Basic arguments like tenant_id are self-explanatory and don't require descriptions.

## 1. Background Jobs

### 1.1 sync_job.rb
- **Responsibility**: Manages synchronization of data between ATS and Maestro
- **Highlighted Methods**:
  + `perform(tenant_id)` - Execute the synchronization process
  + `handle_error(error, context)` - Process and log errors during synchronization
  + `reschedule(tenant)` - Set up the next synchronization based on tenant settings

## 2. Mailers

### 2.1 notification_mailer.rb
- **Responsibility**: Sends email notifications related to system events and provides templates for different types of notifications
- **Highlighted Methods**:
  + `sync_completed(tenant, sync_report)` - Send notification about completed synchronization
    * `sync_report`: Hash containing synchronization report data
  + `error_notification(tenant, error_data)` - Notify administrators about system errors
    * `error_data`: Hash containing error details, stack traces, etc.
  + `weekly_summary(tenant, report_data)` - Send weekly summary of system activity
    * `report_data`: Hash containing weekly activity metrics

## 3. Models

### 3.1 tenant.rb
- **Responsibility**: Represents a customer in the system and stores configuration and settings for each customer
- **Highlighted Methods**:
  + `find_by_subdomain(subdomain)` - Find tenant based on subdomain
  + `active?` - Check if tenant is active
  + `configuration` - Access tenant configuration

## 4. Services

### 4.1 rest_bullhorn/client.rb
- **Responsibility**: Interacts with Bullhorn REST API, processes requests/responses, and manages authentication and session with Bullhorn
- **Highlighted Methods**:
  + `initialize(bullhorn_setting)` - Initialize client with credentials
    * `bullhorn_setting`: Bullhorn::Setting object containing API credentials and configuration
  + `get(endpoint, params = {})` - Perform GET request
  + `post(endpoint, params = {}, data = {})` - Perform POST request
    * `data`: Hash containing the data to send in the request body
  + `login(credentials)` - Log in to Bullhorn API
