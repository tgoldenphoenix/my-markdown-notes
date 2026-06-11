# Jackson

`@JsonProperty` (The JSON Translator)

This annotation comes from the Jackson library

Standard Java uses camelCase naming conventions (e.g., `landingPageUrl`), but APIs usually return JSON using snake_case (e.g., `landing_page_url`).  
`@JsonProperty` acts as the mapping bridge between the two. It tells the JSON parser: "When you read the API text and find a key named exactly `landing_page_url`, extract its value and inject it into the Java variable named `landingPageUrl`