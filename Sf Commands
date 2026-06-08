# 🚀 Salesforce CLI (SFDX / SF) Complete Cheatsheet

> **Note:** Legacy `sfdx` commands were deprecated (removed Nov 2024). Use the new `sf` CLI for all new work.
> Both syntaxes are listed side-by-side where relevant.

---

## 📦 Installation & Setup

| Task | Command |
|------|---------|
| Install SF CLI (macOS/Linux) | `npm install -g @salesforce/cli` |
| Install via official installer | https://developer.salesforce.com/tools/salesforcecli |
| Check version | `sf --version` / `sfdx --version` / `sfdx -v` |
| Update CLI | `sf update` / `sfdx update` |
| List installed plugins | `sf plugins` |
| Install a plugin | `sf plugins install <plugin-name>` |
| Get help on any topic | `sf <topic> --help` / `sf <topic> <command> -h` |

---

## 🔐 Authentication (Auth / Login)

### Login & Logout

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Login via browser (Dev Hub) | `sf org login web --set-default-dev-hub -a DevHub` | `sfdx auth:web:login -d -a DevHub` |
| Login via browser (Sandbox) | `sf org login web -r https://test.salesforce.com -a MySandbox` | `sfdx auth:web:login -r https://test.salesforce.com -a MySandbox` |
| Login via JWT (CI/CD) | `sf org login jwt -u user@org.com -f key.pem -i <clientId> -a MyOrg` | `sfdx auth:jwt:grant -u user@org.com -f key.pem -i <clientId>` |
| Login using SFDX auth URL | `sf org login sfdx-url -f authFile.txt` | `sfdx auth:sfdxurl:store -f authFile.txt` |
| Login using access token | `sf org login access-token -r https://login.salesforce.com` | — |
| List all authorized orgs | `sf org list` | `sfdx auth:list` |
| Logout from org | `sf org logout -o <alias>` | `sfdx auth:logout -u <alias>` |
| Logout from all orgs | `sf org logout --all` | — |

---

## ⚙️ Configuration & Aliases

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Set default Dev Hub | `sf config set target-dev-hub=<alias>` | `sfdx config:set defaultdevhubusername=<alias>` |
| Set default org | `sf config set target-org=<alias>` | `sfdx config:set defaultusername=<alias>` |
| List config | `sf config list` | `sfdx config:list` |
| Set config globally | `sf config set <key>=<value> --global` | — |
| Unset config | `sf config unset target-org` | — |
| List all aliases | `sf alias list` | — |
| Set alias | `sf alias set MyAlias=<username>` | — |
| Unset alias | `sf alias unset MyAlias` | — |

---

## 📁 Project Management

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Create new SFDX project | `sf project generate -n MyProject` | `sfdx force:project:create -n MyProject` |
| Create project with manifest | `sf project generate -n MyProject --manifest` | `sfdx force:project:create --projectname MyProject --manifest` |
| Generate manifest (package.xml) | `sf project generate manifest --from-org -o <alias>` | `sfdx force:source:manifest:create` |
| Generate manifest from metadata | `sf project generate manifest -m ApexClass,LightningComponentBundle` | — |
| Display project structure info | `sf project --help` | — |

---

## 🏢 Org Management

### General Org Commands

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Open org in browser | `sf org open -o <alias>` | `sfdx force:org:open -u <alias>` |
| Open specific page | `sf org open -o <alias> -p lightning/setup/ApexClasses` | — |
| Display org info | `sf org display -o <alias>` | `sfdx force:org:display -u <alias>` |
| List all orgs | `sf org list` | `sfdx force:org:list` |
| List all orgs (clean stale) | `sf org list --clean` | `sfdx force:org:list --clean` |

### Scratch Orgs

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Create scratch org | `sf org create scratch -f config/project-scratch-def.json -a MyScratch -v DevHub` | `sfdx force:org:create -f config/project-scratch-def.json --setalias MyScratch -v DevHub` |
| Create scratch org (set default) | `sf org create scratch -f config/project-scratch-def.json -a MyScratch --set-default` | `sfdx force:org:create ... --setdefaultusername` |
| Create scratch org (30 days) | `sf org create scratch -f config/project-scratch-def.json -d 30 -a MyScratch` | `sfdx force:org:create ... -d 30` |
| Delete scratch org | `sf org delete scratch -o MyScratch` | `sfdx force:org:delete -u MyScratch` |
| Generate user password | `sf org generate password -o MyScratch` | `sfdx force:user:password:generate -u MyScratch` |
| List scratch orgs | `sf org list --all` | `sfdx force:org:list --all` |
| Resume scratch org creation | `sf org resume scratch --job-id <id>` | — |

