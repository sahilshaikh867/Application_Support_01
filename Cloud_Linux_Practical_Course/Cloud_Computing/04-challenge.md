# Cloud Computing Independent Challenge

## Project: Highly Available Web Application Design

### Objective

Design a production-style architecture for a simple web application.

## Requirements

The application should:

- Accept internet traffic.
- Run on more than one availability zone.
- Scale when traffic increases.
- Store uploaded files in object storage.
- Use a managed relational database.
- Provide monitoring.
- Follow least-privilege access.
- Have a clear cost-control strategy.

## Student Deliverables

1. Architecture diagram.
2. List of AWS services.
3. Security explanation.
4. Scaling explanation.
5. Monitoring explanation.
6. Backup/recovery explanation.
7. Cost-control plan.
8. Cleanup plan.

## Viva Questions

- Why use a load balancer?
- Why use multiple AZs?
- Why use S3 instead of storing files on EC2?
- What happens if one EC2 instance fails?
- What happens if traffic doubles?
- How would you secure the database?
- How would you detect an outage?
