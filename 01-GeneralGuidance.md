# General Guidance

 * FHIR R4 will be used as the base FHIR version
 
 * Wherever possible codes should be SNOMED, but for England International Patient Summaries, this will use codes from the full UK SNOMED release and **WILL NOT** be limited to the ["IPS Terminology"](https://www.snomed.org/international-patient-summary-terminology) subset created by SNOMED International for use in international summaries.

 * FHIR Resources included in the IPS bundle **SHOULD** conform to UK Core profiles as well as the relevant profiles within the International Patient Summary implementation guide.

* The source Shared Care Record **MUST** return all relevant data from their ShCR, as per the agreed specification, regardless of potential duplication at the consumer end.

* Source shared care records **SHOULD** return data equivalent to what is provided locally within their own systems.

* Where data is structured, de-duplication **SHOULD** be handled by the consumer system (e.g. Shared care records or NCRS - when ready)

* Where additional patient information is available from the Spine or other external shared records, there is no requirement for the Shared Care Record generating the summary to attempt to retrieve and incorporate this into the summary that is generated. This applied to (for example) care plans & relevant documents. However, the IPS should have an indication that a Care Plan or relevant documents exist. Where the care plan/relevant document is held on the NRL, then a link to the NRL entry would be the preferred approach. Where care plans/relevant document is held on an accessible system, then a link to the care plan/relevant document should be provided (though this may have authentication issues?). At the very least, there should be some indication that further information exists (even if its not readily accessible), even if this is just a textual statement.

* IPS documents **WILL NOT** include meta.profile attributes when sent over the wire. They may be included in examples in this guide to allow for validation and testing, but would not be included in IPS documents generated in live systems.

* Composition sections **MUST** be LOINC coded [as per the IPS spec](https://build.fhir.org/ig/HL7/fhir-ips/StructureDefinition-Composition-uv-ips.html), and will use the fixed LOINC codes defined in that specification.
