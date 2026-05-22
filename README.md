# AWS Infrastructure Playbook

Production AWS patterns from building [GG Loop](https://ggloop.io) - a proof-of-effort gaming rewards platform. Not tutorials. Real infrastructure in production.

## Stack

| Service | Use | Cost/mo |
|---------|-----|---------|
| App Runner | Node.js API, auto-scaling | ~$25-50 |
| RDS PostgreSQL | VPC-private database | ~$15-25 |
| SES | Transactional email (OTP, receipts) | ~$0.10/1k |
| Bedrock | Claude AI via managed API | ~$5-15 |
| Lambda | Event-driven background jobs | ~$1 |
| Route 53 | DNS + health checks | ~$1 |

## Architecture

```
Internet -> Route 53 -> App Runner (Node.js API, port 8080)
                              |
                        VPC Private Subnet
                        +-- RDS PostgreSQL (no public access)

App Runner -> SES (transactional email)
App Runner -> Bedrock (Claude AI)
App Runner -> S3 (asset storage)
Lambda    -> RDS (scheduled jobs)
```

## Patterns

- `iam/policy.json` - Least-privilege IAM for Node.js SaaS backend
- `cicd/deploy.yml` - GitHub Actions to App Runner (zero-downtime)
- `patterns/app-runner.md` - Health checks, auto-scaling, env vars
- `patterns/rds-private.md` - VPC setup, connection pooling, backups
- `patterns/ses-email.md` - Domain verification, DKIM, sandbox exit
- `patterns/bedrock.md` - Claude integration, prompt caching, cost control

## About

Built by [Jayson Quindao](https://github.com/djjrip), Founder & Full-Stack Engineer at [GG Loop LLC](https://ggloop.io).

Stack: TypeScript, Node.js, React, PostgreSQL, Drizzle ORM, AWS, Tauri/Rust, Electron
