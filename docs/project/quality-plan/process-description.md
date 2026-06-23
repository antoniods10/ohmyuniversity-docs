# Process Descriptions

The development process follows an Agile/Scrum approach. The work is divided into sprint activities, where the team selects a limited set of requirements, implements them, verifies their behavior, and updates the related documentation.

Each feature must follow a clear workflow:

1. requirement analysis;
2. implementation;
3. local verification;
4. code review;
5. quality check;
6. documentation update.

Before being considered complete, a feature should be consistent with the corresponding functional requirement and with the behavior expected by the user.

The team applies Continuous Integration and Continuous Quality practices. Code quality is checked through static analysis tools, linters, and SonarQube. These tools help identify problems related to security, reliability, maintainability, duplication, and test coverage.

Code reviews are used to verify that the implementation is understandable, coherent with the project architecture, and aligned with the coding conventions.

The team follows consistent coding standards for the technologies used in the project, including Angular for the Web application, Flutter for the Mobile application, and backend/API components where applicable.

Testing activities include functional checks on the main user flows, such as:

- login;
- academic career visualization;
- grade calculation;
- study plan consultation;
- exam session visualization;
- calendar usage;
- access to external services.

When possible, automated tests should be introduced or extended, especially for critical functions that involve academic data, authentication, or calculations.