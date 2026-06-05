# Salesforce CLI (`sf`) Command Reference Guide

Welcome to the comprehensive reference guide for the Salesforce CLI (`sf`). This guide covers the unified `sf` commands (Salesforce CLI v2) which replaced the older `sfdx` syntax. The structure below categorizes commands by functional area, providing clear explanations, typical use cases, syntax, and essential flags.

---

## Table of Contents
1. [Global Flags & Configuration](#1-global-flags--configuration)
2. [Authentication (`sf auth`)](#2-authentication-sf-auth)
3. [Configuration & Aliasing (`sf config` & `sf alias`)](#3-configuration--aliasing-sf-config--sf-alias)
4. [Org Management (`sf org`)](#4-org-management-sf-org)
5. [Project Management & Metadata (`sf project`)](#5-project-management--metadata-sf-project)
6. [Apex & Testing (`sf apex`)](#6-apex--testing-sf-apex)
7. [Data Operations (`sf data`)](#7-data-operations-sf-data)
8. [Packaging (`sf package`)](#8-packaging-sf-package)
9. [CLI Maintenance & Troubleshooting](#9-cli-maintenance--troubleshooting)

---

## 1. Global Flags & Configuration

Every `sf` command supports a set of global flags that alter how the command executes or formats its output.

### Global Flags
* `-h | --help`: Displays help text, subcommands, and flags for any command.
* `--json`: Formats the terminal output as a structured JSON object instead of human-readable text. Ideal for CI/CD scripting.
* `--loglevel`: Sets the logging level for the command (`trace`, `debug`, `info`, `warn`, `error`, `fatal`).

---

## 2. Authentication (`sf auth`)

Before interacting with any Salesforce org, you must authenticate. These commands handle logging in, viewing active sessions, and logging out.

### `sf auth web login`
* **Explanation**: Opens a browser window to log into a Salesforce org using the standard OAuth web server flow.
* **Use Case**: Authorizing your local development environment to connect to a Sandbox, Production, or Developer Edition org.
* **Example**:
    ```bash
    sf auth web login --instance-url https://test.salesforce.com --set-default --alias my-sandbox
    ```
* **Key Flags**:
    * `-r | --instance-url`: Specify the login URL (e.g., `https://login.salesforce.com` or `https://test.salesforce.com`).
    * `-a | --alias`: Assign a memorable name to this org.
    * `-d | --set-default`: Sets this org as the default for all subsequent commands.

### `sf auth jwt grant`
* **Explanation**: Authenticates an org using a JSON Web Token (JWT) flow, requiring a connected app, consumer key, and a digital certificate.
* **Use Case**: Non-interactive environments like CI/CD pipelines (GitHub Actions, GitLab CI, Jenkins) where a browser cannot be opened.
* **Example**:
    ```bash
    sf auth jwt grant --client-id <CONSUMER_KEY> --jwt-key-file assets/server.key --username devops@myorg.com --instance-url https://test.salesforce.com
    ```
* **Key Flags**:
    * `-i | --client-id`: The Consumer Key from your Salesforce Connected App.
    * `-f | --jwt-key-file`: Path to the private key (`.key` file) used to sign the JWT.
    * `-u | --username`: The username of the user authenticating.

### `sf auth list`
* **Explanation**: Lists all authorized Salesforce orgs on your local machine, indicating access tokens status, aliases, and defaults.
* **Example**:
    ```bash
    sf auth list
    ```

### `sf auth logout`
* **Explanation**: Disconnects and removes authentication credentials for a specific org or all orgs from your machine.
* **Example**:
    ```bash
    sf auth logout --target-org my-sandbox
    ```

---

## 3. Configuration & Aliasing (`sf config` & `sf alias`)

Manage local CLI behavior, set default target environments, and manage simple text shortcuts (aliases).

### `sf config set`
* **Explanation**: Sets configuration variables globally or locally for your current project directory.
* **Use Case**: Setting your default scratch org or target org so you don't have to pass the `-o` flag every time.
* **Example**:
    ```bash
    sf config set target-org=my-sandbox --global
    ```
* **Common Variables**:
    * `target-org`: The username or alias of your default environment.
    * `target-dev-hub`: The username or alias of your default Dev Hub.

### `sf config get` & `sf config list`
* **Explanation**: Retrieves specific configuration values or prints all currently active configurations.
* **Example**:
    ```bash
    sf config list
    ```

### `sf alias set`
* **Explanation**: Associates a custom nickname (alias) with a username or long identifier.
* **Example**:
    ```bash
    sf alias set devorg=dev-user@mycompany.com
    ```

### `sf alias list`
* **Explanation**: Displays all currently configured aliases.
* **Example**:
    ```bash
    sf alias list
    ```

---

## 4. Org Management (`sf org`)

Commands dedicated to tracking, opening, creating, and destroying Salesforce environments like scratch orgs and sandboxes.

### `sf org open`
* **Explanation**: Automatically generates a secure, front-door URL and opens the specified Salesforce org in your default web browser without prompting for a password.
* **Example**:
    ```bash
    sf org open --target-org my-sandbox -p lightning/setup/FlexiPageList/home
    ```
* **Key Flags**:
    * `-p | --path`: Navigates directly to a specific page or setup item upon login.

### `sf org create scratch`
* **Explanation**: Spins up a highly configurable, temporary Salesforce environment (Scratch Org) dictated by a definition file.
* **Use Case**: Source-driven, isolated feature development.
* **Example**:
    ```bash
    sf org create scratch --definition-file config/project-scratch-def.json --alias feature-branch-org --set-default --duration-days 7
    ```
* **Key Flags**:
    * `-f | --definition-file`: Path to the scratch org configuration JSON.
    * `-d | --duration-days`: Lifetime of the scratch org (1 to 30 days).
    * `-v | --target-dev-hub`: The Dev Hub org responsible for spinning up this scratch org.

### `sf org create sandbox`
* **Explanation**: Requests the creation or clone of a persistent Sandbox from your production or sandbox environment.
* **Example**:
    ```bash
    sf org create sandbox --name devbox --license-type Developer --target-org prod-hub
    ```

### `sf org resume`
* **Explanation**: Resumes a running, long-lasting async task like tracking sandbox creation status.
* **Example**:
    ```bash
    sf org resume sandbox --job-id 0GR...
    ```

### `sf org delete scratch` / `sf org delete sandbox`
* **Explanation**: Deletes and releases resources of a scratch org or sandbox.
* **Example**:
    ```bash
    sf org delete scratch --target-org feature-branch-org --no-prompt
    ```

---

## 5. Project Management & Metadata (`sf project`)

These commands are the engine of source-driven development, handling the generation of local projects and syncing code/metadata with Salesforce environments.

### `sf project generate`
* **Explanation**: Initializes a standard local Salesforce DX project structure containing directories for source code, configuration files, and tests.
* **Example**:
    ```bash
    sf project generate --name my-new-app --template standard
    ```

### `sf project deploy start`
* **Explanation**: Deploys local metadata/source code from your project into a targeted Salesforce org.
* **Use Case**: Deploying your changes to a sandbox or production environment, or pushing code to non-source-tracked orgs.
* **Example**:
    ```bash
    sf project deploy start --target-org my-sandbox --metadata ApexClass:AccountTriggerHandler --metadata CustomObject:Account
    ```
* **Example to deploy manifest (Releative path)**:
    ```bash
    sf project deploy start --target-org my-sandbox --manifest manifest/packagePermission.xml
    ```
* **Key Flags**:
    * `-m | --metadata`: Specfies specific components or component types to deploy.
    * `-p | --source-dir`: Path to the specific local directory you want to deploy.
    * `-c | --checkonly`: Validates the deployment components and runs tests *without* actually committing changes (dry-run).
    * `-l | --test-level`: Specifies test execution level (`NoTestRun`, `RunSpecifiedTests`, `RunLocalTests`, `RunAllTestsInOrg`).

### `sf project retrieve start`
* **Explanation**: Fetches metadata from a target Salesforce org and writes it locally to your project directory.
* **Use Case**: Pulling changes made by an admin via the Setup UI directly into your local IDE (VS Code).
* **Example**:
    ```bash
    sf project retrieve start --target-org my-sandbox --metadata CustomField:Contact.Subscription_Status__c
    ```
* **Example to retrieve manifest (Releative path) **:
    ```bash
    sf project retrieve start --target-org my-sandbox --manifest manifest/packagePermission.xml
    ```

### `sf project deploy validate` / `sf project deploy quick`
* **Explanation**: `validate` checks deployment readiness. `quick` executes a fast deployment using a previously successful validation ID (skipping test steps if already validated).
* **Example**:
    ```bash
    sf project deploy quick --job-id 0Af... --target-org production
    ```

---

## 6. Apex & Testing (`sf apex`)

Commands for running Apex code anonymously, streaming system logs, and executing automated test suites.

### `sf apex run`
* **Explanation**: Executes a block of Apex code anonymously on the target org.
* **Use Case**: Running brief data maintenance scripts or testing utility methods on the fly.
* **Example**:
    ```bash
    sf apex run --file scripts/apex/hello.apex --target-org my-sandbox
    ```

### `sf apex run test`
* **Explanation**: Runs Apex unit tests within the target org and reports code coverage results.
* **Example**:
    ```bash
    sf apex run test --target-org my-sandbox --result-format human --code-coverage --test-level RunLocalTests
    ```
* **Key Flags**:
    * `-c | --code-coverage`: Calculates and reports the line-by-line test coverage.
    * `-r | --result-format`: Sets the response format (`human`, `tap`, `junit`, `json`).

### `sf apex tail log`
* **Explanation**: Streams real-time debug logs directly from the target org to your local terminal window.
* **Example**:
    ```bash
    sf apex tail log --target-org my-sandbox --debug-level MyCustomDebugLevel
    ```

---

## 7. Data Operations (`sf data`)

Interact directly with record data inside your Salesforce org via queries or bulk operations.

### `sf data query`
* **Explanation**: Executes a SOQL (Salesforce Object Query Language) query against records in the target org.
* **Example**:
    ```bash
    sf data query --query "SELECT Id, Name, Industry FROM Account WHERE Industry = 'Technology'" --target-org my-sandbox
    ```

### `sf data record create` / `sf data record update` / `sf data record delete`
* **Explanation**: CRUD operations on single records directly from the terminal.
* **Example**:
    ```bash
    sf data record create --sobject Account --values "Name='Acme Corp' Industry='Manufacturing'" --target-org my-sandbox
    ```

### `sf data import tree` / `sf data export tree`
* **Explanation**: Imports or exports collections of related records using SObject Tree APIs, maintaining parent-child relationships via JSON files.
* **Use Case**: Seeding a fresh scratch org or developer sandbox with realistic mock test data.
* **Example**:
    ```bash
    sf data export tree --query "SELECT Id, Name, (SELECT LastName FROM Contacts) FROM Account" --output-dir data/
    sf data import tree --files data/Account-Contact.json --target-org scratch-org
    ```

---

## 8. Packaging (`sf package`)

Commands dedicated to managing the life cycle of second-generation unlocked packages (2GP) and managed packages.

### `sf package create`
* **Explanation**: Initializes a new package record on your Dev Hub org.
* **Example**:
    ```bash
    sf package create --name "CoreBillingEngine" --package-type Unlocked --path force-app
    ```

### `sf package version create`
* **Explanation**: Compiles local metadata into a new immutable version of the package.
* **Example**:
    ```bash
    sf package version create --package "CoreBillingEngine" --installation-key secretKey123 --wait 10
    ```

### `sf package install`
* **Explanation**: Installs a specific package version (via its package version ID `04t...`) into an authorized target org.
* **Example**:
    ```bash
    sf package install --package 04t... --target-org test-sandbox --wait 15
    ```

---

## 9. CLI Maintenance & Troubleshooting

Keep your CLI binary engine clean, working, and updated.

### `sf update`
* **Explanation**: Checks for and installs the latest stable version of the Salesforce CLI binary tool.
* **Example**:
    ```bash
    sf update
    ```

### `sf version`
* **Explanation**: Displays version details of the CLI core system package alongside system architecture notes.
* **Example**:
    ```bash
    sf version
    ```

### `sf plugins`
* **Explanation**: Manages CLI extensibility plugins. You can add community features or official add-on packages.
* **Example**:
    ```bash
    sf plugins install @salesforce/plugin-templates
    ```

---
*Tip: If you're adapting from older scripts, most commands mapping from `sfdx force:<domain>:<action>` map neatly into `sf <domain> <action> [sub-action]`. Run `sf commands` to get an interactive table of all available commands in your specific environment.*
