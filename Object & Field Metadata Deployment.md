# Salesforce Metadata XML Cheatsheet

*A definitive development guide for modern SFDX Source Format (.object-meta.xml, .field-meta.xml, etc.)*

This cheatsheet provides the standard XML structures required to build custom schema elements in Salesforce using the Metadata API or Salesforce DX (SFDX) source format.

---

# 1. CustomObject Metadata (`.object-meta.xml`)

Defines the core structure of a custom object configuration.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CustomObject xmlns="http://soap.sforce.com/2006/04/metadata">
    <deploymentStatus>Deployed</deploymentStatus>
    <description>Tracks company-wide strategic projects.</description>
    <enableActivities>true</enableActivities>
    <enableBulkApi>true</enableBulkApi>
    <enableHistory>true</enableHistory>
    <enableReports>true</enableReports>
    <enableSharing>true</enableSharing>
    <enableStreamingApi>true</enableStreamingApi>
    <label>Project</label>
    <pluralLabel>Projects</pluralLabel>
    <sharingModel>ReadWrite</sharingModel>

    <nameField>
        <label>Project Name</label>
        <type>Text</type>
    </nameField>
</CustomObject>
```

| Tag Element | Supported Values | Description |
|------------|------------------|-------------|
| deploymentStatus | InDevelopment, Deployed | Controls visibility of the object to non-admin profiles |
| sharingModel | Private, Read, ReadWrite, ControlledByParent | Defines OWD sharing rules |
| nameField | Label/Type Block | Defines standard Name field |

---

# 2. CustomField Metadata (`fields/*.field-meta.xml`)

## A. Text Field

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CustomField xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Project_Code__c</fullName>
    <label>Project Code</label>
    <type>Text</type>
    <length>50</length>
    <required>true</required>
    <unique>true</unique>
    <externalId>true</externalId>
</CustomField>
```

---
## B. Number, Currency, Percent Field

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CustomField xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Project_Code__c</fullName>
    <label>Project Code</label>
    <type>Text</type>
    <precision>18</precision>
    <scale>2</scale>
    <required>true</required>
    <unique>true</unique>
    <externalId>true</externalId>
</CustomField>
```

---

## C. Checkbox Field

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CustomField xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Active__c</fullName>
    <label>Active</label>
    <type>Checkbox</type>
    <defaultValue>true</defaultValue>
</CustomField>
```

---


## D. Picklist Field (Global & Local Value Sets)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CustomField xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Priority__c</fullName>
    <label>Priority</label>
    <type>Picklist</type>

    <valueSet>
        <restricted>true</restricted>

        <valueSetDefinition>
            <sorted>false</sorted>

            <value>
                <fullName>High</fullName>
                <default>false</default>
                <label>High</label>
            </value>

            <value>
                <fullName>Medium</fullName>
                <default>true</default>
                <label>Medium</label>
            </value>

        </valueSetDefinition>
    </valueSet>
</CustomField>
```

### Using Global Value Sets

Replace:

```xml
<valueSetDefinition>
...
</valueSetDefinition>
```

With:

```xml
<valueSetName>YourGlobalValueSetName</valueSetName>
```

---

## E. Lookup Relationship Field

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CustomField xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Department__c</fullName>
    <label>Department</label>
    <type>Lookup</type>
    <referenceTo>Department__c</referenceTo>

    <relationshipLabel>Associated Projects</relationshipLabel>
    <relationshipName>Projects</relationshipName>

    <required>false</required>
    <deleteConstraint>SetNull</deleteConstraint>
</CustomField>
```

### Master-Detail Relationship

Change:

```xml
<type>Lookup</type>
```

To:

```xml
<type>MasterDetail</type>
```

Remove:

```xml
<deleteConstraint>
```

Add:

```xml
<writeRequiresMasterRead>false</writeRequiresMasterRead>
<reparentableMasterDetail>true</reparentableMasterDetail>
```

---

## F. Formula Field

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CustomField xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Days_Remaining__c</fullName>
    <label>Days Remaining</label>
    <type>Number</type>

    <formula>End_Date__c - TODAY()</formula>
    <formulaTreatBlanksAs>BlankAsZero</formulaTreatBlanksAs>

    <precision>18</precision>
    <scale>0</scale>
</CustomField>
```

---

# 3. Validation Rules (`validationRules/*.validationRule-meta.xml`)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ValidationRule xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Prevent_Future_Start_Date</fullName>
    <active>true</active>

    <description>
        Ensures start date cannot be set in the future.
    </description>

    <errorConditionFormula>
        Start_Date__c > TODAY()
    </errorConditionFormula>

    <errorDisplayField>
        Start_Date__c
    </errorDisplayField>

    <errorMessage>
        The project start date cannot be a future calendar date.
    </errorMessage>
</ValidationRule>
```

---

# 4. Record Types (`recordTypes/*.recordType-meta.xml`)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<RecordType xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Internal_Project</fullName>
    <active>true</active>
    <label>Internal Project</label>

    <picklistValues>
        <picklist>Priority__c</picklist>

        <values>
            <fullName>Medium</fullName>
            <default>true</default>
        </values>
    </picklistValues>
</RecordType>
```

---

# 5. List Views (`listViews/*.listView-meta.xml`)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ListView xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Active_Projects_View</fullName>

    <columns>NAME</columns>
    <columns>Project_Code__c</columns>
    <columns>Priority__c</columns>

    <filterScope>Everything</filterScope>

    <filters>
        <field>Status__c</field>
        <operation>equals</operation>
        <value>In Progress</value>
    </filters>

    <label>Active Projects</label>
</ListView>
```

---

## Salesforce Metadata Folder Structure (SFDX)

```text
force-app/
└── main/
    └── default/
        └── objects/
            └── Project__c/
                ├── Project__c.object-meta.xml
                ├── fields/
                │   ├── Project_Code__c.field-meta.xml
                │   ├── Priority__c.field-meta.xml
                │   └── Department__c.field-meta.xml
                ├── validationRules/
                │   └── Prevent_Future_Start_Date.validationRule-meta.xml
                ├── recordTypes/
                │   └── Internal_Project.recordType-meta.xml
                └── listViews/
                    └── Active_Projects_View.listView-meta.xml
```

---

## Quick Reference

| Metadata Type | Folder |
|--------------|---------|
| Custom Object | objects/ObjectName/ObjectName.object-meta.xml |
| Custom Field | objects/ObjectName/fields |
| Validation Rule | objects/ObjectName/validationRules |
| Record Type | objects/ObjectName/recordTypes |
| List View | objects/ObjectName/listViews |
| Compact Layout | objects/ObjectName/compactLayouts |
| Field Set | objects/ObjectName/fieldSets |
| Web Link | objects/ObjectName/webLinks |
| Business Process | objects/ObjectName/businessProcesses |
