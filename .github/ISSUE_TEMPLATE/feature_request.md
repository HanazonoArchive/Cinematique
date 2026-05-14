name: Feature Request
description: Suggest an improvement or new feature
title: "[FEATURE] "
labels: ["enhancement"]
assignees: []

body:
  - type: markdown
    attributes:
      value: |
        Thank you for suggesting an improvement! We'd love to hear your ideas.

  - type: textarea
    id: description
    attributes:
      label: Feature Description
      description: Clear and concise description of the feature
      placeholder: I would like the application to...
    validations:
      required: true

  - type: textarea
    id: motivation
    attributes:
      label: Motivation & Use Case
      description: Why is this feature needed? What problem does it solve?
      placeholder: This feature would help because...
    validations:
      required: true

  - type: textarea
    id: solution
    attributes:
      label: Proposed Solution
      description: Describe how you envision this feature working
      placeholder: The feature could work by...
    validations:
      required: false

  - type: textarea
    id: alternative
    attributes:
      label: Alternative Solutions
    validations:
      required: false

  - type: textarea
    id: additional
    attributes:
      label: Additional Context
      description: Any other context, mockups, or examples

  - type: checkboxes
    id: terms
    attributes:
      label: Checklist
      options:
        - label: I have searched existing issues for similar requests
          required: true
