# dbt\_cloud.yml file

The `dbt_cloud.yml` file stores the credentials that dbt tools — like the [dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md), the [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md), and more — use to authenticate with dbt platform. You can download it from dbt platform and save it locally to your `.dbt` directory.

This page covers:

* [How to download it and set up the `.dbt` directory](#download-dbt_cloudyml) for the dbt CLI or the VS Code extension
* [Update or switch projects](#update-or-switch-projects)
* [The file structure](#file-structure) and field reference
* [The companion `dbt-cloud` block](#the-dbt-cloud-block-in-dbt_projectyml) in `dbt_project.yml`

Keep this file safe

The `dbt_cloud.yml` file contains API keys. Store it securely and make sure you *do not* commit it to version control.

## Download dbt\_cloud.yml[​](#download-dbt_cloudyml "Direct link to Download dbt_cloud.yml")

<!-- -->

How you download the file depends on whether you're configuring the [dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md) or the [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md). The downloaded `dbt_cloud.yml` includes your [personal access token (PAT)](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md).

1. In dbt platform, select the project you want to work on. The project must already have a [development environment](https://docs.getdbt.com/docs/dbt-platform-environments.md#create-a-development-environment) set up.
2. Go to **Account settings** → **Your profile**, then follow the steps for your tool:

* dbt CLI: Navigate to **CLI** → **Configure Cloud authentication** and click **Download CLI configuration file**.
* dbt VS Code extension: Navigate to **VS Code Extension** → **Set up your credentials** and click **Download credentials**.

3. Move the file to your `.dbt` directory. If you don't have one yet, try one of these quick setup (one command) options:

   * Mac / Linux
   * Windows

   ```bash
   mkdir -p ~/.dbt && mv ~/Downloads/dbt_cloud.yml ~/.dbt/dbt_cloud.yml
   ```

   ```powershell
   mkdir %USERPROFILE%\.dbt 2>nul & move %USERPROFILE%\Downloads\dbt_cloud.yml %USERPROFILE%\.dbt\dbt_cloud.yml
   ```

    Manually create and move your .dbt directory

   If you've never had a `.dbt` directory, you should perform the following recommended steps to create one. If you already have a `.dbt` directory, move the `dbt_cloud.yml` file into it. Some information about the `.dbt` directory:

   * A `.dbt` directory is a hidden folder in the root of your filesystem. It's used to store your dbt configuration files. The `.` prefix is used to create a hidden folder, which means it's not visible in Finder or File Explorer by default.
   * To view hidden files and folders, press Command + Shift + G on macOS or Ctrl + Shift + G on Windows. This opens the "Go to Folder" dialog where you can search for the `.dbt` directory.

   - Create a .dbt directory
   - Move the dbt\_cloud.yml file

   1. Clone your dbt project repository locally.
   2. Use the `mkdir` command followed by the name of the folder you want to create.

   * If using macOS, add the `~` prefix to create a `.dbt` folder in the root of your filesystem:

     <!-- -->

     * macOS: `mkdir ~/.dbt`
     * Windows: `mkdir %USERPROFILE%\.dbt`

   You can move the `dbt_cloud.yml` file into the `.dbt` directory using the `mv` command or by dragging and dropping the file into the `.dbt` directory by opening the Downloads folder using the "Go to Folder" dialog and then using drag-and-drop in the UI.

   To move the file using the terminal, use the `mv/move` command. This command moves the `dbt_cloud.yml` from the `Downloads` folder to the `.dbt` folder. If your `dbt_cloud.yml` file is located elsewhere, adjust the path accordingly.

   #### Mac or Linux[​](#mac-or-linux "Direct link to Mac or Linux")

   In your command line, use the `mv` command to move your `dbt_cloud.yml` file into the `.dbt` directory. If you've just downloaded the `dbt_cloud.yml` file and it's in your Downloads folder, the command might look something like this:

   ```bash
   mv ~/Downloads/dbt_cloud.yml ~/.dbt/dbt_cloud.yml
   ```

   #### Windows[​](#windows "Direct link to Windows")

   In your command line, use the move command. Assuming your file is in the Downloads folder, the command might look like this:

   ```bash
   move %USERPROFILE%\Downloads\dbt_cloud.yml %USERPROFILE%\.dbt\dbt_cloud.yml
   ```

   If your downloaded file has a numerical suffix (for example, `dbt_cloud(2).yml`), rename it to `dbt_cloud.yml` before moving it. The dbt CLI and extension only look for the exact filename.

4. Confirm that the `project-id` in your [`dbt_project.yml` `dbt-cloud` block](#the-dbt-cloud-block-in-dbt_projectyml) matches the project you're working on. This registers and connects your tool to dbt platform and enables platform features such as Mesh and deferral.

## Update or switch projects[​](#update-or-switch-projects "Direct link to Update or switch projects")

The `dbt_cloud.yml` file is local to your machine and doesn't update automatically. Make sure to re-download it when:

* You get access to a new project and want to work on it locally
* A project is removed from your account or your project access changes
* Your token changes or is rotated
* Your account host changes, such as when your account moves regions

The file can include multiple projects from the same dbt platform account. To switch projects, update `context.active-project` to the `project-id` for the project you want to use. The active project must match one of the projects listed under `projects`.

If you work in multiple dbt platform accounts, keep a separate `dbt_cloud.yml` file for each account and move the file you want to use into your `.dbt` directory.

## File structure[​](#file-structure "Direct link to File structure")

Your `dbt_cloud.yml` file has the following structure:

dbt\_cloud.yml

```yaml
version: "1"
context:
  active-host: your_active_host       # for example, "abc123.us1.dbt.com"
  active-project: your_project_id     # for example, "123456"
projects:
  - project-name: your_project_name
    project-id: your_project_id
    account-name: your_account_name
    account-id: your_account_id
    account-host: your_account_host   # for example, "abc123.us1.dbt.com"
    token-name: your_token_name       # for example, "cloud-cli-1234"
    token-value: your_token_value

  - project-name: your_project_name
    project-id: your_project_id
    account-name: your_account_name
    account-id: your_account_id
    account-host: your_account_host
    token-name: your_token_name
    token-value: your_token_value
```

For example, if you have a Jaffle and wizard shop account, here's what your `dbt_cloud.yml` file would look like:

dbt\_cloud.yml

```yaml
version: "1"
context:
  active-host: "abc123.us1.dbt.com"
  active-project: "123456"
projects:
  - project-name: "Project 1"
    project-id: "123"
    account-name: "Jaffle and wizard shop" 
    account-id: "1"
    account-host: "abc123.us1.dbt.com"
    token-name: "cloud-cli-1091"
    token-value: "dbtu_token_value"            # this would be a longer token value
  - project-name: "Project 2"
    project-id: "456"
    account-name: "Jaffle and wizard shop"
    account-id: "1"
    account-host: "abc123.us1.dbt.com"
    token-name: "cloud-cli-1091"
    token-value: "dbtu_token_value"
  - project-name: "Project 3"
    project-id: "789"
    account-name: "Jaffle and wizard shop"
    account-id: "1"
    account-host: "abc123.us1.dbt.com"
    token-name: "cloud-cli-1091"
    token-value: "dbtu_token_value"
```

### Field reference[​](#field-reference "Direct link to Field reference")

| Field                    | Required | Description                                                                                                                                                                                                                                               |
| ------------------------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `version`                | Yes      | The schema version of the file. Currently `"1"`.                                                                                                                                                                                                          |
| `context.active-host`    | Yes      | The `account-host` to use by default. You can find it in the **Account settings** page.                                                                                                                                                                   |
| `context.active-project` | Yes      | The `project-id` of the project to use by default when running commands. Must match a `project-id` listed under `projects`.                                                                                                                               |
| `context.defer-env-id`   | No       | The environment ID to defer to for build artifacts. Optional override of the project's default deferral environment. dbt CLI only. Refer to [Configure deferral](https://docs.getdbt.com/docs/platform/about-defer.md#configure-deferral-environment-id). |
| `projects.project-name`  | Yes      | A human-readable name for the project.                                                                                                                                                                                                                    |
| `projects.project-id`    | Yes      | The dbt platform project ID. Find it in the URL when viewing your project (for example, `…/projects/123456`).                                                                                                                                             |
| `projects.account-name`  | Yes      | A human-readable name for the account.                                                                                                                                                                                                                    |
| `projects.account-id`    | Yes      | The dbt platform account ID.                                                                                                                                                                                                                              |
| `projects.account-host`  | Yes      | The host for your account, for example `cloud.getdbt.com`, `emea.dbt.com`, or your single-tenant access URL.                                                                                                                                              |
| `projects.token-name`    | Yes      | A name for the [Personal access token (PAT)](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md).                                                                                                                                                       |
| `projects.token-value`   | Yes      | The PAT value. Treat this as a secret.                                                                                                                                                                                                                    |

## The dbt-cloud block in dbt\_project.yml[​](#the-dbt-cloud-block-in-dbt_projectyml "Direct link to The dbt-cloud block in dbt_project.yml")

The `dbt-cloud` block is a companion config that lives in your project's `dbt_project.yml` file (not in `dbt_cloud.yml`). It tells the dbt CLI, the Studio IDE, and Fusion which dbt platform project your local project corresponds to.

dbt\_project.yml

```yaml
name:
version:
# Your project configs...

dbt-cloud:
  project-id: your_project_id
  defer-env-id: '123456'  # optional
```

| Field          | Required | Description                                                                                                                                                                             |
| -------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `project-id`   | Yes      | The dbt platform project ID this local project maps to. Find it in the URL when viewing your project (for example, `https://YOUR_ACCESS_URL/develop/26228/projects/123456` → `123456`). |
| `defer-env-id` | No       | The environment ID to defer to for build artifacts. Used for Fusion [auto-deferral](https://docs.getdbt.com/docs/platform/about-defer.md) and dbt CLI deferral overrides.               |

## Related docs[​](#related-docs "Direct link to Related docs")

* [dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md)
* [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [`dbt_project.yml`](https://docs.getdbt.com/reference/dbt_project.yml.md)
