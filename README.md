# CSC418 Lab 11 — Jenkins Selenium Testing and Multi-stage CI/CD Pipeline

Student: Bassam Zafar
Course: CSC418 — Cloud Computing

Pipeline: GitHub push → Jenkins
Checkout → Build (node:22-alpine) → Package (Docker image) →
Start Staging (port 8090) → Test (Selenium test agent) →
Deploy (port 80, only if all tests pass)

Run the tests by hand (see Lab 11 Part 3):
```
docker build -t selenium-test-agent tests/
docker run --rm -u $(id -u):$(id -g) -e HOME=/tmp -v "$PWD":/app -w /app node:22-alpine sh -c "npm install && npm run build"
docker build -t lab11-site:manual .
docker run -d --name lab11-staging -p 8090:80 lab11-site:manual
docker run --rm --network host -u $(id -u):$(id -g) -e HOME=/tmp -e BASE_URL=http://localhost:8090 -v "$PWD":/work -w /work selenium-test-agent pytest tests/test_site.py -v
```

<!-- Task 8: webhook test - harmless comment to trigger the Jenkins webhook -->

<!-- Task 8: second webhook test - verify automatic build #8 -->
