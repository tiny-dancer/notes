# The Software Engineer's Handbook

## What I learned by working for the largest US Healthcare company. 


- Performance Tests
- Quality Tests
  - Code Analsysis
- Security Tests
  - SAST, DAST, IAST,
  - Pen Tests
  - Code Scans

Security
- Peremeter Control
- Compute baselines (VM vulerabilies)
- TVM
- SIR
- Configuration Security
- Intrusion detection

Testing Pyramid
- Unit Tests
  - Everything is mocked, once you learn the unit test framework and understand how to mock in most situations, these should take no more than a minute or two to write each test.
- Functional/Integration Tests
  - These are usually built using the same framework as the unit test framework.  Here only some or none of the depedencies are mocked and you're actually verifying against real depedencies.  These usually take longer as they entail more set up and clean up, for example after writing to a real database you'll want to clean up or teardown the database after test execution. 
- Acceptance/E2E Tests

Everything as Code

Configuration Management (VM's)
- Chef, ansible, etc.
- Immutable (e.g. Packer)

Infrastracture as Code
- Terraform (Cross-Provider)
- Cloudformations (AWS)
- ARM Templates (Azure)

Pipeline as Code
- Jenkins

Policy as Code
- Hashi Vault, Config Rules, Azure Policy
