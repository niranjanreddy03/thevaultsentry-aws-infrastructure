# Deployment Notes

## Frontend Deployment with AWS Amplify

The frontend is deployed using AWS Amplify hosting. Amplify can be connected to a source repository or configured to deploy a prepared build artifact. The frontend should use environment-specific configuration for backend API URLs and should not include secrets in client-side files.

Recommended checks:

- Confirm the production branch or deployment artifact is correct before publishing.
- Store only public frontend configuration in Amplify environment variables.
- Verify that the frontend calls backend APIs over HTTPS.
- Review Amplify deployment logs after each release.

## Backend Deployment with Elastic Beanstalk

The backend is deployed to an Elastic Beanstalk application environment. Elastic Beanstalk manages the application lifecycle and provisions the EC2 compute resources used by the backend.

Recommended checks:

- Package only required backend application files.
- Keep runtime configuration in Elastic Beanstalk environment properties.
- Review Elastic Beanstalk health status after deployment.
- Confirm backend routes do not expose debug output, credentials, or administrative functionality.

## CloudFront CDN Integration

CloudFront acts as the public CDN layer and provides HTTPS access for users. It should be configured with appropriate cache behaviors for frontend assets and backend API traffic.

Recommended checks:

- Enforce HTTPS viewer connections.
- Avoid caching authenticated or sensitive API responses.
- Configure cache invalidations when frontend assets change.
- Add response headers for browser security where appropriate.

## IAM Permission Handling

IAM permissions should be scoped to the minimum access required for deployment and runtime behavior.

Recommended checks:

- Use separate IAM roles for deployment users, Elastic Beanstalk service access, and EC2 instance profiles.
- Avoid broad wildcard permissions unless they are temporary and justified.
- Rotate access keys used for deployment workflows.
- Prefer role-based access over long-lived user credentials.

## CloudWatch Monitoring

CloudWatch should be used for operational visibility across the frontend deployment process, Elastic Beanstalk environment, and EC2 instances.

Recommended checks:

- Review Elastic Beanstalk application logs and environment health events.
- Enable useful EC2 and application metrics.
- Configure log retention instead of leaving logs indefinitely.
- Add alarms for high error rates, unhealthy environment status, and unusual resource usage.

## SSL Configuration

HTTPS should be configured using AWS Certificate Manager and attached to the CloudFront distribution.

Recommended checks:

- Use a valid ACM certificate in the correct region for CloudFront.
- Redirect HTTP traffic to HTTPS.
- Verify the frontend and backend API URLs use HTTPS.
- Confirm certificates are renewed and attached to the expected distribution.
