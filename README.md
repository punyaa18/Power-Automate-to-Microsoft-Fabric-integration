# From Flow to Fabric: Connect Power Automate to Microsoft Fabric



This guide shows how to send events from Power Automate to Microsoft Fabric using Eventstream and land them in a Lakehouse Delta table. Use this to bridge business logic (flows) with analytics (Fabric): capture user inputs, trigger insights, and power dashboards.

## Overview
- Power Automate posts events via the **Send Event** connector
- Microsoft Fabric **Eventstream** ingests those events
- Events are written to a **Lakehouse** table (Delta format)
## Prerequisites
- Access to https://app.fabric.microsoft.com and permission to create a **Workspace**, **Lakehouse**, and **Eventstream**
- Power Automate with access to the **Send Event** trigger/connector

### Workflow Flowchart
```mermaid
flowchart LR
  subgraph PA[Power Automate]
    A1[Trigger or form input] --> A2[Build JSON payload]
    A2 --> A3[Send Event action]
  end

  subgraph FAB[Microsoft Fabric]
    B1[Custom Endpoint]
    B2[Destination: Lakehouse]
    B3[Delta table]
  end

  A3 --> B1
  B1 --> B2 --> B3

  C1[Event Hub Name + SAS] -. configure .-> A3
  C1 -. authenticate .-> B1

  B3 --> D1[Power BI / analytics]
```


## Step-by-Step

### 1) Create a Power Automate Flow
- Build a simple flow that accepts an input (e.g., `Name`).
- Add the **Send Event** action to the flow.
- Keep the flow drafted; you’ll complete connections after setting up Fabric.
<img width="750" height="351" alt="image" src="https://github.com/user-attachments/assets/517945e9-cea4-401d-9ca6-17fdd439e38f" />
<img width="750" height="252" alt="image" src="https://github.com/user-attachments/assets/9f84bf27-5e7a-45bd-b05d-daa05759b163" />
<img width="750" height="216" alt="image" src="https://github.com/user-attachments/assets/43613d7f-7047-4e62-a358-4bd1898c4ac0" />

### 2) Set Up Microsoft Fabric
- In Fabric: create a **new Workspace**.
- Add a **Lakehouse** (via "+ New Item").
- Add an **Eventstream** (via **Get Data**) and select **Custom Endpoints**.
<img width="640" height="218" alt="image" src="https://github.com/user-attachments/assets/a02d1d34-e29c-4994-a48b-b957749ed67e" />
<img width="640" height="173" alt="image" src="https://github.com/user-attachments/assets/47011117-2b4d-49a6-a6c9-87b19263b3b7" />

### 3) Configure Eventstream Input & Publish
- Name your input and click **Publish**.
- After publishing, open **Details** → **SAS Key Authentication**.
- Copy the **Event Hub Name** and **Primary Connection String**.
<img width="750" height="136" alt="image" src="https://github.com/user-attachments/assets/01825e80-1b7f-4428-a19a-a70245c8c851" />
<img width="750" height="277" alt="image" src="https://github.com/user-attachments/assets/a2f5a7dc-3e1c-46ee-96ac-ce1a91adf4dc" />

### 4) Connect Power Automate to Fabric
- Back in your flow, configure **Send Event**:
  - Use your **Workspace Name** to form the connection.
  - Paste the **Primary Connection String** and create the connection.
  - Manually enter the **Event Hub Name** (it may not populate automatically).
- In the **Content** field, pass the payload you want (e.g., `{ "Name": "YourValue" }`).
<img width="768" height="633" alt="image" src="https://github.com/user-attachments/assets/df6e2d24-b911-4e9c-aafc-a9426306a657" />
<img width="750" height="539" alt="image" src="https://github.com/user-attachments/assets/27fe1f6f-b776-4473-a93b-54d9b93f1e5a" />

### 5) Set the Lakehouse Destination
- In Eventstream, choose **Lakehouse** as the destination and connect to your Workspace.
- For the table:
  - Choose **Create New** under **Delta Table**, or
  - Point to an **existing** Delta table in the Lakehouse.
- Tip: If data doesn’t land, first create the table in the Lakehouse, then reconfigure the Eventstream connection.
<img width="750" height="238" alt="image" src="https://github.com/user-attachments/assets/776e5839-98d8-449c-97c7-186d57e25a3a" />
<img width="640" height="322" alt="image" src="https://github.com/user-attachments/assets/07d1f747-a217-4c3f-83e0-dc9d0c23fda1" />

### 6) Finalize & Publish
- Complete the connection and **Publish**.
- Trigger the flow—events should now appear in the Lakehouse Delta table.
<img width="750" height="281" alt="image" src="https://github.com/user-attachments/assets/b8ee8ff6-0d89-4537-b5c7-2d3423f3ba04" />

## What’s Next
- **Advanced analytics** with notebooks
- **Real-time Power BI** dashboards
- **Machine learning** integrations

## Troubleshooting
- **Event Hub name missing**: Enter it manually when setting up the Power Automate connection.
- **No rows in Lakehouse**: Verify the table exists and the Eventstream destination points to the correct Workspace and Lakehouse.
- **Schema mismatch**: Ensure the payload keys (e.g., `Name`) match the columns in the Delta table.

