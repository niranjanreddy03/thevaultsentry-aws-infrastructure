# Deployment Notes

## Frontend Deployment with AWS Amplify

The frontend is deployed using AWS Amplify hosting. Amplify can be connected to a source repository or configured to deploy a prepared build artifact. The frontend should use environment-specific configuration for backend API URLs and should not include secrets in client-side files.

Deployment steps:

- Connect the frontend repository or upload the prepared frontend build.
- Configure public environment variables such as the backend API URL.
- Deploy the production branch or selected build artifact.
- Review Amplify deployment logs and verify that the frontend loads over HTTPS.

## Backend Deployment with Elastic Beanstalk

The backend is deployed to an Elastic Beanstalk application environment. Elastic Beanstalk manages the application lifecycle and provisions the EC2 compute resources used by the backend.

Deployment steps:

- Package the backend application files required by the runtime.
- Create or update the Elastic Beanstalk application version.
- Deploy the application version to the target environment.
- Verify Elastic Beanstalk health status and review backend application logs.

## CloudFront CDN Integration

CloudFront acts as the public CDN layer and provides HTTPS access for users. It should be configured with appropriate cache behaviors for frontend assets and backend API traffic.

Configuration steps:

- Configure the CloudFront distribution for HTTPS viewer connections.
- Use separate cache behavior for frontend assets and backend API traffic.
- Avoid caching authenticated or sensitive API responses.
- Run cache invalidations when frontend assets are updated.

## IAM Permission Handling

IAM permissions should be scoped to the minimum access required for deployment and runtime behavior.

Configuration steps:

- Use separate IAM roles for deployment access, Elastic Beanstalk service access, and EC2 instance profiles.
- Scope policies to the AWS services and resources required by the project.
- Prefer role-based access where possible.
- Keep long-lived access keys out of source control and local documentation.

## CloudWatch Monitoring

CloudWatch should be used for operational visibility across the frontend deployment process, Elastic Beanstalk environment, and EC2 instances.

Monitoring setup:

- Review Elastic Beanstalk application logs and environment health events.
- Track EC2 instance metrics and backend application behavior.
- Configure log retention for CloudWatch log groups.
- Add alarms for unhealthy environment status, high error rates, or unusual resource usage.

## SSL Configuration

HTTPS should be configured using AWS Certificate Manager and attached to the CloudFront distribution.

Configuration steps:

- Request or attach a valid AWS Certificate Manager certificate.
- Associate the certificate with the CloudFront distribution.
- Redirect HTTP requests to HTTPS.
- Verify that frontend and backend API URLs use HTTPS.
