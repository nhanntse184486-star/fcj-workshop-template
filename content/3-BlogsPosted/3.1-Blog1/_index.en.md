---
title: "Blog 1"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## CI/CD on AWS: Integrating DevSecOps Before Production Deployment

### 1. Problem and objective

Release speed matters little if new versions introduce security weaknesses. DevSecOps embeds risk checks into development so that problems can be addressed at the stage where they arise rather than only after deployment.

### 2. Designing an AWS pipeline

Reference flow: `Git → CodePipeline → CodeBuild → Staging → Security tests → Manual approval → Production`.

| Component | Responsibility |
|---|---|
| AWS CodePipeline | Orchestrate release stages |
| AWS CodeBuild | Build, unit testing and scanner execution |
| Amazon S3 | Store pipeline artifacts |
| AWS CodeDeploy | Coordinate deployment on compatible platforms |
| Amazon ECS | Run container workloads in the reference architecture |
| Amazon CloudWatch | Monitor logs and execution results |

### 3. Security checks

| Method | Timing and purpose |
|---|---|
| SCA | Inspect dependencies for known vulnerabilities |
| SAST | Analyze source code without running the application |
| DAST | Test a running application in an authorized staging environment |
| IAST | Analyze an instrumented application during execution |

Define release-blocking criteria, such as unresolved critical findings. Retain scan reports for review. Scanners require separate configuration; CodePipeline does not perform every security test by itself.

### 4. Reference implementation workflow

1. Connect the Git repository and configure the release branch.
2. Create a build project and run unit tests, SCA and SAST. Fail the build when security policy is violated.
3. Package the artifact and deploy the built revision to staging.
4. Run DAST and IAST in a suitable test environment and collect findings.
5. Add a manual approval gate before production. Deploy the tested artifact after approval.
6. Monitor post-release errors and prepare a rollback to a stable revision.

### 5. Monitoring, permissions and risk control

Use separate least-privilege IAM roles for pipeline stages and never store access keys in Git. Parameter Store or Secrets Manager can protect sensitive configuration. CloudTrail supports API activity auditing, AWS Config tracks resource configurations, and Security Hub consolidates findings from integrated sources.

Suggested validation cases include vulnerable dependencies, failed builds, unauthorized approvals and faulty staging releases. A pipeline should block unacceptable revisions instead of deploying them.

### 6. Connection to the internship project

For Driver Drowsiness & Distraction Detection, pre-release checks can be applied to Lambda code, the dashboard frontend and Python dependencies at the edge. This reduces the risk of committing secrets or releasing vulnerable dependencies. This is a potential application of the principles, not a claim that the project uses CodePipeline or ECS.

### 7. Conclusion

DevSecOps promotes accountable release workflows by automating repeatable checks, defining acceptance conditions and keeping deployments traceable.

### References

- [AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)
- [Manual approval](https://docs.aws.amazon.com/codepipeline/latest/userguide/approvals.html)
- [AWS CodeBuild](https://docs.aws.amazon.com/codebuild/)
- [AWS Security Hub](https://docs.aws.amazon.com/securityhub/)
- [AWS CloudTrail](https://docs.aws.amazon.com/cloudtrail/)
