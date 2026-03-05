## Problem 4: Solving bug issue in the system

## Problems that ive found

### Bug 1: Port wrong in NGINX Config
- API runs on port 3000 but in the nano NGINX its pointing to port 3001
- Upon requests, the 502 Bad Gateway error prompts.

### Bug 2: Postgres and Redis without Healthchecks
- Instead of waiting containers to be ready, depends_on only waits for containers to start.
- Since postgres/redis weren't ready, the API starts to crash.
- So it causes the API to be intermittent.

## How to Diagnose
- Type curl http://localhose:8080/api/users and ran docker compose up
- The error 502 Bad Gateway prompts.
- Checked nginx/conf.d/default.conf and saw port 3001
- Checked api/src/index.js and saw API listens on port 3000
- Added healthchecks to fix startup conditions.

## Applied some fixes
1. Changed proxy_pass http://api:3001 to proxy_pass http//api:3000
2. Added healthchecks to postgres and redis in docker-compose.yml
3. Changed depends_on to wait for healthy service condition
4. Added restart: always to all services so they ayto-recover.

## Monitoring stacks
- Alert when API returns 5xx errors more than 5 times in a minute
- Alert when containers restarts unexpectedly
- Alert when postgres/redis healthcheck fails

## How to Prevent In Production
- For docker-compose or Kubernetes, always use healthchecks
- Catch port mismatches before production by using a staging environment
- After deployment, add automated tests that hit every endpoint 
