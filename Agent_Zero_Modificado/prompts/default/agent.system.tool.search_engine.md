<tool_documentation tool_name="search_engine">

    <summary>
        This tool is your connection to the world's live information. It is not a simple question-answering machine; it is a precision instrument. Your ability to master advanced search techniques is critical to providing accurate, timely, and well-sourced answers. Generic queries yield generic results. Expert queries yield expert results.
    </summary>

    <parameters>
        <title>Parameters</title>
        <parameter name="query" type="string" required="true">
            <description>A meticulously crafted search string, potentially including keywords and advanced operators, designed to locate specific information on the web.</description>
        </parameter>
    </parameters>

    <core_search_philosophy>
        <title>Core Search Philosophy</title>
        <principle name="Keyword is King">
            Your queries must be built around precise keywords. Focus on nouns, technical terms, error codes, and unique identifiers. Avoid conversational filler, pronouns, and ambiguous questions.
        </principle>
        <principle name="Iterative Refinement">
            Searching is a cycle. Your first query is often not your last. If results are poor, you must analyze why and refine your query by adding keywords, excluding terms, or using operators. Never settle for the first page of results if it is not relevant.
        </principle>
    </core_search_philosophy>

    <power_user_techniques>
        <title>Power User Operator Manual</title>
        <intro>The following operators are your primary tools for refining searches. You are expected to use them whenever appropriate.</intro>
        
        <technique>
            <operator>"..." (Exact Phrase)</operator>
            <description>Searches for the exact sequence of words inside the quotes. This is the most important operator for reducing noise.</description>
            <use_case>Finding a specific error message, a quote, or an official name.</use_case>
        </technique>
        
        <technique>
            <operator>- (Exclude Term)</operator>
            <description>Excludes results that contain the term immediately following the hyphen.</description>
            <use_case>Removing a common but irrelevant context (e.g., `jaguar speed -car`).</use_case>
        </technique>

        <technique>
            <operator>site:</operator>
            <description>Restricts the search to a single website or domain.</description>
            <use_case>Finding information only within official documentation or a specific news source (e.g., `python file io site:python.org`).</use_case>
        </technique>

        <technique>
            <operator>filetype:</operator>
            <description>Restricts results to a specific file format.</description>
            <use_case>Finding official reports, academic papers, or presentations (e.g., `market analysis filetype:pdf`).</use_case>
        </technique>

        <technique>
            <operator>*</operator>
            <description>Acts as a wildcard to represent one or more unknown words.</description>
            <use_case>Completing a partial quote or finding variations of a phrase (e.g., `"to be or * to be"`).</use_case>
        </technique>
    </power_user_techniques>

    <query_progression_example>
        <title>Example: From Basic to Expert Query</title>
        <scenario>Find the original academic paper about the "Transformer" AI model.</scenario>
        <level_1 type="Basic (Poor)">`Transformer AI paper`</level_1>
        <analysis>Returns millions of blog posts, news articles, and explanations. Fails to find the primary source.</analysis>
        <level_2 type="Intermediate (Better)">`"Attention Is All You Need" paper`</level_2>
        <analysis>Much better. Uses the exact title. Still mixed with non-academic sources.</analysis>
        <level_3 type="Expert (Correct)">`"Attention Is All You Need" filetype:pdf site:arxiv.org`</level_3>
        <analysis>Excellent. This query targets the exact paper title, specifies the PDF file format common for academic papers, and restricts the search to the primary repository for scientific preprints (arxiv.org). This is the most direct path to the source.</analysis>
    </query_progression_example>

    <integration_with_reasoning_protocol>
        <title>Example of Full Reasoning</title>
        <code>
{
    "thoughts": [
        "1. Goal: The user wants to know the current Long-Term Support (LTS) version of Node.js. This information is specific, technical, and changes over time.",
        "2. Plan: This information is best found on the official Node.js website. A generic search could lead to outdated blog posts. I will use the `site:` operator to restrict my search to `nodejs.org` and use precise keywords.",
        "3. Critique: Simply searching 'nodejs lts version' is good, but searching only on the official site is safer and guarantees an authoritative answer. The plan is sound and minimizes the risk of citing incorrect information.",
        "4. Action: I will call the `search_engine` with a query that is targeted to the official Node.js website."
    ],
    "tool_name": "search_engine",
    "tool_args": {
        "query": "\"LTS version\" site:nodejs.org"
    }
}
        </code>
    </integration_with_reasoning_protocol>

</tool_documentation>