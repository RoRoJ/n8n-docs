---
title: Trigger nodes
description: Learn about trigger nodes in n8n
contentType: overview
---

# Trigger nodes

A trigger node starts a workflow and supplies the initial data. You can use different triggers to start workflows based on different events or conditions. 

Popular triggers include:

- **Application events**, like a page being added to Notion or a message sent in Telegram
- **Webhook calls**, like an HTTP request being received on an exposed URL.
- **Scheduled triggers**, where you set the trigger to fire every day, hour, or other time interval.

The trigger node passes data from the detected event into the workflow to be used by downstream nodes.

You are prompted to add a trigger node as soon as you create a new workflow. A production workflow needs at least one trigger node to determine when the workflow should run. 

Trigger nodes can be distinguished from regular nodes by their rounded edge and bolt icon <span class="n8n-inline-image">![Trigger icon](/_images/common-icons/trigger.png){.off-glb}</span>.

![Screenshot of a workflow with a trigger node](/_images/integrations/builtin/trigger-nodes/trigger-node.png)

## Types of trigger

A trigger can be any of the following:

- [Manual trigger](/integrations/builtin/core-nodes/n8n-nodes-base.manualworkflowtrigger.md): You click a button in n8n. Useful for testing and development.
- [App event](/integrations/builtin/trigger-nodes/index.md): Something happens in an app like Telegram, Notion or Airtable.
- [Scheduled trigger](/integrations/builtin/core-nodes/n8n-nodes-base.scheduletrigger/index.md): The trigger fires every day, hour, or other time interval.
- [Webhook call](/integrations/builtin/core-nodes/n8n-nodes-base.webhook/index.md): An HTTP request is received.
- [Form submission](/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger.md): A webform response is submitted.
- [Execution](/integrations/builtin/core-nodes/n8n-nodes-base.executeworkflowtrigger.md) by another workflow: An Execute Workflow node from a different workflow is called.
- [Chat message](/integrations/builtin/core-nodes/n8n-nodes-langchain.chattrigger/index.md): A chat message is sent to an AI node.
- [Evaluation run](/integrations/builtin/core-nodes/n8n-nodes-base.evaluationtrigger.md): You run a dataset through the workflow to test performance.
- [Email reception](/integrations/builtin/core-nodes/n8n-nodes-base.emailimap.md) (IMAP): A new email is received.
- [Error](/integrations/builtin/core-nodes/n8n-nodes-base.errortrigger.md): Another workflow has an error
- [MCP server trigger](/integrations/builtin/core-nodes/n8n-nodes-langchain.mcptrigger.md): An MCP server connects to and executes a tool node.
- [n8n trigger](/integrations/builtin/core-nodes/n8n-nodes-base.n8ntrigger.md): An event occurs on your n8n instance or the current workflow.
- [SSE](/integrations/builtin/core-nodes/n8n-nodes-base.ssetrigger.md): A Server-Sent Event occurs.

Under the hood, most trigger nodes are either **polling-based** or **webhook-based**:

- Polling-based triggers check periodically to see if a triggering event has occurred. 
- Webhook-based triggers are constantly listening for a triggering event. 

For more details on polling vs. webhook triggers, refer to [Plan your node type](/integrations/creating-nodes/plan/node-types.md). 

## Multiple trigger nodes

You can add multiple trigger nodes to a workflow, but with each execution, only one of them will execute, depending on the triggering event.

## Firing a trigger 

A trigger is fired either:

- **Manually**, when you click the Execute workflow button. This is useful when building and testing workflows. The entire workflow is executed, starting from the trigger. The workflow can be in either **Active** or **Inactive** mode.

/// note | Keep in mind
You can execute only the trigger node and not the entire workflow, by selecting **Execute Step** on the node’s menu.
///

- **Automatically**, when you have switched a workflow’s toggle to **Active**, and a triggering event or schedule occurs. This prompts a production execution of the entire workflow.

## Trigger node data

Trigger nodes do not receive data from any other nodes, as they are the starting point for the workflow.

The output of a trigger node is the data from the triggering event. This data is fed into the next node in the workflow as input data. It is always represented as JSON.

The data output depends on the trigger type. For example:

- Webhook-based triggers output the JSON payload received. 
- Polling-based triggers output the details of the new event that was detected (e.g. details of a received email).
- Schedule triggers output a timestamp.

Refer to the [Data section](/data/index.md) to learn more about data in n8n workflows.

## Writing trigger nodes

The method for writing your own trigger nodes is different to other nodes. Trigger nodes must be built using the programmatic style, not the declarative (JSON-based) style. Only the programmatic style allows them to handle the logic and operations necessary for triggers.

Refer to [Creating nodes](/integrations/creating-nodes/overview.md) for more information.

## Common problems

### Trigger isn’t firing automatically

Ensure your workflow is in [active](/courses/level-one/chapter-5/chapter-5.8.md) mode. Inactive workflows only run when you fire the trigger manually. Your workflow must be activated for the trigger to automatically listen for, or fetch, events.

### Webhook-based trigger isn’t working

- Click the **Listen for test event** button to test the trigger node with a testing URL. The webhook starts listening for a sample event. You can send this event via your external service, curL, or an n8n [HTTP request node](/integrations/builtin/core-nodes/n8n-nodes-base.webhook/common-issues.md#use-the-http-request-node-to-trigger-the-webhook-node) in a different workflow.
- n8n uses different [Webhook URLs](/integrations/builtin/core-nodes/n8n-nodes-base.webhook/index.md) for testing and production. Ensure you have configured the node and the associated external service with the correct Webhook URL type. 
- Refer to [Common problems with Webhook nodes](/integrations/builtin/core-nodes/n8n-nodes-base.webhook/common-issues.md) for further help.

### Polling-based trigger isn’t working

- Click the **Fetch test event** button to perform one immediate polling cycle and get the most recent event found. You can use this to test and debug.
- Ensure the polling interval is set correctly in the trigger node’s settings. Experimenting with a shorter interval may be beneficial.

### Can’t find a suitable trigger type

- Make or upvote an n8n [feature request](https://community.n8n.io/c/feature-requests/5) if you can’t find a trigger type suitable for your needs.