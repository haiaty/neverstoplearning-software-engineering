
# quality attributes

## performance

- response time
- processing speed
- worst case latency
- average latency
- throughput

 Performance scenarios typically involve the following components:

Source of stimulus: Where is the performance demand coming from? It could be from an internal component within the system, a user action, or an external system.

Stimulus: What is the action or event causing a performance demand? This could be a specific API call, a batch process that needs to be run, or a certain number of users attempting to use the system at the same time.

Artifact: What part of the system is being stressed? This might be a specific component or module within the system, or it could be the system as a whole.

Environment: Under what conditions is the performance demand occurring? This could include details about the operating environment, network conditions, or the state of the system at the time of the stimulus.

Response: What is the system's response to the stimulus? This often includes measures like response time, throughput, and resource usage.

Response measure: How is the system's performance being measured? This might involve specific performance metrics or benchmarks, as well as the specific tools or methods being used to measure performance.

Here are some example scenarios that you might consider:

Scenario 1:

Source of stimulus: End user
Stimulus: User submits a complex database query
Artifact: Database
Environment: During peak usage hours
Response: The system executes the query and returns the results
Response measure: Query completes within 2 seconds for 95% of such requests
Scenario 2:

Source of stimulus: External system
Stimulus: External system makes a high volume of API calls
Artifact: System API
Environment: Under normal operating conditions
Response: The system processes API calls and returns responses
Response measure: API maintains a throughput of 1000 requests per second
Scenario 3:

Source of stimulus: Internal system process
Stimulus: A scheduled job to generate complex reports
Artifact: Reporting module
Environment: Late night when user activity is minimal
Response: The system generates the report
Response measure: Reports are generated within 1 hour for 99% of such requests

## availability

- power failures
- failover
- failures of the database

Availability in software architecture refers to the ability of a system or component to be functional and accessible when expected. Availability scenarios often measure the system's uptime, downtime, and the effectiveness of redundancy and failover mechanisms.

Like performance scenarios, availability scenarios usually involve the following components:

Source of stimulus: Where is the demand for availability coming from? This could be an internal system process, a user, or an external system.

Stimulus: What is the action or event that requires system availability? It could be a user request, a system process that needs to run, or an external system trying to access services.

Artifact: What part of the system is required to be available? This could be a specific component or module, or the system as a whole.

Environment: Under what conditions is the availability demand occurring?

Response: How does the system ensure availability in response to the stimulus?

Response measure: How is the system's availability measured? This might include metrics like uptime, failover time, or recovery time.

Here are a few example scenarios:

Scenario 1:

Source of stimulus: User
Stimulus: User attempts to access the system
Artifact: System as a whole
Environment: During peak usage times
Response: The system processes the user request and provides required service
Response measure: The system is available 99.99% of the time during peak hours
Scenario 2:

Source of stimulus: Internal system process
Stimulus: A critical component of the system fails
Artifact: Backup and redundancy systems
Environment: At any time
Response: The system switches to a backup or redundant component
Response measure: Failover occurs within 30 seconds of component failure
Scenario 3:

Source of stimulus: External system
Stimulus: External system makes API calls
Artifact: API services
Environment: Under normal operating conditions
Response: The system processes the API calls and returns responses
Response measure: The API is available 99.95% of the time under normal conditions

## Maintainability

Maintainability in software architecture refers to the ease with which a software system or component can be modified to correct faults, improve performance or other attributes, or adapt to a changed environment. Maintainability scenarios often involve code updates, bug fixes, feature enhancements, and adaptability to new platforms or technologies.

Like other quality attribute scenarios, maintainability scenarios usually involve the following components:

Source of stimulus: This might be a developer, a user who has reported a bug, a system administrator, or an external system.

Stimulus: The stimulus is the reason for the change. It could be a new feature request, a reported bug, a performance enhancement, or a required adaptation to a new technology or platform.

Artifact: What part of the system is to be changed? This could be a single module, a set of interfaces, a database schema, etc.

Environment: Under what conditions is the change being made? The software could be in development, in testing, or in production.

Response: The system is successfully modified.

Response measure: How is the success of the modification measured? This could include measures of time and effort, the number of errors introduced, the impact on other parts of the system, etc.

Here are a few example scenarios:

Scenario 1:

Source of stimulus: Developer
Stimulus: A new feature request
Artifact: The system's user interface
Environment: During development
Response: The feature is added to the system
Response measure: The feature is added within a reasonable amount of time, without introducing new defects or significantly impacting the system's performance
Scenario 2:

Source of stimulus: User who reported a bug
Stimulus: A reported bug
Artifact: A specific module within the system
Environment: While the system is in production
Response: The bug is fixed
Response measure: The bug is fixed quickly, without introducing new defects or causing significant downtime
Scenario 3:

