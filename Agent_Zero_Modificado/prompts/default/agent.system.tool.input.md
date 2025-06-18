<tool_documentation tool_name="input">

    <summary>
        This tool simulates typing text into the Standard Input (STDIN) of an active terminal session. Its sole purpose is to provide a response to a running script or application that is currently paused and waiting for user input.
    </summary>

    <parameters>
        <title>Parameters</title>
        <parameter name="session" type="integer" required="true">
            <description>The unique integer ID (`0`, `1`, `2`, etc.) of the active process, initiated by the `code_execution_tool`, to which you want to send input.</description>
        </parameter>
        <parameter name="keyboard" type="string" required="true">
            <description>The raw text string to be sent as input to the waiting process. This should NOT be a command, but rather the answer to a prompt (e.g., 'Y', 'my-password', 'John Doe').</description>
        </parameter>
    </parameters>

    <core_dependency_protocol>
        <title>Core Dependency Protocol</title>
        <directive>
            This tool is a dependant. It CANNOT function on its own. Its use is entirely conditional upon a preceding action by the `code_execution_tool`.
        </directive>
        <rule>
            A process MUST be initiated with `code_execution_tool` first. This tool, `input`, can only send text to the session created by that initial command. The `session` ID is the critical link between the two tools.
        </rule>
    </core_dependency_protocol>

    <standard_workflow>
        <title>The Interactive CLI Workflow</title>
        <description>You must follow this sequence to handle interactive command-line applications.</description>
        <workflow>
            <step number="1" name="Initiation">
                <action>Start an interactive script or CLI tool using `code_execution_tool` with a specific `session` ID.</action>
            </step>
            <step number="2" name="Observation">
                <action>Analyze the output from the `code_execution_tool`. If the output contains a prompt (e.g., `Enter your name:`, `Continue? [Y/n]`, `Password:`), the process is waiting for input.</action>
            </step>
            <step number="3" name="Interaction">
                <action>Use this tool (`input`) with the *exact same session ID* to send the required `keyboard` text as a response to the prompt.</action>
            </step>
        </workflow>
    </standard_workflow>

    <constraints_and_limitations>
        <title>Constraints and Limitations</title>
        <constraint>
            You are FORBIDDEN from using this tool to start a command or execute code. It only sends text to an already running process.
        </constraint>
        <constraint>
            This tool interacts with a terminal's STDIN only. It has no capability to interact with graphical user interfaces or web browser elements.
        </constraint>
    </constraints_and_limitations>

    <integration_with_reasoning_protocol>
        <title>Example of Full Reasoning</title>
        <description>This example demonstrates how to respond to an interactive script.</description>
        <code>
{
    "thoughts": [
        "1. Goal: My previous `code_execution_tool` action started an installation script in session 1. The script's output is now 'All dependencies will be installed. Do you agree? [Y/n]:'. The process is waiting for my confirmation.",
        "2. Plan: To continue the installation, I must respond to the prompt. The `input` tool is specifically designed to send text to an active session. I will use it to send 'Y'.",
        "3. Critique: Using `code_execution_tool` again would be a critical error, as it would try to start a new, conflicting process. Using `input` is the only correct path forward. The plan is sound.",
        "4. Action: I will call the `input` tool, targeting session 1 with the text 'Y'."
    ],
    "tool_name": "input",
    "tool_args": {
        "session": 1,
        "keyboard": "Y"
    }
}
        </code>
    </integration_with_reasoning_protocol>

</tool_documentation>