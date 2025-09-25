# Red Hat Automotive Development Platform

## Overview

TBD

## Groups, Roles and Permissions

### Groups

#### Default platform groups:
cluster_admin_group: "cluster-admins"  
platform_admin_group: "platform-admins"  
platform_user_group: "platform-users"  

#### Default jumpstarter groups:
jumpstarter_admin_group: "jumpstarter-admins"  
jumpstarter_user_group: "jumpstarter-users"  

### Group membership

platform_user_group
- platform_admin_group
    - cluster_admin_group
        - user:default_admin_user
- cluster_admin_group
    - user:default_admin_user

jumpstarter_user_group
- jumpstarter_admin_group
    - cluster_admin_group
    - platform_admin_group

### Roles

platform_user_group
- "basic-user"
- "cluster-status"
- "cluster-reader"
- "self-provisioner"
- "view"

This is the basic setup to allow non-admin users to create projects and resources.

### Role bindings

cluster_admin_group
