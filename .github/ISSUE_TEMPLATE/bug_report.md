name: Bug Report
description: Report a bug or issue with Cinematique
title: "[BUG] "
labels: ["bug"]
assignees: []

body:
  - type: markdown
    attributes:
      value: |
        Thank you for reporting a bug! Please provide as much detail as possible to help us understand and fix the issue.

  - type: textarea
    id: description
    attributes:
      label: Description
      description: Clear description of what the bug is
      placeholder: I experienced a bug where...
    validations:
      required: true

  - type: textarea
    id: reproduce
    attributes:
      label: Steps to Reproduce
      description: Steps to reproduce the behavior
      placeholder: |
        1. Go to '...'
        2. Click on '....'
        3. Scroll down to '....'
        4. See error
    validations:
      required: true

  - type: textarea
    id: expected
    attributes:
      label: Expected Behavior
      description: What should happen instead?
      placeholder: I expected the application to...
    validations:
      required: true

  - type: textarea
    id: actual
    attributes:
      label: Actual Behavior
      description: What actually happens?
      placeholder: Instead, the application...
    validations:
      required: true

  - type: textarea
    id: screenshots
    attributes:
      label: Screenshots
      description: If applicable, add screenshots to help explain the problem
      placeholder: Drag and drop images here

  - type: dropdown
    id: os
    attributes:
      label: Operating System
      options:
        - Windows
        - macOS
        - Linux
        - Other
    validations:
      required: true

  - type: input
    id: java_version
    attributes:
      label: Java Version
      description: Output of `java -version`
      placeholder: "openjdk version \"16.0.1\" 2021-04-20"
    validations:
      required: true

  - type: input
    id: mysql_version
    attributes:
      label: MySQL Version
      description: Output of `mysql --version`
      placeholder: "mysql Ver 8.0.25-0ubuntu0.21.04.1"
    validations:
      required: false

  - type: textarea
    id: logs
    attributes:
      label: Relevant Error Logs
      description: Paste any error messages or stack traces
      render: java

  - type: textarea
    id: additional
    attributes:
      label: Additional Context
      description: Any other context about the problem

  - type: checkboxes
    id: terms
    attributes:
      label: Checklist
      options:
        - label: I have searched existing issues
          required: true
        - label: I have provided detailed steps to reproduce
          required: true
        - label: I have included my environment details
          required: true
