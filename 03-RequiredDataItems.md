# Required Data Items

The list below provides details of how to populate key items from the urgent care data-set into the International Patient Summary FHIR Bundle

NOTE - as these sections are mandatory, if no data is provided in any section, [as per the IPS spec](https://build.fhir.org/ig/HL7/fhir-ips/Empty-Sections-and-Missing-Data.html) an "emptyReason" must be provided for the section to assert the absence of data - generally "unavailable" or "notasked". This is not required for recommended or optional sections, which can be omitted.

## Problems
*(this section is agreed)*

Items recorded as "problems" in local clinical systems (e.g. items on the problems list in primary care). In addition, where possible, any other coded items from the record that have codes that indicate that they are conditions or disorders.

 * Filters/Constraints
   * Where there is a status, include only those that are "active"
   * Items in the narrative should be sorted newest to oldest based on recorded date
 * FHIR Resource Profiles to conform to:
   * [UKCore-Condition](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Condition?version=2.0.1)
   * [Condition (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Condition-uv-ips)

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: Conditions (Current active and significant past)
   * PRSB: CIS Problem List
   * Must have data items:
      * Problem - code
      * Problem - free text
      * Problem onset date
      * Problem end date

**Example:**

* [Example FHIR Condition](Examples/Condition.json)
* [Example HTML Narrative](https://html-preview.github.io/?url=https://github.com/ahatherly-gn/NHS-SCR-IPS/blob/main/Examples/Narrative-Problems.html)

## Allergies and Intolerances
*(this section is agreed)*

Items recorded as Allergies or Intolerances in local clinical systems.

 * Filters/Constraints
   * Include ALL where clincalStatus="active"
   * Where known, include allergies and intolerances that have been resolved, or become inactive (AllergyIntolerance.clinicalStatus=inactive|resolved). These MUST have a date when the allergy or intolerance was recorded as inactive or resolved (AllergyIntolerance.recordedDate) and should have the last know recorded occurrence (AllergyIntolerance.lastOccurrence)
   * Items in the narrative should be sorted newest to oldest based on recorded date
 * FHIR Resource Profiles to conform to:
   * [UKCore-AllergyIntolerance](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-AllergyIntolerance?version=2.0.1)
   * [AllergyIntolerance (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/AllergyIntolerance-uv-ips)

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: Allergies
   * PRSB: CIS Allergies
   * Must have data items:
      * Causative agent  - coded value
      * Causative agent - free text
      * Type of reaction
      * Description of reaction - coded value
      * Description of reaction - free text

**Example:**

* [Example FHIR AllergyIntolerance](Examples/AllergyIntolerance.json)
* [Example HTML Narrative](https://html-preview.github.io/?url=https://github.com/ahatherly-gn/NHS-SCR-IPS/blob/main/Examples/Narrative-Allergies.html)

## Medication Summary
*(this section is agreed)*

Current medications being taken. This includes active prescribed medications, including repeat, one-off and acute meds and any meds recorded in other care settings if available.

 * Filters/Constraints
   * Current repeat medications
   * Where possible:
     * Discontinued repeat medications from the last 6 months
     * Acute Medications taken in the last year
   * Items in the narrative should be sorted newest to oldest based on recorded date
 * FHIR Resource Profiles to conform to:
   * [UKCore-MedicationStatement](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-MedicationStatement?version=2.0.1)
   * [MedicationStatement (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/MedicationStatement-uv-ips)

The details of the actual Medication will be in a FHIR Medication resource. This can either be contained within the MedicationStatement, or included separately in the bundle and referenced from the MedicationStatement.

 * FHIR Resource Profiles to conform to:
   * [UKCore-Medication](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Medication?version=2.0.1)
   * [Medication (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Medication-uv-ips)

NOTE: The UK Core profile specifies that the "derivedFrom" should be provided, which links the MedicationStatement to the MedicationRequest. For the purposes of a patient summary this is not required, so this relationship can be omitted.

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: Medication (Current GP Meds and Discharge Meds)
   * PRSB: CIS Medications
   * Must have data items:
      * Medication name - coded value
      * Medication name - free text

**Example:**

* [Example FHIR MedicationStatement](Examples/MedicationStatement.json)
* [Example HTML Narrative](https://html-preview.github.io/?url=https://github.com/ahatherly-gn/NHS-SCR-IPS/blob/main/Examples/Narrative-Medications.html)