Source of stimulus: System administrator
Stimulus: Requirement to adapt to a new platform
Artifact: The entire system
Environment: When the new platform is introduced
Response: The system is successfully adapted to the new platform
Response measure: The system is adapted with minimal errors and within an acceptable time frame, without impacting its functionality or performance
These scenarios can be used to ensure that a system is maintainable and can evolve to meet new requirements or to correct problems as they are identified.

## security

Security in software architecture refers to the ability of a system or component to resist unauthorized access or modification, and to provide access and modification rights to authorized users. Security scenarios often involve actions like authentication, authorization, data protection, and vulnerability handling.

Like other quality attribute scenarios, security scenarios usually involve the following components:

Source of stimulus: This could be a hacker attempting to gain unauthorized access, a legitimate user trying to access the system, a system process, or an automated security tool.

Stimulus: This is the event that requires a security response. It could be an attempted data breach, an authentication request, a request to change access rights, etc.

Artifact: What part of the system is involved in the security scenario? This could be the system's authentication service, the database, the user interface, etc.

Environment: Under what conditions is the security scenario taking place? The system might be under heavy load, the attempt might be coming from an unfamiliar IP, the data might be highly sensitive, etc.

Response: How does the system respond to the security stimulus? This could be to block an IP, require additional authentication, encrypt data, etc.

Response measure: How is the system's security response measured? This might involve time to detect a breach, percentage of attempts that are successfully blocked, the amount of time a service remains available during a DDoS attack, etc.

Here are some example scenarios:

Scenario 1:

Source of stimulus: Hacker
Stimulus: Attempted data breach
Artifact: Database
Environment: From an unfamiliar IP
Response: The system blocks the IP and alerts the system administrator
Response measure: 99% of such attempts are blocked within 1 minute
Scenario 2:

Source of stimulus: Legitimate user
Stimulus: User attempts to log in
Artifact: Authentication service
Environment: From a known IP
Response: The system checks the user's credentials and, if they are valid, grants access
Response measure: Authentication is completed within 2 seconds for 99% of such requests
Scenario 3:

Source of stimulus: System process
Stimulus: A request to change a user's access rights
Artifact: Authorization service
Environment: During normal operations
Response: The system validates the request and changes the user's access rights
Response measure: 99% of such requests are completed correctly within 5 seconds
These scenarios can help in designing a system with robust security mechanisms and ensure that it reacts effectively to a variety of potential security threats.

## Configurability

Configurability in software architecture refers to the ability of a system or a component to adapt to different environments, user preferences, or system requirements by adjusting various parameters or settings. Configurability scenarios often involve changes to system parameters, installation of new modules, changes to user preferences, and adaptation to different environments or devices.

Like other quality attribute scenarios, configurability scenarios usually involve the following components:

Source of stimulus: This could be a system administrator, a user, or a system process.

Stimulus: This is the event requiring configuration. It could be a change in system requirements, a user preference change, an installation of a new module, etc.

Artifact: What part of the system is being configured? This could be a specific component, a user interface, a database, etc.

Environment: Under what conditions is the configuration change taking place? The system might be under normal operations, in maintenance mode, etc.

Response: The system adapts to the configuration change.

Response measure: How is the success of the configuration change measured? This could be the time to complete the configuration change, the correctness of the change, the system's behavior after the change, etc.

Here are some example scenarios:

Scenario 1:

Source of stimulus: System administrator
Stimulus: Need to adjust system parameters for performance optimization
Artifact: System configuration files
Environment: During a scheduled maintenance window
Response: System parameters are adjusted according to the new settings
Response measure: The change is completed within the maintenance window, and system performance improves as expected
Scenario 2:

Source of stimulus: User
Stimulus: User changes their personal preferences in the application
Artifact: User interface
Environment: During normal system operations
Response: The system applies the new user preferences
Response measure: The changes are applied instantly and persist across different sessions
Scenario 3:

Source of stimulus: System process
Stimulus: Installation of a new software module
Artifact: System's module installation and configuration component
Environment: During normal system operations
Response: The new module is installed and correctly integrated into the system
Response measure: The module is fully operational within an acceptable timeframe, and there's no disruption to other system functions
These scenarios help ensure the system is flexible enough to adapt to varying needs, thus providing a better user experience and easier system maintenance

## Usability

Usability in software architecture refers to the degree to which a system or component can be understood, learned, used, and attractive to the user, when used under specified conditions. Usability scenarios often focus on user interaction with the system and consider factors such as learnability, efficiency, memorability, errors (their frequency, severity, and the ease of recovery from them), and satisfaction.

