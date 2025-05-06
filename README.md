# Monitoring a Data Warehouse in Microsoft Fabric 

## Overview
This lab demonstrates how to monitor a data warehouse in Microsoft Fabric using Dynamic Management Views (DMVs) and Query Insights.

## Prerequisites
- Microsoft Fabric trial account
- Web browser access

## Lab Steps

### 1. Create a Workspace
1. Navigate to [Microsoft Fabric home page](https://app.fabric.microsoft.com)
2. Select "Workspaces" from the left menu (🗇 icon)
3. Create a new workspace with your preferred name

### 2. Create Sample Data Warehouse
1. Select "Create" from the left menu
2. Choose "Sample warehouse" under Data Warehouse section
3. Create new data warehouse named `sample-dw`

### 3. Explore Dynamic Management Views
1. Run queries against system DMVs:
 
   ```
   sql
   
   -- View active connections
   
   SELECT * FROM sys.dm_exec_connections;
   
   -- View active sessions
   
   SELECT * FROM sys.dm_exec_sessions;
   
   -- View active requests
   
   SELECT * FROM sys.dm_exec_requests;
   
   -- Combined view of running queries
   
   SELECT connections.connection_id,
   
     sessions.session_id, sessions.login_name, sessions.login_time,
   
     requests.command, requests.start_time, requests.total_elapsed_time
   
   FROM sys.dm_exec_connections AS connections
   
   INNER JOIN sys.dm_exec_sessions AS sessions
   
       ON connections.session_id=sessions.session_id
   
   INNER JOIN sys.dm_exec_requests AS requests
   
       ON requests.session_id = sessions.session_id
   
   WHERE requests.status = 'running'
   
       AND requests.database_id = DB_ID()
   
   ORDER BY requests.total_elapsed_time DESC;
   
   ```

### 4. Explore Query Insights
1. Query the query insights views:
 
   ```
   sql
   
   -- Query execution history
   
   SELECT * FROM queryinsights.exec_requests_history;
   
   -- Frequently run queries
   
   SELECT * FROM queryinsights.frequently_run_queries;
   
   -- Long running queries
   
   SELECT * FROM queryinsights.long_running_queries;
   
   ```

### 5. Clean Up Resources
1. To delete the workspace:
   - Navigate to workspace settings
     
   - Select "Remove this workspace"

## Key Learnings
- Monitoring active connections and sessions using DMVs
  
- Analyzing query performance with Query Insights
  
- Identifying long-running and frequently executed queries
  
- Practical experience with Microsoft Fabric monitoring capabilities

