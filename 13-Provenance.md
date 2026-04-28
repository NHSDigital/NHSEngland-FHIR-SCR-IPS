# Including Provenance Information in IPS

FHIR provides a machanism for recording provenance which is very rich, and if used in different ways in different FHIR implementations.

The international IPS implementation guide mentions that Provenance resources can be used, but does not mandate how or if these should be used, nor does it provide any profiles or example of the use of provenance resources in IPS bundles.

This page sets out some options for how provenance could be recorded in an IPS bundle - a decision will need to be made on the preferred approach to ensure implementers implement this consistently.

A suggested approach could be to require option 1 below, but also allow implementers who are able to provide more in-depth provenance information to adopt option 4 in addition?

## Option 1 - Just document level provenance

The international IPS guide does mandate provenance information is included at the document level, in the form of the document author (likely to be a Device resource as outlinedin this guide).

## Option 2 - Simple Resource level provenance using tags

As a very light-weight addition to the core bundle, meta.tag values could be used to provide very simple provenance information for the resource. This would likely need to include:

 * The Org the data originated from
   * This would include an Org code and System URI as a minimum, which would be an ODS code where possible
   * Ideally it would also include the Org name
 
 In addition we could add a second tag to identify the care setting type (e.g. gp, acute, community) if that is available - this would need to come from an agreed ValueSet

Example of how this could look:

```
    {
        "resourceType": "Condition",
        "id": "ABC123",
        "meta": {
          "tag": [
            {
              "system": "https://fhir.nhs.uk/Id/ods-organization-code",
              "code": "ODS Code",
              "display": "Org Name"
            }
          ]
        },
        ...
```

This has the benefit of being simple, but the main downside is that it would be specific to the English implementation, and it cannot represent more complex chains of provenance or more detailed information about the organisations and practitioners involved in the provenance.

## Option 3 - Contained Provenance Resource

The approach take in the national "Flags" FHIR API on the Spine is to contain a FHIR provenance resource inside each resource. This would allow a full FHIR provenance model to be populated, with the "Entity" being the enclosing resource, and the Agent and Activity to be specified as per the standard FHIR profile.

It is also possible that multiple provenance resources could be included to show a chain of provenance using this approach.

To fully represent practitioners and organisations using this approach would also imply additional Practitioner and Organisation resources either also contained in the resource, or referenced to entities elsewhere in the bundle. Alternatively, if just the textual names are required these could be populated into the display value of the relevant reference field (as is done in the Spine Flags service).

Example of how this could look:

```
    {
        "resourceType": "Condition",
        "id": "ABC123",
        "contained": [
            {
                "resourceType": "Provenance",
                "target": [
                    {
                        "reference": "#"
                    }
                ],
                "recorded": "2018-07-24T10:05:33+00:00",
                "activity": {
                    "coding": [
                        {
                            "system": "http://terminology.hl7.org/CodeSystem/v3-DataOperation",
                            "code": "CREATE",
                            "display": "create"
                        }
                    ]
                },
                "agent": [
                    {
                        "who": {
                            "display": "User Name"
                        },
                        "onBehalfOf": {
                            "display": "Org Name"
                        }
                    }
                ]
            }
        ]
        ...
```

The main downside of this is that where many resources share the same provenance information it would significantly bloat the bundle size through repeating this in every resource.

## Option 4 - Provenance Resources in the Bundle

A more "standard" way of using FHIR provenance resources is to include provenance items as separate items in the bundle, with references to the other items in the bundle that they relate to. In this way, if multiple items share the same provenance information, there only needs to be one provenance resource with links to all of those items.

This still allows for detailed chains of provenance to be recorded if required, but may reduce the duplication of data in the bundle.

Example of how this could look:
```
{
    "resourceType": "Bundle",
    "entry": [
        
        ... various FHIR resources ...
        
        {
            "fullUrl": "https://example/Provenance/P001",
            "resource": {
                "resourceType": "Provenance",
                "id": "P001",
                "target": [
                    { "reference": "Condition/ABC001" },
                    { "reference": "Condition/ABC002" },
                    { "reference": "Condition/ABC003" },
                    { "reference": "MedicationStatement/BCD001" },
                ],
                "recorded": "2018-07-24T10:05:33+00:00",
                "activity": {
                    "coding": [
                        {
                            "system": "http://terminology.hl7.org/CodeSystem/v3-DataOperation",
                            "code": "CREATE",
                            "display": "create"
                        }
                    ]
                },
                "agent": [
                    {
                        "who": {
                            "display": "User Name"
                        },
                        "onBehalfOf": {
                            "reference": "Organization/ORG001"
                        }
                    }
                ]
            }
        },
        {
            "fullUrl": "https://example/Organization/ORG001",
            "resource": {
                "resourceType": "Organization",
                "id": "ORG001",
                "identifier": [
                    {
                    "system": "https://fhir.nhs.uk/Id/ods-organization-code",
                    "value": "ODS Code"
                    }
                ],
                "name": "Org Name",
            }
        }
    ]
}
```