Like other quality attribute scenarios, usability scenarios usually involve the following components:

Source of stimulus: This is typically a user of the system, but could also be a system process that interacts with the user.

Stimulus: This is the action or event requiring interaction with the user. It could be a user attempting to complete a task, learn a new feature, recover from an error, etc.

Artifact: What part of the system is involved in the interaction? This could be the user interface, a specific feature or functionality, help documentation, etc.

Environment: Under what conditions is the interaction taking place? The user might be unfamiliar with the system, under time pressure, using a particular device or platform, etc.

Response: How does the system support the user in their interaction? This could involve providing intuitive navigation, clear instructions, helpful error messages, easy recovery from mistakes, etc.

Response measure: How is the usability of the system's response measured? This could include time to complete a task, the number of errors made, user satisfaction scores, etc.

Here are some example scenarios:

Scenario 1:

Source of stimulus: New user
Stimulus: User attempts to complete a core task for the first time
Artifact: User interface and help documentation
Environment: User has no previous experience with the system
Response: The system guides the user through the task with clear instructions and intuitive interface design
Response measure: The user is able to complete the task correctly within 10 minutes, with a satisfaction score of at least 4 out of 5
Scenario 2:

Source of stimulus: Regular user
Stimulus: User makes an error while using a feature
Artifact: Error handling component of the feature
Environment: During normal system operations
Response: The system provides a helpful error message and clear recovery instructions
Response measure: The user is able to recover from the error and complete the task with no more than 1 additional error
Scenario 3:

Source of stimulus: User on a mobile device
Stimulus: User attempts to complete a task using the system's mobile interface
Artifact: Mobile interface of the system
Environment: User is on a small screen mobile device with touch input
Response: The system provides a mobile-optimized interface that supports the user in completing the task
Response measure: The user is able to complete the task within a reasonable amount of time and rates the experience at least 4 out of 5
These scenarios can help ensure the system provides a positive user experience and supports users in effectively and efficiently achieving their goals.

## testability

Testability in software architecture refers to the degree to which a system or component facilitates the establishment of test criteria and the performance of tests to determine whether those criteria have been met. Testability scenarios often involve conducting various types of tests, setting up testing environments, and evaluating test outcomes.

Like other quality attribute scenarios, testability scenarios usually involve the following components:

Source of stimulus: This could be a tester, a developer, an automated testing tool, or a continuous integration system.

Stimulus: The stimulus is the need to conduct a test. This could involve running unit tests, performance tests, usability tests, security tests, etc.

Artifact: What part of the system is being tested? This could be a specific module, the whole system, the user interface, etc.

Environment: Under what conditions is the test being conducted? The system could be in a development environment, a test environment, a production-like environment, etc.

Response: The system produces test results and logs that can be analyzed to evaluate the outcome of the test.

Response measure: How is the outcome of the test measured? This could involve the percentage of tests passed, the number and severity of defects found, the amount of code coverage achieved, etc.

Here are a few example scenarios:

Scenario 1:

Source of stimulus: Developer
Stimulus: Need to run unit tests for a module
Artifact: Specific module in the system
Environment: Development environment
Response: The system runs the tests and provides detailed results
Response measure: 95% of the unit tests pass, and detailed logs are produced for the ones that fail
Scenario 2:

Source of stimulus: Continuous integration system
Stimulus: Automated execution of regression test suite after a code check-in
Artifact: Whole system
Environment: Continuous integration pipeline
Response: The system runs the test suite and provides results
Response measure: The test suite completes within 1 hour, with a 98% pass rate and detailed logging for any failures
Scenario 3:

Source of stimulus: Tester
Stimulus: Manual testing of the user interface for usability defects
Artifact: User interface
Environment: Test environment
Response: The system allows the tester to interact with the user interface, produces logs of the interaction, and the tester records any defects
Response measure: All user interface elements are testable, and any defects are logged and reproducible based on the interaction logs
These scenarios help ensure the system can be effectively tested to identify and rectify defects, thereby improving the overall quality of the system.

# How to gather quality attributes scenarios

## Utility tree

a tree where the most important quality attributes (performance, modifiability, availability, etc..)  are the high level nodes 
and scenarios are the leaves.

Each scenario is evaluated both from a business perpective and from an architectural difficulty perspective. The evaluation is (H)igh, (M)edium, (L)ow.
so for example: if a scenario is evaluated as (H, H) this means that the scenario is both important from a business view and from an architectural view therefore it should have
the max priority

## QWA (quality attributes workshop)

Meeting with stakeholders and architects to brainstorm quality attributes (performance, modifiability, availability, etc..) scenarios and prioritize them 
