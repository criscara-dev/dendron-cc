---
id: 18b8b7a9-a908-4f43-b74e-4bc6aa189187
title: Flow
desc: ''
updated: 1618396148323
created: 1611246491976
---

# Flow

Simple informations [here](https://trailhead.salesforce.com/content/learn/modules/flow-basics/meet-flow-builder?trail_id=build-flows-with-flow-builder) 
Recap:
Areas:
1. toolbox
2. canvas
3. button bar

Building blocks:
1. Elements: Each element is a step in the flow that instructs the flow on what to do. Can be broken down into:
    - screens
    - logic
    - actions
2. Connectors: Connectors define the path that the flow takes as it runs. They tell the flow which element to execute next.
3. Resources: Resources are placeholders that you reference throughout your flow. For example, look up an account’s ID, store that ID in a variable, and later reference that ID to update the account


Steps to create a flow:
1. Plan Out the Flow

    Requirement	Element | Type to Use
    --- | ---
    requirement 1 | ???
    ... | ...
2. create the Flow
3. test the flow

---

==Examples automation with flow:==

- **Avoid to frequently create duplicate contacts**:
    Capture values for only the required fields (First Name and Last Name) and the associated account.
    If there’s a matching contact, update the existing one. If there isn’t a matching contact, create one.
    - check main Admin org or this [Build a Simple Flow](https://trailhead.salesforce.com/en/content/learn/projects/build-a-simple-flow?trail_id=build-flows-with-flow-builder) Trailhead.