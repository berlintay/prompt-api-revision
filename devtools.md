I've been playing around with it.

very helpful for when trying to read through the console logs.


it can generate code snippets in console, and is very fast.

I was kind of able to get the logic for the amount of tokens per call but theres alot id want to implement and if dont include anything now I will totally forget lol

```js
// Function to display directories in a tree-like format with proper names
function displayDirectoriesAsTree() {
    // Extract the pathname from each URL found on the page, with error handling
    const links = [...document.querySelectorAll('a')]
        .map(link => {
            try {
                return new URL(link.href).pathname;
            } catch (e) {
                console.warn(`Invalid URL skipped: ${link.href}`);
                return null; // Skip invalid URLs
            }
        })
        .filter(path => path); // Remove null values

    // Function to build a nested directory tree from the paths
    function buildTree(paths) {
        const tree = {};

        paths.forEach(path => {
            const parts = path.split('/').filter(part => part); // Split and remove empty parts
            let current = tree;

            parts.forEach((part, index) => {
                if (!current[part]) {
                    current[part] = {};
                }
                current = current[part];
            });
        });

        return tree;
    }
    
    
    // Function to display the tree structure in a fancy format
    function displayTree(tree, indent = '') {
        Object.keys(tree).forEach((key, index, array) => {
            const isLast = index === array.length - 1;
            console.log(`${indent}${isLast ? '└──' : '├──'} ${key}`);
            displayTree(tree[key], `${indent}${isLast ? '    ' : '│   '}`);
        });
    }

    // Build and display the tree
    const directoryTree = buildTree(links);
    console.log("Directories (URLs) found on the page:");
    displayTree(directoryTree);
    return `Total directories: ${Object.keys(directoryTree).length}`;
}
function findAllLinks() {
    const links = [...document.querySelectorAll('a')].map(link => link.href);
    return links.length ? `Links found on the page:\n${links.join('\n')}` : 'No links found on the page.';
}


async function createSession() {
    let active = true; // State to track if the session is active

    const session = {
        async prompt(input) {
            if (!active) {
                return "Session is not active.";
            }

            showLoadingSpinner();

            console.log(`AI is processing your request: ${input}`);

            await new Promise(resolve => setTimeout(resolve, 1000)); // Simulating delay

            hideLoadingSpinner();

            if (input.toLowerCase().includes('find all links')) {
                return findAllLinks();
            } else if (input.toLowerCase().includes('find div by id')) {
                const id = input.split(' ').pop(); // Extracting the ID from the input
                return findDivById(id);
            } else if (input.toLowerCase().includes('find all xpath')) {
                const xpath = input.match(/"([^"]+)"/)[1]; // Extract the XPath expression from the input
                return findAllXPath(xpath);
            } else if (input.toLowerCase().includes('find hidden elements')) {
                return findHiddenElements();
            } else if (input.toLowerCase().includes('iterate directories')) {
                return displayDirectoriesAsTree();
            } else if (input.toLowerCase().includes('find manifest')) {
                return findManifest();
            } else if (input.toLowerCase().includes('display errors')) {
                return displayErrors();
            } else if (input.toLowerCase().includes('fix error')) {
                return fixError();
            } else if (input.toLowerCase().includes('show tokens')) {
                return showTokenCount();
            } else if (input.toLowerCase() === 'end session') {
                active = false;
                return "Session has ended.";
            } else {
                return `I don't understand the command: "${input}". Try asking to "find all links", "find div by id [your_id]", "find all xpath [your_xpath]", "find hidden elements", "iterate directories", "find manifest", "display errors", or "fix error".`;
            }
        },

        async promptStreaming(input) {
            console.log(`Streaming is not supported for this command: ${input}`);
        },

        isActive() {
            return active;
        }
    };

    return session;
}

// Function to interact with the AI assistant
async function aiAssistant(action, input) {
    const session = await createSession();

    if (session.isActive()) {
        console.log('Session is active.');
    } else {
        console.log('Session is not active.');
    }

    if (action === 'prompt') {
        const result = await session.prompt(input);
        console.log(result);
    } else if (action === 'stream') {
        await session.promptStreaming(input);
    } else {
        console.log('Unknown action. Use "prompt" for processing commands.');
    }
}

// Other functions like showLoadingSpinner, hideLoadingSpinner, etc., remain the same...

// Example usage in the console:
// aiAssistant('prompt', 'iterate directories');
```

![Screenshot 2024-08-14 074849](https://github.com/user-attachments/assets/f3d03f27-4d44-4e20-86cf-9b34dd553a99)
![Screenshot 2024-08-14 075001](https://github.com/user-attachments/assets/de246075-6368-4c6c-b997-a1cfcf0c8b63)
![Screenshot 2024-08-14 075152](https://github.com/user-attachments/assets/55dc6d3c-b98c-4235-b7fa-05a11729c030)
![Screenshot 2024-08-14 075651](https://github.com/user-attachments/assets/fb5dc8fc-672f-478c-913e-1de3d151c9ba)