### Sandboxes

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Create sandbox | `sf org create sandbox -o <prodAlias> -a MySandbox -f config/sandbox-def.json` | — |
| Delete sandbox | `sf org delete sandbox -o <alias>` | — |
| Resume sandbox creation | `sf org resume sandbox -n <sandboxName> -o <prodAlias>` | — |
| List sandboxes | `sf org list sandboxes -o <prodAlias>` | — |

---

## 🔄 Source Tracking (Push/Pull — Scratch Orgs)

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Push source to scratch org | `sf project deploy start --source-dir force-app -o MyScratch` | `sfdx force:source:push -u MyScratch` |
| Pull changes from scratch org | `sf project retrieve start -o MyScratch` | `sfdx force:source:pull -u MyScratch` |
| View push/pull status | `sf project deploy report` | `sfdx force:source:status` |
| Force push (ignore conflicts) | `sf project deploy start --ignore-conflicts -o MyScratch` | `sfdx force:source:push -f` |
| Force pull (ignore conflicts) | `sf project retrieve start --ignore-conflicts -o MyScratch` | `sfdx force:source:pull -f` |

---

## 📤 Deploy (Source Format)

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Deploy via package.xml | `sf project deploy start -x manifest/package.xml -o <alias>` | `sfdx force:source:deploy -x manifest/package.xml -u <alias>` |
| Deploy specific metadata type | `sf project deploy start -m ApexClass -o <alias>` | `sfdx force:source:deploy -m ApexClass -u <alias>` |
| Deploy specific component | `sf project deploy start -m ApexClass:MyClass -o <alias>` | `sfdx force:source:deploy -m ApexClass:MyClass` |
| Deploy source directory | `sf project deploy start -d force-app -o <alias>` | `sfdx force:source:deploy -p force-app` |
| Deploy and run tests | `sf project deploy start -x manifest/package.xml --test-level RunLocalTests -o <alias>` | `sfdx force:source:deploy -x manifest/package.xml --testlevel RunLocalTests` |
| Deploy with specific tests | `sf project deploy start -x manifest/package.xml -l RunSpecifiedTests -t MyTest -o <alias>` | `sfdx force:source:deploy --testlevel RunSpecifiedTests -r MyTest` |
| Validate only (dry run) | `sf project deploy validate -x manifest/package.xml -o <alias>` | `sfdx force:source:deploy -c -x manifest/package.xml` |
| Quick deploy (after validate) | `sf project deploy quick --job-id <id> -o <alias>` | — |
| Check deploy status | `sf project deploy report --job-id <id>` | `sfdx force:source:deploy:report` |
| Cancel deployment | `sf project deploy cancel --job-id <id>` | — |

**Test Level Options:**
- `NoTestRun` — No tests (only allowed in sandboxes)
- `RunSpecifiedTests` — Run listed test classes only
- `RunLocalTests` — Run all local (non-managed) tests
- `RunAllTestsInOrg` — Run every test in the org

---

## 📥 Retrieve (Source Format)

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Retrieve via package.xml | `sf project retrieve start -x manifest/package.xml -o <alias>` | `sfdx force:source:retrieve -x manifest/package.xml` |
| Retrieve specific metadata type | `sf project retrieve start -m ApexClass -o <alias>` | `sfdx force:source:retrieve -m ApexClass` |
| Retrieve specific component | `sf project retrieve start -m ApexClass:MyClass -o <alias>` | `sfdx force:source:retrieve -m ApexClass:MyClass` |
| Retrieve to specific directory | `sf project retrieve start -x manifest/package.xml -r ./retrieved` | — |
| Preview what will be retrieved | `sf project retrieve preview -o <alias>` | — |

---

