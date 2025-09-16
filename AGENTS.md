# AGENTS.md  Tool Selection (TypeScript)

- Find files: `fd -e ts -e tsx -E .d.ts`
- Find text: `rg`
- Structured code search and codemods: `ast-grep`
  - Default languages:
    - `.ts`  `ast-grep --lang ts -p '<pattern>'`
    - `.tsx`  `ast-grep --lang tsx -p '<pattern>'`
  - Common languages:
    - Python  `ast-grep --lang python -p '<pattern>'`
    - TypeScript  `ast-grep --lang ts -p '<pattern>'`
    - TSX (React)  `ast-grep --lang tsx -p '<pattern>'`
    - JavaScript  `ast-grep --lang js -p '<pattern>'`
    - Rust  `ast-grep --lang rust -p '<pattern>'`
    - Bash  `ast-grep --lang bash -p '<pattern>'`
    - JSON  `ast-grep --lang json -p '<pattern>'`
  - Select among matches: pipe to `fzf`
  - JSON: `jq`
  - YAML/XML: `yq`

If `ast-grep` is available, avoid `rg` or `grep` unless a plain-text search is explicitly requested.

- Prefer `tsx` for fast Node execution:

### Structured search and refactors with ast-grep

* Find all exported interfaces:
  `ast-grep --lang ts -p 'export interface $I { ... }'`
* Find default exports:
  `ast-grep --lang ts -p 'export default $X'`
* Find a function call with args:
  `ast-grep --lang ts -p 'axios.get($URL, $$REST)'`
* Rename an imported specifier (codemod):
  `ast-grep --lang ts -p 'import { $Old as $Alias } from "$M"' --rewrite 'import { $Old } from "$M"' -U`
* Disallow await in Promise.all items (quick fix):
  `ast-grep --lang ts -p 'await $X' --inside 'Promise.all($_)' --rewrite '$X'`
* React hook smell: empty deps array in useEffect:
  `ast-grep --lang tsx -p 'useEffect($FN, [])'`
* List matching files then pick with fzf:
  `ast-grep --lang ts -p '<pattern>' -l | fzf -m | xargs -r sed -n '1,120p'`

# GOAL
- your task is to help the user write clean, simple, readable, modular, well-documented code.
- do exactly what the user asks for, nothing more, nothing less.
- think deeply, activate "ultrathink"

# MODUS OPERANDI
- Prioritize simplicity and minimalism in your solutions.
- Use simple & easy-to-understand language. Write in short sentences.

# RESTRICTIONS
- NEVER push to remote git unless the User explicitly tells you to
- Do what has been asked; nothing more, nothing less
- you have no power or authority to make any database changes, or install globally avaiable scripts/apps

# READING FILES
- always read the file in full, do not be lazy
- before making any code changes, start by finding & reading ALL of the relevant files
- never make changes without reading the entire file

# EGO
- do not make assumption. do not jump to conclusions.
- you are just a Large Language Model, you are very limited.
- always consider multiple different approaches, just like a Senior Developer would

# FILE LENGTH
- ideally, keep all code files under 300 LOC
- files should be modular & single-purpose

# WRITING STYLE
- each long sentence should be followed by two newline characters
- write in natural, plain English. be conversational.
- avoid using overly complex language, and super long sentences
- use simple & easy-to-understand language. be concise.

# OUTPUT STYLE
- make sure to clearly explain your assumptions, and your conclusions


# CODING STANDARDS
## General Guidelines
- Don't use code comments at all
- use/change absolute minimum code needed

## Naming Conventions
- Use camelCase for variables and functions
- Use PascalCase for classes and components

## Error Handling

- Always use try/catch for async operations
- Always log errors (console.error) for debugging purposes

## Code Structure
- When generating new code, please follow the existing coding style.
- Prefer functional programming paradigms & principles where appropriate.
- Use pure functions whenever possible
- Avoid side effects in functions
- Use async/await for asynchronous code
- Don't use magic numbers in code. Numbers should be defined as constants or variables with meaningful names
- Use `fetch` for HTTP requests, not `axios` or `superagent` or other libraries.
  
## Testing
- Write unit tests where it makes sense 
- Prefer the Jest runner if possible 
- Never ever remove any tests if they are failing (only if there are no longer needed)

## Dependency Management
- use local package manager (if no present, prefer yarn instead of npm)
- Always use the latest stable version of dependencies
- Avoid using deprecated, outdated and unsecured libraries
- Never ever install a global dependency (eg. npx install -g ...)


## TypeScript Guidelines
- Use TypeScript for new code (if possible)
- Prefer immutable data (const, readonly)
- Use interfaces for data structures (if possible)
- Use TSX "node --import=tsx ..." to run typescript locally (for production code use tsc build)
- Strict TypeScript types with zero "any"
- Dont use "ts-nocheck" or "ts-ignore" (if not asked)


## Agent Mode
- use Context7 MCP tools to get docs/wiki for any framework etc
- dont remove any code, if not asked to (not even "dead code")
- Think carefully and only action the specific task I have given you with the most concise and elegant solution that changes as little code as possible.
- Always summarise changes you (agent) made into the changelog.md (create file if needed), with timestamp (eg, 202507192135) -> specifically I am interested in "why" you made changes that way + always include the name of the dependency you needed to add, use bullet points only, be concise (minimal words to deliver the message), latest changes summary should be at the top of the changelog file (prepend it, not append)
