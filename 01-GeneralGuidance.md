# General Guidance

 * FHIR R4 will be used as the base FHIR version

 * Prior to loading IPS pointers on the National Record Locator, all patient NHS Numbers MUST be traced against PDS. This also applies to updating and deleting pointers.
 
 * Wherever possible codes should be SNOMED, but for England International Patient Summaries, this will use codes from the full UK SNOMED release and **WILL NOT** be limited to the ["IPS Terminology"](https://www.snomed.org/international-patient-summary-terminology) subset created by SNOMED International for use in international summaries.

 * Where clinical data is coded in a system other then SNOMED, these codes can still be used in the coded data (with appropriate system element included). Where data is not coded, the [CodeableConcpet].text element should be used.

 * FHIR Resources included in the IPS bundle **SHOULD** conform to UK Core profiles as well as the relevant profiles within the International Patient Summary implementation guide.

* The source Shared Care Record **MUST** return all relevant data from their ShCR, as per the agreed specification, regardless of potential duplication at the consumer end.

* Source shared care records **SHOULD** return data equivalent to what is provided locally within their own systems.

* Where data is structured, de-duplication **SHOULD** be handled by the consumer system (e.g. Shared care records or NCRS - when ready)

* Systems generating IPSs **SHOULD** retain the same business identifier (resourceType.identifier) each time an IPS for a patient is generated. Further, is a system subsumes data from another system, the original business identifier should be retained, and used in any subsequent interactions. Identifiers should be unique, and be scoped by the owning organisation. There is guidance from GPConnect that may be followed [here](https://webarchive.nationalarchives.gov.uk/ukgwa/20250307021104mp_/https://developer.nhs.uk/apis/gpconnect-1-5-0/accessrecord_structured_development_allergyintolerance.html#identifier)

* Where additional patient information is available from the Spine or other external shared records, there is no requirement for the Shared Care Record generating the summary to attempt to retrieve and incorporate this into the summary that is generated. This applied to (for example) care plans & relevant documents. However, the IPS should have an indication that a Care Plan or relevant documents exist. Where the care plan/relevant document is held on the NRL, then a link to the NRL entry would be the preferred approach. Where care plans/relevant document is held on an accessible system, then a link to the care plan/relevant document should be provided (though this may have authentication issues?). At the very least, there should be some indication that further information exists (even if its not readily accessible), even if this is just a textual statement.

* Composition sections **MUST** be LOINC coded [as per the IPS spec](https://build.fhir.org/ig/HL7/fhir-ips/StructureDefinition-Composition-uv-ips.html), and will use the fixed LOINC codes defined in that specification.