## 🗄️ Metadata API (mdAPI Format)

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Retrieve via mdapi | `sf project retrieve start --manifest manifest/package.xml --output-dir ./mdapi-out -o <alias>` | `sfdx force:mdapi:retrieve -r ./mdapi-out -o <alias>` |
| Deploy via mdapi zip | `sf project deploy start --metadata-dir ./mdapi-package -o <alias>` | `sfdx force:mdapi:deploy -d ./mdapi-package -w 1000 -u <alias>` |
| Convert source → mdapi format | `sf project convert source -d ./mdapi-out` | `sfdx force:source:convert -d ./mdapi-out` |
| Convert mdapi → source format | `sf project convert mdapi -r ./mdapi-package -d force-app` | `sfdx force:mdapi:convert -r ./mdapi-package` |

---

## ⚡ Apex

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Create Apex class | `sf apex generate class -n MyClass -d force-app/main/default/classes` | `sfdx force:apex:class:create -n MyClass -d force-app/main/default/classes` |
| Create Apex trigger | `sf apex generate trigger -n MyTrigger -s Account -d force-app/main/default/triggers` | `sfdx force:apex:trigger:create -n MyTrigger -s Account` |
| Execute anonymous Apex | `sf apex run -f scripts/apex/myScript.apex -o <alias>` | `sfdx force:apex:execute -f scripts/apex/myScript.apex -u <alias>` |
| Run Apex tests | `sf apex run test -n MyTestClass -o <alias>` | `sfdx force:apex:test:run -n MyTestClass -u <alias>` |
| Run tests synchronously | `sf apex run test -n MyTestClass --synchronous -o <alias>` | `sfdx force:apex:test:run -n MyTestClass -s` |
| Run all local tests | `sf apex run test -l RunLocalTests -o <alias>` | `sfdx force:apex:test:run -l RunLocalTests` |
| Run tests with code coverage | `sf apex run test -n MyTest --code-coverage -o <alias>` | `sfdx force:apex:test:run -n MyTest -c` |
| Get test results | `sf apex get test --test-run-id <id> -o <alias>` | `sfdx force:apex:test:report -i <id>` |
| Tail Apex debug logs | `sf apex tail log -o <alias>` | `sfdx force:apex:log:tail -u <alias>` |
| List Apex logs | `sf apex list log -o <alias>` | `sfdx force:apex:log:list -u <alias>` |
| Get specific Apex log | `sf apex get log --log-id <id> -o <alias>` | `sfdx force:apex:log:get -i <id>` |

---

## ⚡ Lightning Web Components (LWC)

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Create LWC component | `sf lightning generate component -n myComp -d force-app/main/default/lwc` | `sfdx force:lightning:component:create --type lwc -n myComp` |
| Create Aura component | `sf lightning generate component -n myComp --type aura -d force-app/main/default/aura` | `sfdx force:lightning:component:create -n myComp` |
| Create Lightning app | `sf lightning generate app -n MyApp -d force-app/main/default/aura` | `sfdx force:lightning:app:create -n MyApp` |
| Create Lightning event | `sf lightning generate event -n MyEvent -d force-app/main/default/aura` | `sfdx force:lightning:event:create -n MyEvent` |
| Create Lightning interface | `sf lightning generate interface -n MyInterface -d force-app/main/default/aura` | `sfdx force:lightning:interface:create -n MyInterface` |

---

