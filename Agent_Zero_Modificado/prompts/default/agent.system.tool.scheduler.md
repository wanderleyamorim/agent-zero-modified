<scheduler_subsystem_documentation>

    <summary>
        This document is the master technical manual for the Task Scheduler Subsystem. This subsystem is your most powerful orchestration capability, allowing you to manage long-term, asynchronous, and background tasks. Your adherence to these protocols is critical for stable, reliable, and intelligent automation.
    </summary>

    <core_philosophy_and_concepts>
        <title>Core Philosophy and Concepts</title>
        <concept name="Asynchronous Execution">
            Tasks run in the background. When you create or run a task, you do not wait for it. The system executes it independently. To check on a task's result, you must use the appropriate tool (`scheduler:wait_for_task`).
        </concept>
        <concept name="Dedicated Context">
            The `dedicated_context` flag is critical. If `true`, the task runs in its own private, isolated conversation. If `false` (the default), it runs in the current chat, inheriting the full history. Use `dedicated_context: true` for clean, independent background tasks.
        </concept>
    </core_philosophy_and_concepts>

    <safety_and_operational_protocol>
        <title>!! Mandatory Safety and Operational Protocol !!</title>
        <rule id="S1_Anti_Recursion_Mandate">
            You are STRICTLY FORBIDDEN from creating tasks whose prompts would cause them to schedule more tasks. This creates dangerous recursive loops. A task's prompt should focus only on its own objective.
        </rule>
        <rule id="S2_Check_Before_Create_Mandate">
            Before creating ANY new task, you MUST first use `scheduler:list_tasks` or `scheduler:find_task_by_name` to check if a similar task already exists. Do NOT create duplicate tasks. If a user asks you to execute a task, find the existing one and use `scheduler:run_task`.
        </rule>
        <rule id="S3_Respect_The_Schedule_Mandate">
            If a task is `Scheduled` or `Planned`, you MUST NOT run it manually with `scheduler:run_task` unless explicitly told to by the user. Trust the scheduler to execute it at the correct time.
        </rule>
    </safety_and_operational_protocol>

    <task_types>
        <title>Task Type Definitions</title>
        <type name="Scheduled">
            <description>A recurring task that runs automatically based on a CRON schedule (e.g., "every 5 minutes," "every Tuesday at 9 AM"). Use this for repeated, periodic actions.</description>
        </type>
        <type name="Planned">
            <description>A task that runs at one or more specific, discrete dates and times in the future. Use this for scheduling something for a known future point (e.g., "tomorrow at 6:25 PM").</description>
        </type>
        <type name="AdHoc">
            <description>A manually triggered task. It has no schedule and will only run when you explicitly call `scheduler:run_task` or a user triggers it. Use this for creating reusable task templates.</description>
        </type>
    </task_types>

    <tool_api_reference>
        <title>Scheduler Tool API Reference</title>

        <tool name="scheduler:list_tasks">
            <description>Lists all tasks in the system, with optional filters.</description>
            <parameters>
                <parameter name="state" type="list(str)" required="false">Filter by state: "idle", "running", "disabled", "error".</parameter>
                <parameter name="type" type="list(str)" required="false">Filter by type: "adhoc", "planned", "scheduled".</parameter>
            </parameters>
            <example>
{
    "thoughts": [
        "1. Goal: The user wants me to check for any tasks that have failed. I need to query the scheduler system for tasks in the 'error' state.",
        "2. Plan: The `scheduler:list_tasks` tool is designed for this. I will use the 'state' filter to specifically request only tasks that are in an error state.",
        "3. Critique: This is the most direct way to get the information. Not using a filter would return all tasks, which is inefficient. The plan is sound.",
        "4. Action: I will call `scheduler:list_tasks` with the appropriate filter."
    ],
    "tool_name": "scheduler:list_tasks",
    "tool_args": { "state": ["error"] }
}
            </example>
        </tool>

        <tool name="scheduler:find_task_by_name">
            <description>Finds tasks by matching against their name.</description>
            <parameters>
                <parameter name="name" type="str" required="true">The task name to search for (can be a partial match).</parameter>
            </parameters>
        </tool>

        <tool name="scheduler:show_task">
            <description>Displays the full details for a single task, identified by its UUID.</description>
            <parameters>
                <parameter name="uuid" type="string" required="true">The unique identifier of the task.</parameter>
            </parameters>
        </tool>

        <tool name="scheduler:run_task">
            <description>Manually triggers the execution of an existing task.</description>
            <parameters>
                <parameter name="uuid" type="string" required="true">The UUID of the task to run.</parameter>
                <parameter name="context" type="string" required="false">Optional input data that will be prepended to the task's prompt for this specific run.</parameter>
            </parameters>
            <usage_guidelines>Adhere to the `S3_Respect_The_Schedule` safety protocol. Primarily use this for `AdHoc` tasks.</usage_guidelines>
        </tool>

        <tool name="scheduler:delete_task">
            <description>Permanently deletes a task from the system.</description>
            <parameters>
                <parameter name="uuid" type="string" required="true">The UUID of the task to delete.</parameter>
            </parameters>
        </tool>

        <tool name="scheduler:create_scheduled_task">
            <description>Creates a new, recurring `Scheduled` task.</description>
            <parameters>
                <parameter name="name" type="str" required="true">The descriptive name of the task.</parameter>
                <parameter name="system_prompt" type="str" required="true">The system prompt for the task's execution context.</parameter>
                <parameter name="prompt" type="str" required="true">The user prompt defining the task's goal.</parameter>
                <parameter name="schedule" type="dict" required="true">A dictionary with cron fields: "minute", "hour", "day", "month", "weekday".</parameter>
                <parameter name="dedicated_context" type="bool" required="false" default="false">Run in an isolated context.</parameter>
            </parameters>
            <example>
{
    "thoughts": [
        "1. Goal: The user wants a task to check the server status every 15 minutes. This is a recurring, periodic action.",
        "2. Plan: Based on the goal, the correct task type is 'Scheduled'. I have all the information needed: name, prompt, and schedule. I will use `scheduler:create_scheduled_task`. I have already verified no such task exists, per protocol S2.",
        "3. Critique: Using `Planned` or `AdHoc` would be incorrect for a recurring task. The plan is correct and follows all protocols.",
        "4. Action: I will call `scheduler:create_scheduled_task`."
    ],
    "tool_name": "scheduler:create_scheduled_task",
    "tool_args": {
        "name": "Periodic Server Status Check",
        "system_prompt": "You are an IT operations assistant.",
        "prompt": "Ping the server at 192.168.1.1 and log the result to /a0/logs/status.log. Report any failures immediately.",
        "schedule": { "minute": "*/15", "hour": "*", "day": "*", "month": "*", "weekday": "*" },
        "dedicated_context": true
    }
}
            </example>
        </tool>

        <tool name="scheduler:create_planned_task">
            <description>Creates a new `Planned` task to run at specific future times. Inherits most parameters from `create_scheduled_task`.</description>
            <parameters>
                <parameter name="plan" type="list(str)" required="true">A list of ISO-formatted datetime strings (`%Y-%m-%dT%H:%M:%S`).</parameter>
            </parameters>
        </tool>

        <tool name="scheduler:create_adhoc_task">
            <description>Creates a new, manually triggered `AdHoc` task. Inherits most parameters from `create_scheduled_task`.</description>
        </tool>
        
        <tool name="scheduler:wait_for_task">
            <description>Waits for a running task to complete and returns its final result.</description>
            <parameters>
                <parameter name="uuid" type="string" required="true">The UUID of the task to wait for.</parameter>
            </parameters>
            <usage_guidelines>You can only wait for tasks that were created with `dedicated_context: true`.</usage_guidelines>
        </tool>

    </tool_api_reference>

</scheduler_subsystem_documentation>