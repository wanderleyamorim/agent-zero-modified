<tool_documentation tool_name="knowledge_tool">

    <summary>
        This tool is your primary gateway to information. It is a hybrid system that provides access to both your internal memory (conversation history and learned knowledge) and a real-time web search engine. Your effectiveness as an agent is directly tied to your strategic use of this tool.
    </summary>

    <capabilities>
        <title>Core Capabilities</title>
        <capability name="Internal_Memory_Access">
            <description>The tool first searches your internal context, including the current conversation history and long-term memory. This is the fastest way to retrieve information that has already been discussed or established.</description>
        </capability>
        <capability name="Web_Search_Access">
            <description>If information is not available internally, or if the query demands up-to-the-minute data, the tool seamlessly executes a web search. This is essential for topics related to current events, specific public data (like software versions), or anything outside your pre-trained knowledge.</description>
        </capability>
    </capabilities>

    <parameters>
        <title>Parameters</title>
        <parameter name="question" type="string" required="true">
            <description>A precise, keyword-driven question or search term designed to retrieve specific information. The quality of your query directly determines the quality of the result.</description>
        </parameter>
    </parameters>

    <strategic_guidelines>
        <title>Strategic Usage Protocol</title>
        <guideline id="1" name="The Memory-First Principle">
            <directive>Before calling this tool, ask yourself: "Has this information already been provided in our conversation or stored in memory?". Use `memory_load` first if you suspect the information is stored long-term. Redundant searches are inefficient.</directive>
        </guideline>
        <guideline id="2" name="Query Formulation Protocol">
            <directive>You WILL formulate queries that are specific, targeted, and use precise keywords. Avoid conversational or ambiguous questions.</directive>
            <rules>
                <rule>BE SPECIFIC: Instead of "AI news," ask "latest developments in large language models June 2024".</rule>
                <rule>USE KEYWORDS: Focus on nouns, entities, and technical terms. "What is the most current version of the Python requests library?" is better phrased as "Python requests library latest version PyPI".</rule>
                <rule>DECOMPOSE: For broad topics, break your query into smaller, sequential questions.</rule>
            </rules>
        </guideline>
        <guideline id="3" name="Web Search Triggers">
            <directive>A web search is MANDATORY under the following conditions:</directive>
            <triggers>
                <trigger>The query is about current events, news, or any time-sensitive information (e.g., "today's weather in Curitiba").</trigger>
                <trigger>The query involves specific, publicly verifiable facts that can change (e.g., software version numbers, stock prices, URLs for official websites).</trigger>
                <trigger>The information is highly specialized or niche and falls outside standard encyclopedic knowledge.</trigger>
            </triggers>
        </guideline>
    </strategic_guidelines>

    <query_formulation_examples>
        <title>Query Formulation Examples</title>
        <negative_example>
            <description>A POOR, ineffective query:</description>
            <code>"Tell me about Agent Zero."</code>
            <reason>This query is too vague and self-referential. It lacks specific keywords and will likely yield generic or unhelpful results.</reason>
        </negative_example>
        <positive_example>
            <description>A GOOD, effective query:</description>
            <code>"Agent Zero framework open-source license details"</code>
            <reason>This query is specific, uses clear keywords ("Agent Zero framework", "open-source license"), and has a clear, factual goal. It is designed to find a specific piece of data.</reason>
        </positive_example>
    </query_formulation_examples>

    <integration_with_reasoning_protocol>
        <title>Example of Full Reasoning</title>
        <description>The following demonstrates how to use this tool within your mandatory 4-step thinking process.</description>
        <code>
{
    "thoughts": [
        "1. Goal: The user is asking for the official documentation URL for the 'pandas' Python library. This is a specific, public fact.",
        "2. Plan: My internal knowledge might contain a URL, but it could be outdated. The most reliable way to get the current, official URL is a web search. I will use the `knowledge_tool`. I will formulate a precise, keyword-based question to find the official site.",
        "3. Critique: Relying on memory is risky for URLs. A direct search is faster and more accurate than guessing or asking the user to verify. The plan is sound.",
        "4. Action: I will call the `knowledge_tool` with a question designed to find the official documentation."
    ],
    "tool_name": "knowledge_tool",
    "tool_args": {
        "question": "pandas python library official documentation URL"
    }
}
        </code>
    </integration_with_reasoning_protocol>

</tool_documentation>