## 🗂️ Data Commands

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Run SOQL query | `sf data query -q "SELECT Id, Name FROM Account LIMIT 10" -o <alias>` | `sfdx force:data:soql:query -q "SELECT Id, Name FROM Account LIMIT 10"` |
| Query as tooling API | `sf data query --use-tooling-api -q "SELECT Id FROM ApexClass" -o <alias>` | `sfdx force:data:soql:query -t -q "..."` |
| Create record | `sf data create record -s Account -v "Name=TestAccount" -o <alias>` | `sfdx force:data:record:create -s Account -v "Name=TestAccount"` |
| Get record | `sf data get record -s Account -i <recordId> -o <alias>` | `sfdx force:data:record:get -s Account -i <recordId>` |
| Update record | `sf data update record -s Account -i <recordId> -v "Name=NewName" -o <alias>` | `sfdx force:data:record:update -s Account -i <recordId> -v "Name=NewName"` |
| Delete record | `sf data delete record -s Account -i <recordId> -o <alias>` | `sfdx force:data:record:delete -s Account -i <recordId>` |
| Bulk import (CSV) | `sf data import bulk -s Account -f accounts.csv -o <alias>` | `sfdx force:data:bulk:upsert -s Account -f accounts.csv -i Id` |
| Bulk upsert (CSV) | `sf data upsert bulk -s Account -f accounts.csv -i ExternalId__c -o <alias>` | `sfdx force:data:bulk:upsert -s Account -f accounts.csv -i ExternalId__c` |
| Bulk delete (CSV) | `sf data delete bulk -s Account -f ids.csv -o <alias>` | `sfdx force:data:bulk:delete -s Account -f ids.csv` |
| Export data (tree) | `sf data export tree -q "SELECT Id,Name FROM Account" -o <alias>` | `sfdx force:data:tree:export -q "SELECT Id,Name FROM Account"` |
| Import data (tree) | `sf data import tree -f data/Account.json -o <alias>` | `sfdx force:data:tree:import -f data/Account.json` |
| Resume bulk job | `sf data resume bulk --job-id <id> -o <alias>` | — |

---

## 📦 Packaging

### Unlocked / 2nd-Gen Packages

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Create package | `sf package create -n "MyPackage" -t Unlocked -r force-app -v DevHub` | `sfdx force:package:create --name MyPackage --packagetype Unlocked --path force-app -v DevHub` |
| Create package version | `sf package version create -p MyPackage -d force-app -w 10 -v DevHub` | `sfdx force:package:version:create -p MyPackage -d force-app --wait 10 -v DevHub` |
| Promote package version | `sf package version promote -p <packageVersionId> -v DevHub` | `sfdx force:package:version:promote -p <pkgVersion> -v DevHub` |
| Install package | `sf package install -p <subscriberPkgVersionId> -w 10 -o <alias>` | `sfdx force:package:install --package <id> --wait 10 -u <alias>` |
| Uninstall package | `sf package uninstall -p <subscriberPkgVersionId> -o <alias>` | `sfdx force:package:uninstall -p <id>` |
| List packages | `sf package list -v DevHub` | `sfdx force:package:list -v DevHub` |
| List package versions | `sf package version list -p MyPackage -v DevHub` | `sfdx force:package:version:list -v DevHub` |
| List installed packages | `sf package installed list -o <alias>` | `sfdx force:package:installed:list -u <alias>` |
| Report on version create | `sf package version create report --package-create-request-id <id>` | — |

### 1st-Gen Packages

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Create 1GP package version | `sf package1 version create -i <packageId> -n 1.0 -o DevHub` | `sfdx force:package1:version:create -i <packageId>` |
| List 1GP packages | `sf package1 version list -o DevHub` | `sfdx force:package1:version:list` |

---

## 👤 User Management

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Display user info | `sf org display user -o <alias>` | `sfdx force:user:display -u <alias>` |
| List users in org | `sf org list users -o <alias>` | `sfdx force:user:list -u <alias>` |
| Create user | `sf org create user -f config/user-def.json -o <alias>` | `sfdx force:user:create -f config/user-def.json -u <alias>` |
| Assign permission set | `sf org assign permset -n MyPermSet -o <alias>` | `sfdx force:user:permset:assign -n MyPermSet -u <alias>` |
| Assign permission set license | `sf org assign permsetlicense -n MyPSL -o <alias>` | — |
| Generate password | `sf org generate password -o <alias>` | `sfdx force:user:password:generate -u <alias>` |

---

## 🔍 Org Info & Limits

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| View org limits | `sf limits api display -o <alias>` | `sfdx force:limits:api:display -u <alias>` |
| List metadata in org | `sf org list metadata -m ApexClass -o <alias>` | `sfdx force:mdapi:listmetadata -m ApexClass -u <alias>` |
| List all metadata types | `sf org list metadata-types -o <alias>` | `sfdx force:mdapi:describemetadata -u <alias>` |
| Display org details | `sf org display -o <alias>` | `sfdx force:org:display -u <alias>` |

---

