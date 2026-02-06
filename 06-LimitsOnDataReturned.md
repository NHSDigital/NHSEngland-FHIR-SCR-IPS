# Limits on Data Returned

Some of the sections in the IPS cover data which may go back a long way, and for which a very large number of items could potentially be included.

Where possible, sensible date ranges are outlined in this specification to limit the data returned, but there could still be cases where patients have an especially large amount of data within those data ranges. For example, some patients may have a very large number of encounters with healthcare services, or they may have had a very large number of test results. In order to retain a sensible size to the IPS in terms of test results, the initial rule proposed is to populate the last 6 months of test results or last 5 test results, whichever is the smallest. In terms of observations, the last 5 should be included.

To prevent these scenarios resulting in excessively large "summary" documents being returned, this specification also recommends limits to the number of individual resources that would be returned in any given section of the IPS. If the amount of data that would be returned is greater that this limit, the narrative will show a human-readable indication at the end of the list of resources that additional data was available but was not returned.
