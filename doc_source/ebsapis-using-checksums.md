# Use checksums<a name="ebsapis-using-checksums"></a>

The GetSnapshotBlock action returns data that is in a block of a snapshot, and the PutSnapshotBlock action adds data to a block in a snapshot\. The block data that is transmitted is not signed as part of the Signature Version 4 signing process\. As a result, checksums are used to validate the integrity of the data as follows:
+ When you use the GetSnapshotBlock action, the response provides a Base64\-encoded SHA256 checksum for the block data using the **x\-amz\-Checksum** header, and the checksum algorithm using the **x\-amz\-Checksum\-Algorithm** header\. Use the returned checksum to validate the integrity of the data\. If the checksum that you generate doesn't match what Amazon EBS provided, you should consider the data not valid and retry your request\.
+ When you use the PutSnapshotBlock action, your request must provide a Base64\-encoded SHA256 checksum for the block data using the **x\-amz\-Checksum** header, and the checksum algorithm using the **x\-amz\-Checksum\-Algorithm** header\. The checksum that you provide is validated against a checksum generated by Amazon EBS to validate the integrity of the data\. If the checksums do not correspond, the request fails\.
+ When you use the CompleteSnapshot action, your request can optionally provide an aggregate Base64\-encoded SHA256 checksum for the complete set of data added to the snapshot\. Provide the checksum using the **x\-amz\-Checksum** header, the checksum algorithm using the **x\-amz\-Checksum\-Algorithm** header, and the checksum aggregation method using the **x\-amz\-Checksum\-Aggregation\-Method** header\. To generate the aggregated checksum using the linear aggregation method, arrange the checksums for each written block in ascending order of their block index, concatenate them to form a single string, and then generate the checksum on the entire string using the SHA256 algorithm\. 

The checksums in these actions are part of the Signature Version 4 signing process\.