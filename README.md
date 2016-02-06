# azure-teamcity-devops
A collection of powershell scripts for easing dev/test/prod deployments of ASP.Net + Azure SQL apps.

## Assumptions
You use git flow.
You have at least two app services: one for dev, and one for test/prod.
You have sufficient permissions to create/destroy apps service deployment slots, create/destroy Azure SQL DBs, and web deploy to the slot.

## Dev:
Each git flow feature branch is deployed to an app service slot and linked to a database with a corresponding name. 
If the slot doesn't exist for the branch name, a slot and corresponding database will be created.
If the app service can't accept any more slots, the least recently used slot and database will be destroyed.
The latest production database backup will be restored to the dev database and migrated.
The site and corresponding app settings will be depoyed to the dev slot. (No swapping will occur; the site will cold start.)

## Test:
A persistent 'Test' database will be created.
Persistent slots, 'test' and 'test-stage' will be created.
The latest production database backup will be restored to the 'test' database and migrated.
The site and corresponding app settings will be depoyed to the 'test-stage' slot and swapped to the 'test' slot.

## Prod:
A persistent 'Prod' database will be created.
Persistent slots, 'production' (created by default) and 'prod-stage' will be created.
The 'prod' database and migrated.
The site and corresponding app settings will be depoyed to the 'prod-stage' slot and swapped to the 'production' slot.
