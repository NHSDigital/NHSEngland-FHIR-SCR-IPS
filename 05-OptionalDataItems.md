# Optional Data Items

The table below provides details of how to populate key items from the urgent care data-set into the International Patient Summary FHIR Bundle

## Advance Directives

This should be any Observations in the record which relate to the existence of other key documents such as EPaCCS forms or ReSPECT forms - these actual documents should be separate pointers on NRL, so should not be replicated into this summary document, rather this should indicate their existence so the user can review and retrieve them separately from NRL.

In addition, this section of the summary could contain any other specific key coded observations from those documents, such as ADRT/CPR decisions.

* Filters/Constraints
   * Observations that relate to advanced directives, or the existence of documents like EPaCCS or ReSPECT forms, etc.
   * Items in the narrative should be sorted newest to oldest based on recorded date
 * FHIR Resource Profiles to conform to:
   * [UKCore-Observation](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Observation?version=2.0.1)

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: DNAR
   * PRSB: CIS Legal information
   * Must have data items:
      * Advance decision to refuse treatment (ADRT)
      * Date
      * OR, Alert flag and associated free text
   * PRSB: CIS End of life care
   * Must have data items:
      * CPR Decision date and time
      * CPR decision coded value
      * CPR decision free text
      * CPR decision date for review
      * CPR decision location of document

**Example:**

* Examples to be added

## Functional Status

Observations containing any functional assessment scores - e.g. Frailty score, Rockwood score. Potentially any other coded observations that relate to functional status.

* Filters/Constraints
   * Observations that relate to functional status, or assessment scores relating to functional status
   * Items in the narrative should be sorted newest to oldest based on recorded date
 * FHIR Resource Profiles to conform to:
   * [UKCore-Observation](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Observation?version=2.0.1)

**Example:**

* Examples to be added

## History of Pregnancy 

Episodes of care that relate to pregnancies - with dates, and any associated observations relating to due dates, outcomes and pregnancy status.

