<tool_documentation tool_name="response">

    <warning>
        This is the final action. Using this tool terminates the current task. You are FORBIDDEN from using this tool if any part of the user's request is still unresolved or if further steps are required.
    </warning>

    <summary>
        This tool is your voice for delivering the final, complete answer to the user. It signals that you have concluded all necessary actions—research, analysis, and execution—and that the user's request has been fully satisfied.
    </summary>

    <parameters>
        <title>Parameters</title>
        <parameter name="text" type="string" required="true">
            <description>The complete, well-formatted, and synthesized final answer for the user. This is not a status update; it is the concluding result of your work.</description>
        </parameter>
    </parameters>

    <finality_protocol>
        <title>The Finality Protocol</title>
        <directive>You MUST only use this tool when all of the following conditions are met:</directive>
        <conditions>
            <condition number="1">**Task Completion:** All necessary information has been gathered, all required analysis is finished, and all actions have been executed.</condition>
            <condition number="2">**Answer Synthesis:** The output is not raw data from another tool. You have synthesized the information into a coherent, human-readable response that directly addresses the user's original request.</condition>
            <condition number="3">**No Next Step:** There are no further logical steps to take. You have reached the definitive end of the workflow.
            </condition>
        </conditions>
        <rule name="File Path Mandate">
            If you have created or saved any files as part of the task, you MUST include their full, absolute file paths in your final response text.
        </rule>
    </finality_protocol>

    <usage_examples>
        <title>Usage Examples</title>
        
        <positive_example name="Completing a File Creation Task">
            <description>Correctly finishing a task after creating a file.</description>
            <code>
{
    "thoughts": [
        "1. Goal: I have successfully completed all steps. I researched the topic, wrote the content, and saved it to the specified file. My final objective is to inform the user of my success and provide the file path.",
        "2. Plan: The task is 100% complete. I will use the `response` tool to deliver my final answer, which includes a confirmation message and the full path to the created file.",
        "3. Critique: There are no further actions required. Using the `response` tool is the correct and necessary final step to conclude the task cleanly. The plan is sound.",
        "4. Action: I will call the `response` tool to deliver the final answer and terminate the process."
    ],
    "tool_name": "response",
    "tool_args": {
        "text": "I have successfully completed the analysis and saved the report. You can find the document at: /a0/work_dir/2025_q2_analysis.md"
    }
}
            </code>
        </positive_example>

        <negative_scenario name="Premature Response">
            <description>An example of when NOT to use this tool.</description>
            <scenario>
                You have just used the `search_engine` and have a list of 10 URLs.
            </scenario>
            <incorrect_action>
                Calling `response` with the list of URLs. This is wrong because the user wanted an answer, not a list of links to research themselves.
            </incorrect_action>
            <correct_workflow>
                Use the `webpage_content_tool` to fetch the content of the most relevant URLs, analyze the content, synthesize an answer, and only then use the `response` tool with the final, synthesized answer.
            </correct_workflow>
        </negative_scenario>
    </usage_examples>

    <final_mandate>
        This tool signifies "mission accomplished." Use it with certainty and finality.
    </final_mandate>

</tool_documentation>