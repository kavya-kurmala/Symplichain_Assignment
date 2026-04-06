# Symplichain Hackathon Submission

## Overview

This repository contains my submission for the Symplichain Software Engineering Intern Hackathon (Part 3 – CI/CD).

Instead of introducing a completely new deployment system, I focused on improving and formalizing the existing EC2-based deployment flow described in the problem statement.

---

## Current Deployment Flow (As Understood)

From the problem description, deployments are already happening like:

- Push to `staging` → Deploy to staging EC2
- Push to `main` → Deploy to production EC2

I’ve recreated the same flow using GitHub Actions in a more structured and repeatable way.

---

## What This Repo Does

The workflows automate:

1. Pulling the latest code from GitHub  
2. Connecting to EC2 using SSH  
3. Updating the application  
4. Restarting the Gunicorn service  

For staging, I’ve also included a dependency installation step to reflect a slightly safer deployment approach.

---

## Design Choices

### Why not Docker?

I considered using Docker, but chose not to include it in the main solution because:
- The current system is already EC2-based
- Introducing Docker without full adoption (including CI/CD + infra changes) can create partial complexity

That said, Docker would be my next step if this system needed better consistency across environments.

---

### Why SSH-based deployment?

This is the simplest approach that aligns with the current setup.

Trade-off:
- Easy to implement and debug  
- But not ideal for large-scale systems (harder to manage multiple servers)

---

## Trade-offs & Limitations

- No zero-downtime deployments (Gunicorn restart causes brief downtime)
- No rollback mechanism
- No health checks after deployment

I chose not to over-engineer these parts since they were not part of the current system description.

---

## What I Would Improve Next

If I were continuing this system:

- Add Docker-based deployments
- Introduce Terraform for infrastructure reproducibility
- Add health checks + rollback logic
- Move towards blue-green deployments

---

## Note

This repository only contains the CI/CD implementation (Part 3).

Detailed system design (Part 1), mobile architecture (Part 2), and debugging approach (Part 4) are included in the submitted PDF.

---

## Final Thought

I’ve intentionally kept the solution simple and aligned with the existing stack, rather than introducing new tools unnecessarily. My goal was to design something that can realistically be implemented quickly by the current team.