## 🧪 Static Analysis & Code Quality

| Task | Command |
|------|---------|
| Run Salesforce Code Analyzer (PMD) | `sf scanner run -t force-app -e pmd -f table` |
| Run scanner on specific file | `sf scanner run -t force-app/main/default/classes/MyClass.cls -f table` |
| Run scanner with specific ruleset | `sf scanner run -t force-app -c "Design,Best Practices,Performance" -f table` |
| Run ESLint (LWC) | `sf scanner run -e eslint -t force-app/main/default/lwc -f table` |
| List scanner rules | `sf scanner rule list` |
| Output results to CSV | `sf scanner run -t force-app -f csv -o scanner-results.csv` |

---

## 🛠️ Generate / Scaffold Metadata

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Create Apex class | `sf apex generate class -n ClassName -d path/to/classes` | `sfdx force:apex:class:create -n ClassName` |
| Create Apex trigger | `sf apex generate trigger -n TriggerName -s SObjectName` | `sfdx force:apex:trigger:create -n TriggerName -s SObject` |
| Create LWC | `sf lightning generate component -n compName --type lwc` | `sfdx force:lightning:component:create --type lwc -n compName` |
| Create Aura component | `sf lightning generate component -n compName --type aura` | `sfdx force:lightning:component:create -n compName` |
| Create SOQL query file | `sf apex generate class -n ...` | — |
| Create static resource | — | `sfdx force:staticresource:create -n ResourceName` |
| Create custom object | — | `sfdx force:schema:sobject:create` |
| Describe SObject | `sf sobject describe -s Account -o <alias>` | `sfdx force:schema:sobject:describe -s Account` |
| List all SObjects | `sf sobject list -o <alias>` | `sfdx force:schema:sobject:list -c all` |

---

## 🚦 Community / Experience Cloud

| Task | sf CLI | sfdx (legacy) |
|------|--------|---------------|
| Create community | `sf community create -n MyCommunity -t "Build Your Own" -u https://mycommunity -o <alias>` | `sfdx force:community:create -n MyCommunity -t "Build Your Own" -u https://mycommunity` |
| Publish community | `sf community publish -n MyCommunity -o <alias>` | `sfdx force:community:publish -n MyCommunity` |

---

## 📊 Visual Force

| Task | sfdx (legacy) |
|------|---------------|
| Create VF Page | `sfdx force:visualforce:page:create -n MyPage` |
| Create VF Component | `sfdx force:visualforce:component:create -n MyComponent` |

---

## 🔌 API Commands

| Task | sf CLI |
|------|--------|
| Make REST API call (GET) | `sf api request rest '/services/data/v62.0/sobjects/Account/describe' -o <alias>` |
| Make REST API call (POST) | `sf api request rest '/services/data/v62.0/sobjects/Account' --method POST -b '{"Name":"Test"}' -o <alias>` |
| Call SOAP API | `sf api request soap --envelope soapEnvelope.xml -o <alias>` |

---

## 🤖 Agentforce / AI (Spring '26+)

| Task | sf CLI |
|------|--------|
| Generate agent spec | `sf agent generate spec -o <alias>` |
| Create agent from spec | `sf agent create -f agent-spec.yaml -o <alias>` |
| Activate agent | `sf agent enable -o <alias>` |
| Deactivate agent | `sf agent disable -o <alias>` |
| Preview agent | `sf agent preview -o <alias>` |
| Run agent test | `sf agent test run -o <alias>` |
| Get agent test results | `sf agent test results --job-id <id> -o <alias>` |
| Validate agent bundle | `sf agent generate agent-spec --validate -o <alias>` |

---

## 🔄 CI/CD Pipeline Reference

```bash
# ─── Step 1: Authenticate via JWT (CI/CD) ───────────────────────────────────
sf org login jwt \
  --username $SF_USERNAME \
  --jwt-key-file server.key \
  --client-id $CONSUMER_KEY \
  --alias TargetOrg \
  --instance-url https://login.salesforce.com

# ─── Step 2: Validate Deployment (Dry Run) ───────────────────────────────────
sf project deploy validate \
  --manifest manifest/package.xml \
  --target-org TargetOrg \
  --test-level RunLocalTests \
  --json

# ─── Step 3: Quick Deploy (after passing validation) ──────────────────────────
sf project deploy quick \
  --job-id <validationJobId> \
  --target-org TargetOrg

# ─── Step 4: Run Apex Tests ───────────────────────────────────────────────────
sf apex run test \
  --test-level RunLocalTests \
  --target-org TargetOrg \
  --synchronous \
  --code-coverage \
  --result-format human
```

