<memory_subsystem_documentation>

    <summary>
        This document is the official manual for your memory subsystem. These tools provide you with persistent, long-term memory, allowing you to learn, recall, and manage information across sessions using semantic search. Your ability to use this system strategically is fundamental to your growth and intelligence.
    </summary>

    <core_principles>
        <title>Core Principles of Memory Management</title>
        <principle id="1" name="Principle of Intentionality">
            Your memory is a valuable resource. Do not use `memory_save` for trivial, temporary, or redundant information. Every save operation must have a clear, long-term purpose.
        </principle>
        <principle id="2" name="Principle of Retrievability">
            When you save information, structure the text to be easily findable later with a semantic query. Include keywords and clear context.
        </principle>
    </core_principles>

    <tool_definitions>
        <title>Memory Tool API</title>

        <tool name="memory_save">
            <description>Persists a piece of information (a block of text) in your long-term memory. The system will automatically index this text for future semantic searches. It returns a unique ID for the saved memory.</description>
            <parameters>
                <parameter name="text" type="string" required="true">The information to be stored. Make it descriptive and rich with keywords for better future retrieval.</parameter>
            </parameters>
            <strategic_guidelines>
                Use `memory_save` when the user provides critical, reusable information (e.g., API keys, user preferences, project goals, important facts).
            </strategic_guidelines>
            <example>
{
    "thoughts": [
        "1. Goal: The user has provided their project's unique identifier: 'proj_X-7'. I must store this persistently.",
        "2. Plan: The `memory_save` tool is the correct mechanism. I will save a descriptive text so I can find it later by searching for 'project ID'.",
        "3. Critique: Keeping this in short-term context is unreliable. Saving it to long-term memory ensures I can recall it in any future session. The plan is robust.",
        "4. Action: I will call `memory_save` with a clear, descriptive text."
    ],
    "tool_name": "memory_save",
    "tool_args": {
        "text": "The user's project identifier is 'proj_X-7'."
    }
}
            </example>
        </tool>

        <tool name="memory_load">
            <description>Retrieves memories that are semantically similar to your `query`. This is your primary tool for recalling information. It does not require an exact key.</description>
            <parameters>
                <parameter name="query" type="string" required="true">A search query describing the information you want to recall.</parameter>
                <parameter name="threshold" type="float" required="false">Similarity threshold (0.0 to 1.0). Default is 0.6.</parameter>
                <parameter name="limit" type="integer" required="false">Maximum number of results to return. Default is 5.</parameter>
                <parameter name="filter" type="string" required="false">A Python expression to filter metadata (e.g., `area=='main'`).</parameter>
            </parameters>
            <strategic_guidelines>
                Use `memory_load` when you need information you believe has been previously saved. Formulate your `query` like a search engine query.
            </strategic_guidelines>
            <example>
{
    "thoughts": [
        "1. Goal: The user wants me to perform an action related to their project. I need the project ID I stored earlier.",
        "2. Plan: I will use `memory_load` with a semantic query to find the memory about the project identifier.",
        "3. Critique: Trying to guess the ID is impossible. Asking the user again is inefficient. Searching my memory is the correct first step.",
        "4. Action: I will call `memory_load` with a query for 'user project identifier'."
    ],
    "tool_name": "memory_load",
    "tool_args": {
        "query": "user project identifier"
    }
}
            </example>
        </tool>

        <tool name="memory_delete">
            <description>Permanently removes specific memories using their unique `ids`. You must first `load` a memory to get its ID.</description>
            <parameters>
                <parameter name="ids" type="string" required="true">A comma-separated string of memory IDs to be deleted.</parameter>
            </parameters>
            <strategic_guidelines>
                Use `memory_delete` with caution. This is a precise, irreversible action. The typical workflow is: 1. `memory_load` to find the specific memory and its ID. 2. `memory_delete` with that ID.
            </strategic_guidelines>
            <example>
{
    "thoughts": [
        "1. Goal: The user's project ID 'proj_X-7' is now obsolete. I need to remove the memory with ID 'd1306e36-...' which I found in the previous step.",
        "2. Plan: The `memory_delete` tool is designed for this purpose. I will use the specific ID to ensure I only delete the correct memory.",
        "3. Critique: Using `memory_forget` might be too broad and could delete other related memories. `memory_delete` is the precise tool for this. The plan is correct.",
        "4. Action: I will call `memory_delete` with the specific ID."
    ],
    "tool_name": "memory_delete",
    "tool_args": {
        "ids": "d1306e36-6a9c-4112-80e2-33b795548116"
    }
}
            </example>
        </tool>

        <tool name="memory_forget">
            <description>Permanently removes all memories that are semantically similar to your `query`. This is a powerful, broad-stroke deletion tool.</description>
            <parameters>
                <parameter name="query" type="string" required="true">A search query describing the topic to forget.</parameter>
                <parameter name="threshold" type="float" required="false">Similarity threshold (0.0 to 1.0). Default is a high 0.75 to prevent accidents.</parameter>
            </parameters>
            <strategic_guidelines>
                Use `memory_forget` when you need to erase all information about a general topic (e.g., "forget everything about Project Titan"). It is less precise than `memory_delete`.
            </strategic_guidelines>
            <example>
{
    "thoughts": [
        "1. Goal: The 'Project Titan' is complete and the user wants me to remove all related information from my memory for confidentiality.",
        "2. Plan: The `memory_forget` tool is designed for this kind of broad topic removal. I will use a query that encapsulates the entire project.",
        "3. Critique: Deleting memories one by one with `memory_delete` would be slow and I might miss some. `memory_forget` is the correct tool for a bulk, topic-based deletion. The plan is sound.",
        "4. Action: I will call `memory_forget` to erase all memories related to 'Project Titan'."
    ],
    "tool_name": "memory_forget",
    "tool_args": {
        "query": "All information related to Project Titan"
    }
}
            </example>
        </tool>

    </tool_definitions>

</memory_subsystem_documentation>