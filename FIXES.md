# FIXES.md

Document every issue you find and fix in this file.

---

## Fix 1: [Short title of the issue]
Incorrect Inter-Service Communication

**What was wrong:**
Service B was configured to call Service A using
http://localhost:5000

**Why it is a problem:**
In a Docker environment, each container runs in its own isolated network namespace.
localhost refers to the container itself, not other services. This caused Service B to fail with ECONNREFUSED 127.0.0.1:5000

**How I fixed it:**
Updated the service URL to use Docker Compose service name as below in the dokcer-compose.yaml file
environment:
  - SERVICE_A_URL=http://service-a:5000

This allows Docker’s internal DNS to resolve service-a to the correct container.

**What could go wrong if left unfixed:**
Services cannot communicate. Continuous connection failures. Application becomes non functional in containerized environments.
---

## Fix 2: [Short title of the issue]
Environment Variable Not Reflected in Running Containers
Even after updating docker-compose.yml, the application was still using old configuration.
---
**How I fixed it:**

Rebuilt and recreated containers by below docker commands

docker compose down
docker compose up --build

## Self-initiated Improvements
Add Healthcheck for Service A likebelow in the docker-compose.yaml file

### Improvement 1:

Add Healthcheck for Service A likebelow in the docker-compose.yaml file

healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
  interval: 10s
  retries: 3

This Ensures Ensures Service A is ready before Service B starts using it.

Improves reliability also


### Improvement 2:
Should use Gunicorn inside Service A. So that I hv added 
Install Gunicorn in service-a Dockerfile as below
# Install curl + gunicorn + app dependencies
RUN apt-get update && apt-get install -y curl \
    && pip install --no-cache-dir -r requirements.txt \
    && pip install gunicorn

CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]
