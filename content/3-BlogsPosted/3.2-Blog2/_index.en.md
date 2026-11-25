---
title: "Blog 2"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

## Amazon S3: Storing, Protecting and Delivering Data on AWS

### 1. Amazon S3 overview

Amazon S3 provides object storage for application files, documents, analytical datasets, backups and static website assets. Unlike a disk attached to a server, applications interact with objects through API operations.

### 2. Data structure

A bucket is a container, an object is data with metadata, and an object key identifies the object within a bucket. For example, `dashboard/assets/app.js` identifies a JavaScript file. Slashes organize keys into prefixes without requiring physical directories.

### 3. Selecting a storage class

| Storage class | Suitable workload |
|---|---|
| S3 Standard | Frequently accessed data |
| S3 Intelligent-Tiering | Changing access patterns |
| S3 Standard-IA | Infrequent access with fast retrieval |
| S3 One Zone-IA | Re-creatable data stored in one AZ |
| S3 Glacier Instant Retrieval | Long-term storage with fast retrieval |
| S3 Glacier Flexible Retrieval | Archives that can tolerate restoration time |
| S3 Glacier Deep Archive | Very rarely accessed archives |
| S3 Express One Zone | Low-latency workloads using directory buckets |

Consider retrieval charges, minimum storage durations and recovery requirements before choosing a class.

### 4. Management and security

- Keep Block Public Access enabled for private buckets; grant only necessary IAM and bucket-policy permissions.
- Use server-side encryption and consider SSE-KMS for suitable key management and audit requirements.
- Use Versioning to help recover from accidental overwrites or deletions; manage older versions using Lifecycle.
- Configure Lifecycle transitions or expiration according to retention requirements.

### 5. Hands-on guide: secure frontend delivery

1. Create a general purpose bucket in the selected Region and keep Block Public Access enabled.
2. Upload HTML, CSS and JavaScript for a sample website and inspect the content types.
3. Create a CloudFront distribution using an S3 REST origin (not the S3 website endpoint) and Origin Access Control (OAC).
4. Update the bucket policy to allow the specified CloudFront distribution to read required objects.
5. Set `index.html` as the default root object, use HTTPS and test the CloudFront domain.
6. Verify that unauthorized direct object access is denied while CloudFront can deliver the content. Remove test resources when no longer needed.

### 6. Data and machine learning applications

S3 can store datasets, model artifacts and analytical outputs. Organize data by prefixes, grant processing permissions separately and apply retention policies. Costs depend on capacity, requests, retrieval and data transfer.

### 7. Connection to the internship project

In the Driver Drowsiness & Distraction Detection architecture, S3 holds dashboard frontend assets and CloudFront delivers them through OAC. Webcam video is not uploaded to S3; event metadata is stored in DynamoDB. This separation aligns with the project's privacy and data-transfer goals.

### 8. Conclusion

Effective S3 design identifies what is stored, who can read it, how long it remains and how it is accessed. A bucket should not be made public simply to simplify website deployment.

### References

- [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [S3 security](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security.html)
- [S3 Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)
- [CloudFront private S3 origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
