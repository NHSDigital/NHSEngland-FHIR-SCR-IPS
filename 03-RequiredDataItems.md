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

**Where there are no Allergies or Intolerances to report**

Three scenarios have been identified, that should be treated as follows
   * The system generating this summary won't ever populate this section
   In this scenario, the narrative section will be included (as it is a mandated section), and the Composition.section.emptyReason be populated with an "unavailable" code from the https://hl7.org/fhir/R4/valueset-list-empty-reason.html CodeSystem
   * The system generating this summary does not have any data for this section 
   In this scenario, the narrative section will be included (as it is a mandated section), and the Composition.section.emptyReason be populated with an "nilknown" code from the https://hl7.org/fhir/R4/valueset-list-empty-reason.html CodeSystem
   * The system generating this summary believes there's no know allergies for this patient
   In this secenrio, a coded entry should be created with the SNOMED concept No known allergy (situation) \| 716186003, and the narrative section should be populated to state that there are no known allergies for this patient
   
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
* [Example FHIR NoAllergyIntolerance](Examples/NoAllergyIntolerance.json)

## Medication Summary
*(this section is agreed)*

Current medications being taken. This includes active prescribed medications, including repeat, one-off and acute meds and any medications recorded in other care settings if available.

 * Filters/Constraints
   * Current repeat medications
   * Where possible:
     * Discontinued repeat medications from the last 6 months
     * Acute Medications taken in the last year
   * Items in the narrative should be sorted newest to oldest based on recorded date
 * The [International IPS](https://build.fhir.org/ig/HL7/fhir-ips/en/Structure-of-the-International-Patient-Summary.html#medication-summary) says that medication summary may be documented as MedicationStatement or MedicationRequest. However, there is a need to distinguish between repeat medications and acute medications. This distinction is not available directly in a MedicationStatement. Therefore, if MedicationStatement is being used to document medication summary, a MedicationRequest should also be included (as a contained resource, with minimal details) to document the courseOfTherapyType and the last issued date (authoredOn).
 FHIR Resource Profiles to conform to:
   * [UKCore-MedicationStatement](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-MedicationStatement?version=2.0.1)
   * [UKCore-MedicationRequest](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-MedicationRequest?version=2.0.1)
   * [MedicationStatement (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/MedicationStatement-uv-ips)
   * [MedicationRequest (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/MedicationRequest-uv-ips)   

The details of the actual Medication by preference will be carried in the medicationCodeableConcept	element, and represented by a dm+d code. The correct system identifier for dm+d is http://dmd.nhs.uk. A linked Medication resource may be used instead of a medicationCodeableConcept.

 * FHIR Resource Profiles to conform to:
   * [UKCore-Medication](https://simplifier.net/guide/uk-core-implementation-guide-stu2/Home/ProfilesandExtensions/Profile-UKCore-Medication?version=2.0.1)
   * [Medication (IPS)](http://hl7.org/fhir/uv/ips/StructureDefinition/Medication-uv-ips)

**Relevant items from NHSE Urgent Care Dataset**:

 * Data Item: Medication (Current GP Meds and Discharge Meds)
   * PRSB: CIS Medications
   * Must have data items:
      * Medication name - coded value
      * Medication name - free text

**Example:**

* [Example FHIR MedicationStatement](Examples/MedicationStatement.json)
* [Example FHIR MedicationRequest](Examples/MedicationRequest.json)
* [Example HTML Narrative](https://html-preview.github.io/?url=https://github.com/ahatherly-gn/NHS-SCR-IPS/blob/main/Examples/Narrative-Medications.html)

**Notes on distinguishing acute & repeat medication**

*Acute (single issue) Medication:*
- A prescription has been issued with an end of medication period date set (MedicationStatement.effectivePeriod.end) but the end date for the medication period has not yet passed.
- If a prescription has been issued within the last 2 years (MedicationStatement.effectiveDateTime or .effectivePeriod.start), but with no end date.
*Repeat Medication:*
- There is no end date set (MedicationStatement.effectivePeriod.end) for the medication period.
- The medication period end date(MedicationStatement.effectivePeriod.end) is on or after the index date (e.g. today).

And in all cases, the authorised date (MedicationStatement.effectiveDateTime or .effectivePeriod.start) or last issued date (MedicationStatement.extension:medicationStatementLastIssueDate) is within the last two years (whichever is later).