* Filters/Constraints
   * Episodes of care that relate to pregnancy episodes
   * Observations with codes that relate to expected due dates, outcomes and pregnancy status
   * Items in the narrative should be sorted newest to oldest based on recorded date
 * FHIR Resource Profiles to conform to:
   * [UKCore-Observation](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Observation?version=2.0.1)
   * [Observation Pregnancy - Expected Delivery Date (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Observation-pregnancy-edd-uv-ips)
   * [Observation Pregnancy - Outcome (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Observation-pregnancy-outcome-uv-ips)
   * [Observation Pregnancy - Status (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Observation-pregnancy-status-uv-ips)

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: Maternity status (no. of weeks pregnant)
   * PRSB: CIS Pregnancy status
   * Must have data items:
      * Pregnancy state - yes/no
      * Expected delivery date

**Example:**

* Examples to be added

## Plan of Care

Care plan documents should be shared as separate documents with NRL, so they should not be included in the shared record patient summary document.

## Alerts

Any active alerts held against the patient. This could include items explicitly created as alerts and any other coded items relating to safeguarding or other items deemed clinically significant enough to flag as alerts.

* Filters/Constraints
   * Flags relating to items explicitly created as "alerts", plus any other items deemed clinically significant - e.g. safeguarding information
   * Items in the narrative should be sorted newest to oldest based on recorded date
 * FHIR Resource Profiles to conform to:
   * [Flag - Alert (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Flag-alert-uv-ips)

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: Safeguarding
   * PRSB: CIS Safeguarding
   * Must have data items:
      * Safeguarding indicator
      * Safeguarding concerns date
      * Safeguarding concerns end date
 * Data Item: Pre-contact information, e.g. scene safety, information about entry to property etc.

**Example:**

* Examples might include risk to self, risk to others, large dogs at the property etc. and any other flags related to safety held on the sending system

## History of Past Problems

Items recorded as "problems" in local clinical systems (e.g. items on the problems list in primary care) that are not "active"

 * Filters/Constraints
   * Include where status is NOT "active"
   * Items in the narrative should be sorted newest to oldest based on recorded date
 * FHIR Resource Profiles to conform to:
   * [UKCore-Condition](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Condition?version=2.0.1)
   * [Condition (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Condition-uv-ips)

**Example:**

* Examples to be added

## Patient Story (AKA About Me)

About Me documents should be shared as separate documents with NRL, so should not be included in the shared record patient summary document.

## Social History

Any observations in the record which relate to smoking or alcohol use.

* Filters/Constraints
   * Observations with codes that relate to smoking or alcohol use
   * Items in the narrative should be sorted newest to oldest based on recorded date
* FHIR Resource Profiles to conform to:
   * [UKCore-Observation](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Observation?version=2.0.1)
   * [Observation Social History - Alcohol Use (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Observation-alcoholuse-uv-ips)
   * [Observation Social History - Tobacco Use (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Observation-tobaccouse-uv-ips)

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: Social history
   * PRSB: CIS Social Context

**Example:**

* Examples to be added

## Vital Signs

Observations from the record with codes that indicate they relate to vital signs.

* Filters/Constraints
   * Where possible, apply different time-ranges for the effective date of the Observation based on the type of result
     * Blood pressure readings (last 6 months)
     * O2 saturations (last 6 months)
     * Heart Rate (last 6 months)
     * Height / Weight / BMI (Latest only)
   * Where this is not possible, limit the data to the last 6 months
   * Items in the narrative should be grouped by type, and within each type, sorted newest to oldest based on recorded date
* FHIR Resource Profiles to conform to:
   * [UKCore-Observation](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Observation?version=2.0.1)

**Example:**

* Examples to be added

## Social Care

Flag to indicate if the patient is known to social care

* Filters/Constraints
  * Derived flag to indicate that the patient is known to social care based on data about social care being available in the shared record
* FHIR Resource Profiles to conform to:
   * Base FHIR Flag Profile

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: Known to Social Care (Current Social Care Support + provider)
   * PRSB: CIS Services and Care
   * Must have data items:
      * Social care package name
      * Social care package type
      * Service start date
      * Service end date
      * Local authority - coded value
      * Local authority - free text

**Example:**

* Examples to be added

## Encounters

Future Encounters (e.g. Acute Waiting Lists), plus the last 6 months of previous encounters

* Filters/Constraints
   * Encounter records that have a start date greater than 6 months ago
   * Items in the narrative should be sorted newest to oldest based on start date
* FHIR Resource Profiles to conform to:
   * [UKCore-Encounter](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Encounter?version=2.0.1)

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: Encounters (Recent - time/event period needs defining)
   * PRSB: CIS Admission details
   * Must have data items:
      * Date of admission
      * Admitted to  - organisation name (and ODS code if available)
      * Specialty - activity treatment function code
      * Specialty - free text
      * Reason for admission - code
      * Reason for admission - free text
      * Legal status on admission - code
   * PRSB: CIS Discharge details
   * Must have data items:
      * Date of discharge
      * Discharge location - location name or description (and ODS code if available)
      * Discharging specialty - code
      * Discharging specialty - free text
      * Legal status on discharge
      * Discharge destination - code
      * Discharge destination - address -free text

**Example:**

* Examples to be added

## Legal information (PoA)

Lasting power of attorney information where this is recorded in the shared record. This could either be a related person or an observation that a power of attorney has been appointed

* Filters/Constraints
   * RelatedPerson records with a type that indicates they are a Power of Attorney, or any Observations with codes that indicate they relate to power of attorneys
   * Items in the narrative should be sorted newest to oldest based on start date
* FHIR Resource Profiles to conform to:
   * [UKCore-RelatedPerson](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-RelatedPerson?version=2.0.1)
   * [UKCore-Observation](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Observation?version=2.0.1)

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: NOK/POA - New Term
   * PRSB: CIS Legal information
   * Must have data items:
      * Lasting Power of Attorney for health and welfare - coded
      * Location of document
      * Name of LPA
      * Contact details
   * PRSB: CIS Personal Contacts
   * Must have data items:
      * Name
      * Relationship
      * Relationship type
      * Contact details

**Example:**

* Examples to be added
