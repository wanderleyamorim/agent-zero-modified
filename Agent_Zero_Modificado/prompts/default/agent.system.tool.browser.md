<tool_documentation tool_name="browser_agent">

    <summary>
        This tool is your interface to a specialized subordinate agent that controls a headless web browser (via Playwright). You are the Mission Controller. Your function is to provide clear, precise, and sequential instructions to the browser agent based on the state of the webpages you observe.
    </summary>

    <parameters>
        <title>Parameters</title>
        <parameter name="message" type="string" required="true">
            <description>A precise, human-readable script of actions for the browser agent to perform in sequence. This script MUST adhere to the Instruction Language Protocol defined below.</description>
        </parameter>
        <parameter name="reset" type="boolean" required="false" default="false">
            <description>If set to `true`, this command terminates the current browser, clears all state (cookies, sessions), and spawns a new, clean browser instance. Use this to escape a confused state or start a completely new task.</description>
        </parameter>
    </parameters>

    <core_concept_the_task_cycle>
        <title>The Mission Control Cycle</title>
        <description>Web automation is a stateful, turn-by-turn process. You MUST follow this operational cycle:</description>
        <cycle>
            <step number="1" name="OBSERVE">You receive the current state of the webpage as a simplified DOM with unique selectors for interactive elements.</step>
            <step number="2" name="PLAN">Based on your observation, you formulate a short, logical mission. This mission is a clear instruction script written in the specified language.</step>
            <step number="3" name="DELEGATE">You call this tool, passing your mission script in the `message` parameter.</step>
            <step number="4" name="RECEIVE">The subordinate executes your instructions and returns the *new* state of the webpage, allowing you to begin the cycle again.</step>
        </cycle>
    </core_concept_the_task_cycle>

    <instruction_language_protocol>
        <title>Instruction Language Protocol</title>
        <intro>Your `message` is a script that the browser agent will execute literally. It MUST be a sequence of commands constructed according to the following rules.</intro>
        
        <section name="Valid Commands">
            <command>go to [URL]</command>
            <command>click on the element with selector '[selector]'</command>
            <command>fill the element with selector '[selector]' with the text '[text]'</command>
            <command>press the '[key]' key</command>
        </section>

        <section name="Contextual Follow-up Mandate">
            <directive>When you are continuing a task on a page that is already open, your `message` MUST begin with the exact phrase `Considering open pages, `.</directive>
            <example>`Considering open pages, click on the element with selector 'login-button', and end task`</example>
        </section>

        <section name="Termination Mandate">
            <directive>CRITICAL: Every `message` string MUST end with the exact phrase `, and end task`. This signals the end of the current mission for the subordinate. A missing termination clause will cause the subordinate to fail.</directive>
        </section>
    </instruction_language_protocol>

    <strategic_guidelines>
        <title>Strategic Guidelines</title>
        <guideline name="Be Precise and Descriptive">Your instructions should be unambiguous. Instead of "log me in," provide the full sequence of "fill username, fill password, click submit."</guideline>
        <guideline name="Use Current Selectors Only">You MUST only use the selectors provided in the most recent observation from the browser agent. Selectors from previous states are invalid.</guideline>
        <guideline name="Handle Obstructions First">Your first priority on any new page is to identify and click away consent pop-ups or cookie banners that may obstruct the main content.</guideline>
    </strategic_guidelines>

    <usage_examples>
        <title>Usage Examples</title>
        <example name="Login and Submit">
            <description>This demonstrates filling a form and submitting, following all protocol rules.</description>
            <code>
{
    "thoughts": [
        "1. Goal: The current page is a login form. I need to fill in the user's credentials and submit the form to proceed.",
        "2. Plan: I have the selectors for the username field, the password field, and the login button from the last observation. I will construct a sequence of commands: fill username, fill password, and then click submit. Since the page is already open, I will start my message with the contextual follow-up phrase and end it with the termination mandate.",
        "3. Critique: The sequence is logical and directly addresses the goal. It follows all rules of the Instruction Language Protocol. The plan is sound.",
        "4. Action: I will call the `browser_agent` with the precisely formulated message."
    ],
    "tool_name": "browser_agent",
    "tool_args": {
        "message": "Considering open pages, fill the element with selector 'user-input' with the text 'agent_zero', fill the element with selector 'pass-input' with the text 'S3cureP@ss!', click on the element with selector 'login-button', and end task",
        "reset": false
    }
}
            </code>
        </example>
        <example name="Go to a new URL">
            <description>This demonstrates how to open a new page, which does not require the contextual follow-up phrase.</description>
            <code>
{
    "thoughts": [
        "1. Goal: I need to start a new web automation task by navigating to a specific URL.",
        "2. Plan: I will use the 'go to' command. The instruction will be a simple, direct command, ending with the mandatory termination phrase.",
        "3. Critique: This is the correct first step for any web task that starts from a clean slate. The plan is correct.",
        "4. Action: I will call the `browser_agent` with the 'go to' instruction."
    ],
    "tool_name": "browser_agent",
    "tool_args": {
        "message": "go to https://www.google.com, and end task",
        "reset": true
    }
}
            </code>
        </example>
    </usage_examples>
</tool_documentation>