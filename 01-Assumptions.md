# Assumptions

 * The produced IPS will follow [International Patient Summary Implementation Guide 2.0.0 - STU 2](https://build.fhir.org/ig/HL7/fhir-ips/index.html)
 
 * FHIR R4 will be used as the base FHIR version
 
 * Wherever possible codes should be SNOMED, but for England International Patient Summaries, this will use codes from the full UK SNOMED release and **WILL NOT** be limited to the ["IPS Terminology"](https://www.snomed.org/international-patient-summary-terminology) subset created by SNOMED International for use in international summaries.

 * FHIR Resources included in the IPS bundle **SHOULD** conform to UK Core profiles as well as the relevant profiles within the International Patient Summary implementation guide.

* The source ShCR must return all relevant data from their ShCR, as per the agreed specification, regardless of potential duplication at the consumer end.

* Source shared care records must return data equivalent to what is provided locally within their own systems.

* Where data is structured, de-duplication should be handled by the consumer system (e.g. Shared care records or NCRS - when ready)

* Where additional patient information is available from the Spine or other external shared records, there is no requirement for the Shared Care Record generating the summary to attempt to retrieve and incorporate this into the summary that is generated.
