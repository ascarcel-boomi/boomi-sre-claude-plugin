---
name: verify-build
description: Run the appropriate build command for the current project and report results
---

Detect the project type and run the appropriate build/verification command:

- **Package.swift** → `swift build`
- **package.json** → `npm run build` or `npm test`
- **requirements.txt / pyproject.toml** → `python3 -m py_compile` on changed files
- **Terraform (*.tf)** → `terraform validate`
- **Ansible (playbook*.yml)** → `ansible-playbook --syntax-check`

Report: build status, any errors, and suggestions for fixing them.
