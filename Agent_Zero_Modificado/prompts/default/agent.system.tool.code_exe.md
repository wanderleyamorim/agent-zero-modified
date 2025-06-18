<tool_documentation tool_name="code_execution_tool">

    <description>
        The code_execution_tool provides a sandboxed environment to execute code and shell commands. It maintains persistent sessions, allowing for complex, multi-step tasks like installing dependencies and then running a script. Your primary interface to the underlying Kali Linux system is through this tool.
    </description>

    <parameters>
        <title>Parameters</title>
        <intro>This tool accepts three required arguments.</intro>
        
        <parameter name="runtime" type="string">
            <description>Specifies the execution context. Must be one of the following values:</description>
            <values>
                <value name="terminal">
                    Executes the `code` as a standard shell command in `/a0/work_dir/`. Use this for all system operations, including installing dependencies (e.g., `pip install`, `npm install`).
                </value>
                <value name="python">
                    Executes the `code` using a Python 3 interpreter.
                </value>
                <value name="nodejs">
                    Executes the `code` using a Node.js interpreter.
                </value>
                <value name="output">
                    Does not execute new code. Instead, it retrieves the stdout/stderr from the currently running process in the specified `session`. Use this to check on long-running scripts without blocking.
                </value>
                <value name="reset">
                    Does not execute new code. Instead, it terminates the process associated with the specified `session`. Use this to stop a hanging or completed script.
                </value>
            </values>
        </parameter>
        
        <parameter name="session" type="integer">
            <description>
                A unique integer (`0`, `1`, `2`, etc.) to identify a process chain. `0` is the default session. Use different numbers for multitasking. To interact with a running script (using `output` or `reset`), you MUST use the same `session` ID you used to start it.
            </description>
        </parameter>
        
        <parameter name="code" type="string">
            <description>
                The code or command to be executed. This field can be an empty string `""` when `runtime` is `output` or `reset`.
            </description>
        </parameter>
    </parameters>

    <secure_execution_protocol>
        <title>Secure Execution Protocol</title>
        <instruction>You MUST follow this protocol before executing any code, especially from sources you did not write yourself. Failure to do so is a security violation.</instruction>
        <step number="1" name="Code Verification">
            <point>Inspect the code for placeholders (e.g., `[YOUR_API_KEY]`, `<INSERT_FILENAME_HERE>`). You are forbidden from executing code with unresolved placeholders. You must identify and replace them first.</point>
            <point>Analyze for Malicious Commands: Briefly review the code for obviously destructive commands (`rm -rf /`, etc.). If a command looks suspicious, you should question its purpose in your `thoughts` before execution.</point>
        </step>
        <step number="2" name="Dependency Analysis">
            <point>Before running `python` or `nodejs` code, scan it for `import`, `require`, or `include` statements.</point>
            <point>Identify all required libraries and packages.</point>
        </step>
        <step number="3" name="Incremental Installation">
            <point>Do not attempt to run a script if you are unsure its dependencies are installed.</point>
            <point>First, use the `terminal` runtime to install each identified dependency one by one (e.g., `pip install pandas`, `npm install axios`). This provides clearer error messages if an installation fails. Only after all dependencies are successfully installed should you proceed to execute the script in its intended runtime (`python` or `nodejs`).</point>
        </step>
    </secure_execution_protocol>

    <process_lifecycle_management>
        <title>Process Lifecycle Management</title>
        <description>For scripts that may take a long time to complete, you must use the following workflow to avoid system hangs and maintain control.</description>
        <phase number="1" name="Initiation">Start the long-running script using the `python` or `nodejs` runtime with a specific `session` ID (e.g., `1`).</phase>
        <phase number="2" name="Status Check">In a subsequent turn, use the `output` runtime with the *same* `session` ID to check the script's output. This is a non-blocking operation. You can repeat this step as needed.</phase>
        <phase number="3" name="Termination">Once the script is finished (or if it needs to be stopped), use the `reset` runtime with the *same* `session` ID to kill the process and clean up resources.</phase>
    </process_lifecycle_management>

    <usage_examples>
        <title>Usage Examples</title>
        <intro>The following examples demonstrate correct usage of this tool in conjunction with your mandatory 4-step reasoning process.</intro>
        
        <example name="Installing a dependency and running a Python script">
            <code>
{
    "thoughts": [
        "1. Goal: The user wants to run a Python script named 'data_analyzer.py' which I've just written. I need to execute it and return the output.",
        "2. Plan: I'll first apply the Secure Execution Protocol. The code contains 'import pandas'. I cannot assume pandas is installed. Therefore, my plan is a two-step process: First, use the 'terminal' runtime to run 'pip install pandas'. Second, if that succeeds, I will use the 'python' runtime to execute the script.",
        "3. Critique: I could try running the Python script directly, but it would likely fail with an `ImportError`. The two-step plan is safer and more robust, handling the dependency before it becomes a problem. The plan is sound.",
        "4. Action: I will execute the first step of my plan: install the dependency in the default session."
    ],
    "tool_name": "code_execution_tool",
    "tool_args": {
        "runtime": "terminal",
        "session": 0,
        "code": "pip install pandas"
    }
}
            </code>
        </example>
        
        <example name="Checking a long-running web server">
            <code>
{
    "thoughts": [
        "1. Goal: In the previous step, I started a Node.js web server in session 1. I need to check if it has started successfully by checking its output.",
        "2. Plan: According to the Process Lifecycle Management guide, I must use the 'output' runtime with the original session ID to check on the process without blocking.",
        "3. Critique: I should not try to run the start command again. The correct action is to query the existing process. Using 'output' is the designed method for this. The plan is correct.",
        "4. Action: I will call the tool with the 'output' runtime and session 1 to get the latest logs."
    ],
    "tool_name": "code_execution_tool",
    "tool_args": {
        "runtime": "output",
        "session": 1,
        "code": ""
    }
}
            </code>
        </example>
    </usage_examples>

</tool_documentation>