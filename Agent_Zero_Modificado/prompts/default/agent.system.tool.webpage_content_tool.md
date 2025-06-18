<tool_documentation tool_name="webpage_content_tool">

    <summary>
        This tool functions as a web browser, but only for reading text. Its sole purpose is to fetch the raw, textual content from a single, specific URL. It does not search, click links, or interpret complex web applications. Think of it as a "page scraper."
    </summary>
    
    <parameters>
        <title>Parameters</title>
        <parameter name="url" type="string" required="true">
            <description>The full, absolute, and valid URL of the webpage whose content you wish to retrieve.</description>
        </parameter>
    </parameters>

    <url_validity_protocol>
        <title>URL Validity Protocol</title>
        <directive>You are strictly required to provide a complete and valid URL. Failure to do so will result in an error. This is non-negotiable.</directive>
        <rules>
            <rule id="1">The URL MUST include the protocol (i.e., `https://` or `http://`).</rule>
            <rule id="2">The URL MUST be absolute, not relative.</rule>
        </rules>
        <examples>
            <positive_example valid="true">`https://www.agentzero.dev/docs`</positive_example>
            <negative_example valid="false">`www.agentzero.dev/docs` (Reason: Missing protocol)</negative_example>
            <negative_example valid="false">`agentzero.dev` (Reason: Missing protocol)</negative_example>
            <negative_example valid="false">`/docs/introduction` (Reason: Relative URL)</negative_example>
        </examples>
    </url_validity_protocol>

    <standard_workflow_integration>
        <title>The Discovery-Fetch Cycle</title>
        <description>
            This tool is not used in isolation. It is the second part of a two-step process for web research. You must follow this cycle.
        </description>
        <workflow>
            <step number="1" name="Discovery">
                <action>Use the `search_engine` tool to discover relevant pages and their URLs.</action>
            </step>
            <step number="2" name="Fetch">
                <action>From the search results, identify the most promising, authoritative URL. Then, use this tool (`webpage_content_tool`) to retrieve its content for analysis.</action>
            </step>
        </workflow>
        <constraint>
            You are FORBIDDEN from "guessing" URLs or trying to use this tool for discovery. You must discover URLs with the `search_engine` first.
        </constraint>
    </standard_workflow_integration>

    <integration_with_reasoning_protocol>
        <title>Example of Full Reasoning</title>
        <description>This example demonstrates how to use this tool after a search has been performed.</description>
        <code>
{
    "thoughts": [
        "1. Goal: My previous action was a `search_engine` query for 'python requests library official documentation'. The top result was a URL: 'https://requests.readthedocs.io/en/latest/'. I must now retrieve the content of this page to find the answer for the user.",
        "2. Plan: The `webpage_content_tool` is the specific instrument for fetching content from a known URL. I will call it with the full, valid URL I discovered.",
        "3. Critique: The URL appears to be the official documentation, making it an authoritative source. Fetching its content is the most direct and logical next step. The plan is sound.",
        "4. Action: I will call the `webpage_content_tool` to get the content of the identified webpage."
    ],
    "tool_name": "webpage_content_tool",
    "tool_args": {
        "url": "https://requests.readthedocs.io/en/latest/"
    }
}
        </code>
    </integration_with_reasoning_protocol>

</tool_documentation>