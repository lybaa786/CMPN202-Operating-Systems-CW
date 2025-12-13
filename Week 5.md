Phase 5: Advanced Security and Monitoring Infrastructure

I implemented access control using AppArmor to restrict program behaviour and improve system security.
Each application has a profile that defines what it is allowed to do. These profiles control things like file access, permissions, and network use.
AppArmor runs in either Enforce mode (blocks violations) or Complain mode (only logs them).

Step 1: Verifying AppArmor is Running:


Step 2: Review Active Profiles:

Step 3: Checking SSH Profile Specifically:

Step 4:


