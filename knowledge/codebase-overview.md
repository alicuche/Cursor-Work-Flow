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

### 1.2 schema_export_job.rb
- **Responsibility**: Handles asynchronous export of schema from ATS and processes schema data in background to avoid blocking user interface
- **Highlighted Methods**:
  + `perform(tenant_code, entity_name = nil, clean_up = false)` - Execute the schema export process
    * `entity_name`: Optional - specifies which entity schema to export (exports all if nil)
    * `clean_up`: Boolean flag to determine if existing schema data should be deleted before export
  + `process_entity_schema(entity_name, schema)` - Process schema for a specific entity
  + `notify_completion(tenant)` - Send notification when schema export is complete

### 1.3 entity_data_export_job.rb
- **Responsibility**: Manages asynchronous export of entity data from ATS and processes large data sets in batches to optimize performance
- **Highlighted Methods**:
  + `perform(tenant_code, entity_name, ids)` - Execute the entity data export process
    * `ids`: Array of entity IDs to export data for
  + `process_batch(entities, batch_size)` - Handle a batch of entities for processing
    * `batch_size`: Integer specifying how many entities to process in each batch
  + `mark_completed(entity_name)` - Update status after completion

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

### 2.2 tenant_mailer.rb
- **Responsibility**: Handles tenant-specific email communications and manages templates for tenant onboarding and management
- **Highlighted Methods**:
  + `welcome(tenant)` - Send welcome email to new tenants
  + `configuration_change(tenant, changes)` - Notify about configuration changes
    * `changes`: Hash describing the configuration changes that were made
  + `subscription_update(tenant, update_info)` - Send subscription-related updates
    * `update_info`: Hash containing details about the subscription update

## 3. Models

### 3.1 tenant.rb
- **Responsibility**: Represents a customer in the system and stores configuration and settings for each customer
- **Highlighted Methods**:
  + `find_by_subdomain(subdomain)` - Find tenant based on subdomain
  + `active?` - Check if tenant is active
  + `configuration` - Access tenant configuration

### 3.2 bullhorn/setting.rb
- **Responsibility**: Stores connection configuration for Bullhorn API and provides settings for Bullhorn ATS integration
- **Highlighted Methods**:
  + `authenticate(credentials)` - Authenticate with Bullhorn API
    * `credentials`: Hash containing authentication credentials (client_id, client_secret, username, password)
  + `refresh_token` - Refresh access token
  + `valid_credentials?` - Check if credentials are valid

### 3.3 schema_field.rb
- **Responsibility**: Stores information about ATS schema fields, contains metadata for each field, and provides base data for mapping between ATS and Maestro
- **Highlighted Methods**:
  + `by_entity(entity_name)` - Filter schema fields by entity
  + `has_sample_data?` - Check if field has sample data
  + `data_type_formatted` - Return standardized data type format

### 3.4 entity.rb
- **Responsibility**: Stores information about entities from ATS (like Candidate, JobOrder) and contains raw data from ATS including entity_id and data
- **Highlighted Methods**:
  + `find_by_entity_and_id(entity_name, entity_id)` - Find entity based on entity name and entity_id
  + `update_data(data)` - Update the data field
    * `data`: Hash containing the updated entity data
  + `has_data?` - Check if entity has data

### 3.5 user.rb
- **Responsibility**: Manages user authentication and role-based access control within the multi-tenant architecture
- **Highlighted Methods**:
  + `assign_default_role` - Automatically assigns the client role to a new user
  + `has_role?(role_name, resource = nil)` - Check if user has a specific role (provided by Rolify)
  + `has_any_role?(*args)` - Check if user has any of the specified roles (provided by Rolify)
  + `add_role(role_name, resource = nil)` - Assign a role to a user (provided by Rolify)

### 3.6 role.rb
- **Responsibility**: Defines roles for role-based access control using Rolify
- **Highlighted Methods**:
  + `has_and_belongs_to_many :users` - Association to users
  + `belongs_to :resource, polymorphic: true, optional: true` - Polymorphic association to resources (e.g., tenants)
  + `scopify` - Rolify method that adds scoping methods to the Role model

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

### 4.2 rest_bullhorn/export/schema.rb
- **Responsibility**: Pulls schema fields data from Bullhorn and automatically updates schema information in database
- **Highlighted Methods**:
  + `export(options = {})` - Pull and save schema from Bullhorn
    * `options`: Hash containing export options (clean_up, entity_name, etc.)
  + `get_schema(entity_name)` - Get schema for a specific entity
  + `process_field(field_data, entity_name)` - Process field information from API response
    * `field_data`: Hash containing field data from the API

### 4.3 rest_bullhorn/export/dropdown.rb
- **Responsibility**: Pulls data for SchemaField.options_from_data field from Bullhorn and updates information about options for dropdown fields
- **Highlighted Methods**:
  + `export(entity_name)` - Pull and save dropdown options
  + `process_options(options_data, entity_name)` - Process options from API response
    * `options_data`: Array of option values from the API
  + `format_options(raw_options)` - Format options to the correct standard
    * `raw_options`: Array of unformatted option values

### 4.4 rest_bullhorn/export/fetch_id.rb
- **Responsibility**: Loops through all Entities and pulls only entity_id, and saves entity_id information to Entity model without data field
- **Highlighted Methods**:
  + `execute(entity_name)` - Execute the ID fetching process
  + `fetch_ids_for_entity(entity_name, number_records: nil, raise_error: false, clean_up: false, c_where: nil)` - Get IDs for a specific entity
    * `number_records`: Optional integer limit on the number of records to fetch
    * `raise_error`: Boolean determining whether to raise errors or handle them silently
    * `clean_up`: Boolean flag to delete existing records before fetching
    * `c_where`: Optional string containing a WHERE clause for filtering results
  + `create_entity_records(entity_name, ids)` - Create records from fetched IDs

### 4.5 rest_bullhorn/export/fetch_entity.rb
- **Responsibility**: Pulls full data from Bullhorn based on entity_id and updates Entity.data field with complete data
- **Highlighted Methods**:
  + `execute(entity_name)` - Execute the entity data fetching process
  + `fetch_entity_by_id(entity_name, entity_id)` - Get data for a specific entity_id
  + `update_entity_data(entity, data)` - Update data for existing entity
    * `data`: Hash containing the data to store in the entity