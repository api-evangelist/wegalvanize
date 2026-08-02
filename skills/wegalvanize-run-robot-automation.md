---
name: Automate work with HighBond Robots
description: Create a robot, define a task on it, run the task on demand, and monitor the resulting task runs (jobs).
api: openapi/wegalvanize-highbond-openapi-original.yml
operations: [getRobots, createRobot, createRobotTask, runRobotTask, getRobotJobs]
---

# Automate work with HighBond Robots

Robots run Analytics scripts against data on a schedule or on demand. JSON:API
v1.0; `Authorization: Bearer <TOKEN>`; target the region host. Writes are not
idempotent.

## Steps

1. **List robots** — `getRobots` to find an existing robot, or `createRobot` to
   provision one in a folder.
2. **Create a task** — `createRobotTask` on the robot, configuring the script
   and inputs.
3. **Run the task** — `runRobotTask` to execute it immediately (ad hoc).
4. **Monitor runs** — `getRobotJobs` to poll the task runs (jobs) and read
   their status/output.

## Notes
- A robot job is a single execution of a robot task; poll rather than assume
  synchronous completion.
- Handle `403` (permission), `422` (invalid task config), and `429` (rate
  limit) explicitly.
