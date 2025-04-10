# Project Brief: Maestro Mapping

## Core Requirements
- Build a Ruby on Rails application to synchronize data between Maestro and external ATS systems
- Focus on accurate and automated field mapping between ATS and Maestro
- Support multi-tenant architecture

## Project Goals
1. **Automated Synchronization**
   - Pull data from ATS via scheduler, subscription events, or webhooks
   - Build a mapping between ATS and Maestro
   - Transform data bidirectionally between ATS and Maestro

2. **Intelligent Field Mapping**
   - Create accurate schema field mappings
   - Handle custom fields from ATS systems
   - Utilize AI for automated mapping in specific cases

3. **System Architecture**
   - Multi-tenant support for different customer configurations
   - Asynchronous processing using Sidekiq
   - Internal admin interface using Active Admin

## User Experience Goals
- **Internal Admin Users:**
  - Easy configuration of tenant settings
  - Clear visibility of mapping status
  - Simple management of synchronization tasks
  - Efficient troubleshooting capabilities

## Project Scope
- **In Scope:**
  - Data synchronization automation
  - Field mapping and transformation
  - Multi-tenant support
  - Queue-based processing
  - Internal admin interface

- **Out of Scope:**
  - Public end-user interface
  - Direct database modifications
  - Real-time synchronization

## Success Metrics
1. Accurate field mapping between systems
2. Reliable data transformation
3. Efficient queue processing
4. Secure handling of sensitive data
5. Maintainable and scalable architecture