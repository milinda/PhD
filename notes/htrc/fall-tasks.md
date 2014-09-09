# What We Should Do During Fall

## Core Development

1. Restructure source code
2. Fix all the dependency and build issues. All the services should have a client api library, os others can depend on it.
3. Backup and restore (migration) tool for IS, GREG, etc.
4. Docker based continuous integration.
5. Publish stats from all the components to statsd or Netflix Servo.
6. Internal admin console where we can see stats (number of users, worksets, active users), manage users, manage deployment, monitor cluster status.
7. Distributed configuration should be added to all the services.

## Software Process and Concepts

1. Decide how dev stack is different from production stack. Life cycle of new features.
2. Integrate role based authorization
3. Concept of DMZ for HTRC. No authentication internally.
