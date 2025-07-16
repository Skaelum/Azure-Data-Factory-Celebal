# Azure Data Factory Celebal

This repository contains all the artifacts needed to deploy and run the Azure Data Factory solutions for different tasks.

## 1. Repository Structure and Task Classification

| Folder                 | Contains                                      | Task Performed                                                   |
| ---------------------- | --------------------------------------------- | ---------------------------------------------------------------- |
| **linkedService**      | JSON definitions of Linked Services           | Configure connections (Storage, SQL DB, etc.)                    |
| **integrationRuntime** | JSON for Integration Runtimes                 | Define compute (Self-hosted or Azure IR) for copy/data-flow jobs |
| **dataset**            | JSON definitions of Datasets                  | Declare your data sources/sinks (path, format, schema)           |
| **dataflow**           | Mapping Data Flow definitions                 | Define transformation logic (Derived Columns, filters, joins)    |
| **pipeline**           | Pipeline JSON (control-flow)                  | Orchestrate activities (Get Metadata, ForEach, Switch, Copy, DF) |
| **trigger**            | Trigger definitions (e.g. schedules)          | Schedule pipelines (daily, tumbling-window, etc.)                |
| **factory**            | Factory-level config / ARM template fragments | Deploy entire Data Factory via ARM (factory.json, parameters)    |
| `README.md`            | Project overview                              | Documentation and high-level README                              |
| publish\_config.json   | ADF “adf\_publish” branch settings            | Controls how ADF publishes from Git to the live service          |

## 2. Prerequisites

* An Azure subscription with permissions to create Resource Groups, Storage Accounts, SQL Databases, and Data Factories.
* Azure CLI or Azure PowerShell installed and logged in.
* Access to the GitHub repository to configure integration in Azure Data Factory.

## 3. Deployment and Run Instructions

### A. Fork or Clone the Repository

```bash
git clone https://github.com/Skaelum/Azure-Data-Factory-Celebal.git
cd Azure-Data-Factory-Celebal
```

### B. Configure Git Integration in Azure Data Factory

1. In the Azure Portal, open your Data Factory instance.
2. Navigate to **Manage → Git configuration**.
3. Select **GitHub** and authorize the connection, then choose:

   * **Organization**: Skaelum
   * **Repository**: Azure-Data-Factory-Celebal
   * **Collaboration branch**: `main`
   * **Root folder**: `/`

After linking, all artifacts (linked services, datasets, dataflows, pipelines, triggers) will sync from this repository.

### C. Publish to the Live Data Factory

1. In ADF Studio, click **Publish** (top toolbar).
2. Confirm the resources to deploy and click **Publish**.

This action creates or updates Linked Services, Integration Runtimes, Datasets, Data Flows, Pipelines, and Triggers in your Data Factory.

### D. Trigger Pipeline Runs

* **Manually**:

  1. In ADF Studio under **Author**, select a pipeline (e.g., `PL_ProcessAllFiles`).
  2. Click **Debug** or **Trigger Now**.

* **Scheduled**:

  1. Go to **Manage → Triggers**.
  2. Ensure your schedule or tumbling-window triggers are **Activated**.

### E. Deploy via ARM Template (Optional)

If you prefer Infrastructure-as-Code, you can deploy using the ARM templates under the `factory` folder:

```bash
az deployment group create \
  --resource-group MyResourceGroup \
  --template-file factory/factory.json \
  --parameters factory/parameters.json
```

### F. CLI Pipeline Execution

After deployment, you can start pipelines via Azure CLI:

```bash
az datafactory pipeline create-run \
  --resource-group MyResourceGroup \
  --factory-name MyDataFactory \
  --name PL_ProcessAllFiles \
  --parameters '{"folderPath":"curated/CUST_MSTR/","fileName":"CUST_MSTR_20191112.csv"}'
```

## 4. Support

If you encounter issues or have questions, please open an issue in this GitHub repository.