---

## ⚡ Common Flags Quick Reference

| Flag | Long Form | Meaning |
|------|-----------|---------|
| `-o` | `--target-org` | Specify target org alias/username |
| `-v` | `--target-dev-hub` | Specify Dev Hub alias |
| `-a` | `--alias` | Set alias for the org |
| `-d` | `--set-default` | Set as default org |
| `-f` | `--definition-file` | Path to config/definition file |
| `-m` | `--metadata` | Metadata type(s) to operate on |
| `-x` | `--manifest` | Path to package.xml |
| `-p` | `--source-dir` | Path to source directory |
| `-r` | `--instance-url` | Salesforce instance URL |
| `-n` | `--name` | Name of the component |
| `-t` | `--test-level` | Test execution level |
| `-w` | `--wait` | Wait time in minutes |
| `-l` | `--test-level` | Apex test level |
| `--json` | `--json` | Output as JSON |
| `--concise` | `--concise` | Show only summary output |

---

## 🏷️ Org Instance URLs

| Environment | URL |
|-------------|-----|
| Production / Developer | `https://login.salesforce.com` |
| Sandbox | `https://test.salesforce.com` |
| My Domain (Sandbox) | `https://MyDomain--SandboxName.sandbox.my.salesforce.com` |
| My Domain (Prod) | `https://MyDomain.my.salesforce.com` |
| Scratch Org | Auto-generated |

---

## 📁 Key Project Files Reference

| File | Purpose |
|------|---------|
| `sfdx-project.json` | SFDX project config (API version, package dirs, namespace) |
| `manifest/package.xml` | Defines metadata to retrieve/deploy |
| `config/project-scratch-def.json` | Scratch org definition (edition, features, settings) |
| `config/user-def.json` | User definition for scratch org user creation |
| `.forceignore` | Like `.gitignore` — excludes metadata from push/pull |
| `.sf/config.json` | Local CLI config (default org, dev hub) |
| `server.key` | JWT private key for CI/CD authentication |

---

## 🧹 Housekeeping & Diagnostics

| Task | sf CLI |
|------|--------|
| Clean stale org data | `sf org list --clean` |
| Display CLI config | `sf config list` |
| Doctor (diagnose CLI issues) | `sf doctor` |
| Display env info | `sf version --verbose` |
| Generate shell completions | `sf autocomplete` |
| Update to latest CLI | `sf update` |
| Update to specific version | `sf update --version 2.x.x` |

---

## 🔑 Scratch Org Definition — Common Features

```json
{
  "orgName": "My Dev Org",
  "edition": "Developer",
  "hasSampleData": true,
  "features": [
    "EnableSetPasswordInApi",
    "Communities",
    "ServiceCloud",
    "LightningScheduler"
  ],
  "settings": {
    "lightningExperienceSettings": {
      "enableS1DesktopEnabled": true
    },
    "apexSettings": {
      "enableCompileOnDeploy": true
    },
    "securitySettings": {
      "passwordPolicies": {
        "enableSetPasswordInApi": true
      }
    }
  }
}
```

---

## 🔖 Useful Abbreviations

| Abbreviation | Meaning |
|--------------|---------|
| SFDX | Salesforce Developer Experience |
| SF CLI | Salesforce CLI (new unified) |
| DX | Developer Experience |
| mdAPI | Metadata API format |
| pkg.xml | package.xml (manifest file) |
| DevHub | Developer Hub org |
| 2GP | Second-generation packaging |
| 1GP | First-generation packaging |
| LWC | Lightning Web Component |
| SOQL | Salesforce Object Query Language |
| JWT | JSON Web Token (for CI/CD auth) |

---

*Cheatsheet generated from Salesforce SFDX Dev Guide & SF CLI Reference — Current as of Spring '26 (sf v2.133.x)*
