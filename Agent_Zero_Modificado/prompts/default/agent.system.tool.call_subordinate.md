<tool_documentation tool_name="call_subordinate">

    <summary>
        This tool is your primary mechanism for creating and managing temporary, specialized subordinates to handle specific sub-tasks. You use it to spawn a new "expert" AI to solve a problem you cannot handle with your basic tools, and then to communicate with it.
    </summary>

    <parameters>
        <title>Parameters</title>
        <parameter name="message" type="string" required="true">
            <description>The instruction sent to the subordinate. This is the most critical parameter. When spawning a new subordinate, this `message` MUST define its role and task. See the protocol below.</description>
        </parameter>
        <parameter name="reset" type="boolean" required="false" default="false">
            <description>Controls the subordinate's session state. This is a critical strategic parameter.
                - `reset: "true"`: Spawns a **new, fresh subordinate instance**. The `message` must define its role. Use this to start new, independent tasks.
                - `reset: "false"`: Continues a conversation with the **existing subordinate**. Use this for follow-up instructions or providing more data.
            </description>
        </parameter>
    </parameters>

    <strategic_delegation_protocol>
        <title>Strategic Delegation Protocol</title>
        
        <section name="When to Spawn a Subordinate">
            <directive>You spawn a subordinate when a task requires a focused, expert skill set that goes beyond simple tool use. You MUST spawn a new subordinate when:</directive>
            <triggers>
                <trigger>The task requires writing a non-trivial block of code (e.g., Python, SQL, Javascript).</trigger>
                <trigger>The task requires creative writing or re-formatting of text into a complex structure (e.g., "summarize this as a poem", "write a professional email").</trigger>
                <trigger>The task involves multi-step logical reasoning or analysis that would be difficult to manage with single tool calls (e.g., "analyze this data and find anomalies").</trigger>
            </triggers>
            <constraint>
                You are FORBIDDEN from delegating simple, self-contained tasks that you can perform with a basic tool (e.g., do not spawn a subordinate just to list files).
            </constraint>
        </section>

        <section name="How to Write an Effective Briefing (the 'message' parameter)">
            <directive>When `reset` is `true`, your `message` is a contract that creates a new expert. It must be clear, concise, and complete. Follow these rules:</directive>
            <rules>
                <rule id="1" name="Define the Role Explicitly">Start by assigning a role. "You are an expert Python developer." "You are a data analyst." "You are a professional copywriter."</rule>
                <rule id="2" name="State the Task Clearly">Describe the specific task the subordinate needs to perform.</rule>
                <rule id="3" name="Provide All Necessary Context">Include any relevant data, previous findings, or user constraints. The subordinate only knows what you tell it.</rule>
                <rule id="4" name="Specify the Output Format">Define the desired format of the response. "The output must be only the Python code block." "Provide the answer as a JSON object."</rule>
            </rules>
        </section>
    </strategic_delegation_protocol>

    <briefing_examples>
        <title>Briefing Formulation Examples</title>
        <negative_example>
            <description>A POOR, ineffective briefing (`message`):</description>
            <code>"Write some code."</code>
            <reason>This is useless. It fails to define a role, a task, the language, or the desired output.</reason>
        </negative_example>
        <positive_example>
            <description>A GOOD, effective briefing (`message`) for a new 'Coder' subordinate:</description>
            <code>"You are an expert Python developer. Your task is to write a function that takes a list of integers and returns a new list containing only the even numbers. The output must be only the Python code block, with no explanation."</code>
            <reason>This is a model briefing: It specifies the role, the goal, the context, and the success criteria (output format).</reason>
        </positive_example>
    </briefing_examples>

    <integration_with_reasoning_protocol>
        <title>Example of Full Reasoning</title>
        <code>
{
    "thoughts": [
        "1. Goal: The user wants to convert a block of text into a structured JSON object based on a set of rules. This is a complex parsing task.",
        "2. Plan: This task is too complex for a simple tool. It requires understanding context and structure. I will spawn a new subordinate with the role of 'Data Extraction Expert' to handle this.",
        "3. Critique: I could try to write a Python script myself to do this, but it would be slow and prone to error. Spawning a temporary specialist is faster and more reliable for this kind of language-based task. The plan is sound.",
        "4. Action: I will call `call_subordinate` with `reset: \"true\"` to spawn the expert and give it a clear set of instructions."
    ],
    "tool_name": "call_subordinate",
    "tool_args": {
        "message": "You are a Data Extraction Expert. Your task is to read the following text and convert it into a JSON object. The JSON should have two keys: 'name' and 'age'. Text: 'The user, John Doe, is 34 years old.'",
        "reset": "true"
    }
}
        </code>
    </integration_with_reasoning_protocol>

</tool_documentation>