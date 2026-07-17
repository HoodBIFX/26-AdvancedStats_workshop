# Advanced Statistics Workshop
Dr Randy Johnson
2026-07-18

  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/monaco-editor@0.46.0/min/vs/editor/editor.main.css" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css" integrity="sha512-DTOQO9RWCH3ppGqcWaEA1BIZOC6xxalwEsw9c2QQeAIftl+Vegovlnee1c9QX4TctnWMn13TZye+giMm8e2LwA==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  

<style type="text/css">
.monaco-editor pre {
  background-color: unset !important;
}

.qpyodide-editor-toolbar {
  width: 100%;
  display: flex;
  justify-content: space-between;
  box-sizing: border-box;
}

.qpyodide-editor-toolbar-left-buttons, .qpyodide-editor-toolbar-right-buttons {
  display: flex;
}

.qpyodide-non-interactive-loading-container.qpyodide-cell-needs-evaluation, .qpyodide-non-interactive-loading-container.qpyodide-cell-evaluated {
  justify-content: center;
  display: flex;
  background-color: rgba(250, 250, 250, 0.65);
  border: 1px solid rgba(233, 236, 239, 0.65);
  border-radius: 0.5rem;
  margin-top: 15px;
  margin-bottom: 15px;
}

.qpyodide-r-project-logo {
  color: #2767B0; /* R Project's blue color */
}

.qpyodide-icon-status-spinner {
  color: #7894c4;
}

.qpyodide-icon-run-code {
  color: #0d9c29
}

.qpyodide-output-code-stdout {
  color: #111;
}

.qpyodide-output-code-stderr {
  color: #db4133;
}

.qpyodide-editor {
  border: 1px solid #EEEEEE;
}

.qpyodide-editor-toolbar {
  background-color: #EEEEEE;
  padding: 0.2rem 0.5rem;
}

.qpyodide-button {
  background-color: #EEEEEE;
  display: inline-block;
  font-weight: 400;
  line-height: 1;
  text-decoration: none;
  text-align: center;
  color: #000;
  border-color: #dee2e6;
  border: 1px solid rgba(0,0,0,0);
  padding: 0.375rem 0.75rem;
  font-size: .9rem;
  border-radius: 0.25rem;
  transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
}

.qpyodide-button:hover {
  color: #000;
  background-color: #d9dce0;
  border-color: #c8ccd0;
}

.qpyodide-button:disabled,.qpyodide-button.disabled,fieldset:disabled .qpyodide-button {
  pointer-events: none;
  opacity: .65
}

.qpyodide-button-reset {
  color: #696969; /*#4682b4;*/
}

.qpyodide-button-copy {
  color: #696969;
}


/* Custom styling for RevealJS Presentations*/

/* Reset the style of the interactive area */
.reveal div.qpyodide-interactive-area {
  display: block;
  box-shadow: none;
  max-width: 100%;
  max-height: 100%;
  margin: 0;
  padding: 0;
} 

/* Provide space to entries */
.reveal div.qpyodide-output-code-area pre div {
  margin: 1px 2px 1px 10px;
}

/* Collapse the inside code tags to avoid extra space between line outputs */
.reveal pre div code.qpyodide-output-code-stdout, .reveal pre div code.qpyodide-output-code-stderr {
  padding: 0;
  display: contents;
}

.reveal pre div code.qpyodide-output-code-stdout {
  color: #111;
}

.reveal pre div code.qpyodide-output-code-stderr {
  color: #db4133;
}


/* Create a border around console and output (does not effect graphs) */
.reveal div.qpyodide-console-area {
  border: 1px solid #EEEEEE;
  box-shadow: 2px 2px 10px #EEEEEE;
}

/* Cap output height and allow text to scroll */
/* TODO: Is there a better way to fit contents/max it parallel to the monaco editor size? */
.reveal div.qpyodide-output-code-area pre {
  max-height: 400px;
  overflow: scroll;
}
</style>

<script type="module">
// Document level settings ----

// Determine if we need to install python packages
globalThis.qpyodideInstallPythonPackagesList = [''];

// Check to see if we have an empty array, if we do set to skip the installation.
globalThis.qpyodideSetupPythonPackages = !(qpyodideInstallPythonPackagesList.indexOf("") !== -1);

// Display a startup message?
globalThis.qpyodideShowStartupMessage = true;

// Describe the webR settings that should be used
globalThis.qpyodideCustomizedPyodideOptions = {
  "indexURL": "https://cdn.jsdelivr.net/pyodide/v0.27.2/full/",
  "env": {
    "HOME": "/home/pyodide",
  }, 
  stdout: (text) => {qpyodideAddToOutputArray(text, "out");},
  stderr: (text) => {qpyodideAddToOutputArray(text, "error");}
}

// Store cell data
globalThis.qpyodideCellDetails = [{"code":"import numpy as np\nimport matplotlib.pyplot as plt","id":1,"options":{"autorun":"","classes":"","comment":"","context":"interactive","dpi":72,"fig-cap":"","fig-height":5,"fig-width":7,"label":"","message":"true","out-height":"","out-width":"700px","output":"true","read-only":"false","results":"markup","warning":"true"}},{"code":"#' Probability distribution we want to integrate\n#' @param x Numeric defining a point on the x-axis\n#' @return The corresponding value on the y-axis\n\ndef target_pdf(x):\n    return np.exp(-0.5 * (x - 2)**2) + 0.5 * np.exp(-0.5 * (x + 3)**2)","id":2,"options":{"autorun":"","classes":"","comment":"","context":"interactive","dpi":72,"fig-cap":"","fig-height":5,"fig-width":7,"label":"","message":"true","out-height":"","out-width":"700px","output":"true","read-only":"false","results":"markup","warning":"true"}},{"code":"#' Metropolis Hastings function\n#' @param iterations Integer value specifying the number of times to run the algorithm\n#' @return A vector of locations where the MH algorithm spent time\n\ndef metropolis_hastings(iterations=10000):\n    samples = np.zeros(iterations)\n    current_x = 0.0  # Start somewhere arbitrary\n    \n    for i in range(iterations):\n        # Step 1: Propose a nearby random state (the \"Markov Chain\" step)\n        proposed_x = np.random.normal(loc=current_x, scale=1.0)\n        \n        # Step 2: Compare the new state to the current state\n        # The denominator cancels out here!\n        ratio = target_pdf(proposed_x) / target_pdf(current_x)\n        acceptance_prob = min(1.0, ratio)\n        \n        # Step 3: Accept or reject the move (the \"Monte Carlo\" step)\n        if np.random.rand() < acceptance_prob:\n            current_x = proposed_x\n            \n        # Log our position\n        samples[i] = current_x\n        \n    return samples","id":3,"options":{"autorun":"","classes":"","comment":"","context":"interactive","dpi":72,"fig-cap":"","fig-height":5,"fig-width":7,"label":"","message":"true","out-height":"","out-width":"700px","output":"true","read-only":"false","results":"markup","warning":"true"}},{"code":"samples = metropolis_hastings()","id":4,"options":{"autorun":"","classes":"","comment":"","context":"interactive","dpi":72,"fig-cap":"","fig-height":5,"fig-width":7,"label":"","message":"true","out-height":"","out-width":"700px","output":"true","read-only":"false","results":"markup","warning":"true"}},{"code":"plt.hist(samples, bins=50, density=True, alpha=0.6, color='b')\nplt.title(\"MCMC Approximation of the Target Distribution\")\nplt.show()","id":5,"options":{"autorun":"","classes":"","comment":"","context":"interactive","dpi":72,"fig-cap":"","fig-height":5,"fig-width":7,"label":"","message":"true","out-height":"","out-width":"700px","output":"true","read-only":"false","results":"markup","warning":"true"}}];


</script>

<script type="module">
// Declare startupMessageqpyodide globally
globalThis.qpyodideStartupMessage = document.createElement("p");

// Function to set the button text
globalThis.qpyodideSetInteractiveButtonState = function(buttonText, enableCodeButton = true) {
  document.querySelectorAll(".qpyodide-button-run").forEach((btn) => {
    btn.innerHTML = buttonText;
    btn.disabled = !enableCodeButton;
  });
}

// Function to update the status message in non-interactive cells
globalThis.qpyodideUpdateStatusMessage = function(message) {
  document.querySelectorAll(".qpyodide-status-text.qpyodide-cell-needs-evaluation").forEach((elem) => {
    elem.innerText = message;
  });
}

// Function to update the status message
globalThis.qpyodideUpdateStatusHeader = function(message) {

  if (!qpyodideShowStartupMessage) return;

  qpyodideStartupMessage.innerHTML = message;
}

// Status header update with customized spinner message
globalThis.qpyodideUpdateStatusHeaderSpinner = function(message) {

  qpyodideUpdateStatusHeader(`
    <i class="fa-solid fa-spinner fa-spin qpyodide-icon-status-spinner"></i>
    <span>${message}</span>
  `);
}


// Function that attaches the document status message
function qpyodideDisplayStartupMessage(showStartupMessage) {
  if (!showStartupMessage) {
    return;
  }

  // Get references to header elements
  const headerHTML = document.getElementById("title-block-header");
  const headerRevealJS = document.getElementById("title-slide");

  // Create the outermost div element for metadata
  const quartoTitleMeta = document.createElement("div");
  quartoTitleMeta.classList.add("quarto-title-meta");

  // Create the first inner div element
  const firstInnerDiv = document.createElement("div");
  firstInnerDiv.setAttribute("id", "qpyodide-status-message-area");

  // Create the second inner div element for "Pyodide Status" heading and contents
  const secondInnerDiv = document.createElement("div");
  secondInnerDiv.setAttribute("id", "qpyodide-status-message-title");
  secondInnerDiv.classList.add("quarto-title-meta-heading");
  secondInnerDiv.innerText = "Pyodide Status";

  // Create another inner div for contents
  const secondInnerDivContents = document.createElement("div");
  secondInnerDivContents.setAttribute("id", "qpyodide-status-message-body");
  secondInnerDivContents.classList.add("quarto-title-meta-contents");

  // Describe the Pyodide state
  qpyodideStartupMessage.innerText = "🟡 Loading...";
  qpyodideStartupMessage.setAttribute("id", "qpyodide-status-message-text");
  // Add `aria-live` to auto-announce the startup status to screen readers
  qpyodideStartupMessage.setAttribute("aria-live", "assertive");

  // Append the startup message to the contents
  secondInnerDivContents.appendChild(qpyodideStartupMessage);

  // Combine the inner divs and contents
  firstInnerDiv.appendChild(secondInnerDiv);
  firstInnerDiv.appendChild(secondInnerDivContents);
  quartoTitleMeta.appendChild(firstInnerDiv);

  // Determine where to insert the quartoTitleMeta element
  if (headerHTML || headerRevealJS) {
    // Append to the existing "title-block-header" element or "title-slide" div
    (headerHTML || headerRevealJS).appendChild(quartoTitleMeta);
  } else {
    // If neither headerHTML nor headerRevealJS is found, insert after "Pyodide-monaco-editor-init" script
    const monacoScript = document.getElementById("qpyodide-monaco-editor-init");
    const header = document.createElement("header");
    header.setAttribute("id", "title-block-header");
    header.appendChild(quartoTitleMeta);
    monacoScript.after(header);
  }
}

qpyodideDisplayStartupMessage(qpyodideShowStartupMessage);
</script>

<script type="module">
// Create a logging setup
globalThis.qpyodideMessageArray = []

// Add messages to array
globalThis.qpyodideAddToOutputArray = function(message, type) {
  qpyodideMessageArray.push({ message, type });
}

// Function to reset the output array
globalThis.qpyodideResetOutputArray = function() {
  qpyodideMessageArray = [];
}

globalThis.qpyodideRetrieveOutput = function() {
  return qpyodideMessageArray.map(entry => entry.message).join('\n');
}

// Start a timer
const initializePyodideTimerStart = performance.now();

// Encase with a dynamic import statement
globalThis.qpyodideInstance = await import(
  qpyodideCustomizedPyodideOptions.indexURL + "pyodide.mjs").then(
   async({ loadPyodide }) => {

    console.log("Start loading Pyodide");
    
    // Populate Pyodide options with defaults or new values based on `pyodide`` meta
    let mainPyodide = await loadPyodide(
      qpyodideCustomizedPyodideOptions
    );
    
    // Setup a namespace for global scoping
    // await loadedPyodide.runPythonAsync("globalScope = {}"); 
    
    // Update status to reflect the next stage of the procedure
    qpyodideUpdateStatusHeaderSpinner("Initializing Python Packages");

    // Load the `micropip` package to allow installation of packages.
    await mainPyodide.loadPackage("micropip");
    await mainPyodide.runPythonAsync(`import micropip`);

    // Load the `pyodide_http` package to shim uses of `requests` and `urllib3`.
    // This allows for `pd.read_csv(url)` to work flawlessly.
    // Details: https://github.com/coatless-quarto/pyodide/issues/9
    await mainPyodide.loadPackage("pyodide_http");
    await mainPyodide.runPythonAsync(`
    import pyodide_http
    pyodide_http.patch_all()  # Patch all libraries
    `);

    // Load the `matplotlib` package with necessary environment hook
    await mainPyodide.loadPackage("matplotlib");

    // Set the backend for matplotlib to be interactive.
    await mainPyodide.runPythonAsync(`
    import matplotlib
    matplotlib.use("module://matplotlib_pyodide.html5_canvas_backend")
    from matplotlib import pyplot as plt
    `);

    // Unlock interactive buttons
    qpyodideSetInteractiveButtonState(
      `<i class="fa-solid fa-play qpyodide-icon-run-code"></i> <span>Run Code</span>`, 
      true
    );

    // Set document status to viable
    qpyodideUpdateStatusHeader(
      "🟢 Ready!"
    );

    // Assign Pyodide into the global environment
    globalThis.mainPyodide = mainPyodide;

    console.log("Completed loading Pyodide");
    return mainPyodide;
  }
);

// Stop timer
const initializePyodideTimerEnd = performance.now();

// Create a function to retrieve the promise object.
globalThis._qpyodideGetInstance = function() {
    return qpyodideInstance;
}

</script>

<script src="https://cdn.jsdelivr.net/npm/monaco-editor@0.46.0/min/vs/loader.js"></script>
<script type="module" id="qpyodide-monaco-editor-init">

  // Configure the Monaco Editor's loader
  require.config({
    paths: {
      'vs': 'https://cdn.jsdelivr.net/npm/monaco-editor@0.46.0/min/vs'
    }
  });
</script>

## Workshop Overview

<!--
This This document relies on quarto-pyodide extension for quarto. Install with:
&#10;`quarto add coatless-quarto/pyodide`
-->

- Bayes’ Rule & Monty Hall: Updating beliefs based on new evidence.
  <!-- 15 minutes -->
- The Intractability Problem: *Why* do we need computational stats?
  <!-- 10 minutes -->
- MCMC & Python Code: Metropolis-Hastings for numerical integration.
  <!-- 15 minutes -->
- Q&A / Wrap-up <!--  5 minutes -->

## Probability Review

A Venn diagram can be a useful tool when thinking about probabilities.
Let’s say that we have two events, `A` and `B`, with a circle
representing each event.

![](img/Slide1.png)

We can visualize the probabilitis of `A` and `B` as the area of each
circle in this Venn diagram, so the probability of observing event, `A`
is

![](img/Slide2.png)

and the probability of observing event `B` is

![](img/Slide3.png)

The probability of observing `A` or `B` is called the union

![](img/Slide4.png)

and the probability of observing both `A` and `B` is called the
intersection.

![](img/Slide5.png)

We can also visualize conditional probability. The probability of
observing event `A` given the fact that we know event `B` has already
happened is

![](img/Slide6.png)

### Bayes Rule

Rev. Thomas Bayes observed that

and thus

$$
P(A|B) = \frac{P(A) * P(B|A)}{P(B)} 
$$

### The Monty Hall Problem

Monty Hall ran a gameshow in the 60’s and 70’s called *Let’s Make a
Deal*. At the end of the show, the final contestant was given an
opportunity to win a car.

- The contestant first picks between three doors. Behind one is the car,
  the other two are goats.
- Monty Hall reveals a goat behind one of the doors that wasn’t picked.
- The contestant the has to decide to stick with their original choice
  or switch their chioce to the third door.

![](img/Slide7.png)

What is the “right” choice?

![](img/Slide8.png)

Let’s change the sequence of events. Which is the right choice now?

![](img/Slide9.png)

There is nothing special about Monty Hall’s reveal that changes the
probabilities.

![](img/Slide10.png)

### Back to Bayes

We can reframe this in terms of an initial hypothesis with evidence. The
probability of getting a goat is the same regardless of the door we
pick, so let’s go with door 1.

<!--
$$
P(\mbox{Car}) = \frac{1}{3} \forall \mbox(Doors)
$$
-->

![](img/Slide11.png)

Then Monty Hall reveals that there is a goat behind door 3.

![](img/Slide12.png)

#### Posterior Probability

The posterior probability that the car is behind door 2, given that we
originally picked door 1 and Monty revealed a goat behind door 3 is:

$$
P(D_2 | E) = \frac{P(E | D_2) * P(D_2)}{P(E)}
$$

If $D_2$ hides the car and we picked $D_1$, then Monty has to open
$D_3$:

$$
P(E|D_2) = 1
$$

The unconditional probability that the car is behind door 2:

$$
P(D_2) = \frac{1}{3}
$$

The unconditional probability that Monty will open door 3:

$$
P(E) = \frac{1}{2}
$$

Thus, our posterior probability that the car is behind door 2 is:

## Application: Integration

- In real-world machine learning (like training neural networks or
  modeling health data), hypotheses aren’t always discrete outcomes;
  they are often continuous parameters.

- The evidence denominator, $P(E)$, isn’t a simple sum anymore. It
  becomes an integral over all possible parameters:

$$
P(E) = \int P(E | H)P(H) dH
$$

- In high-dimensional spaces (e.g. hundreds of variables), this integral
  is analytically impossible to compute. We know the *shape* of our
  posterior, but we can’t calculate its exact scale (the denominator).

### Markov Chain Monte Carlo (MCMC)

- MCMC is like trying to map the topography of a mountain range while
  blindfolded in the fog.
  - You can’t observe the terain (compute the integral), but you can
    walk around and feel if you are going uphill or downhill.

<!-- -->

- Markov Chain Monte Carlo (MCMC) is an algorithm that takes a “random
  walk” through the parameter space.
  - It prefers to spend time in areas of high probability (the peaks)
    and rarely visits low probability areas (the valleys).
  - By *counting* where the algorithm spends its time, we can
    approximate the integral without ever having to solve it
    mathematically.

### Metropolis-Hastings

We’ll be using the Metropolis-Hastings algorithm to perform this random
walk and estimate the integral of a bimodal probability distribution:

$$
\int e^{\frac{-(x-2)^2}{2}} + e^{\frac{-(x+3)^2}{2}} dx
$$

- We know the shape of the equation
- We need the integral clalculated from 0 to $\infty$ to be able to
  normalize the distribution (i.e. so all probabilities add to 1)
- Integration is annoying, and becomes impossible when equations become
  complex

#### Set up python environment

<div id="qpyodide-insertion-location-1"></div>
<noscript>Please enable JavaScript to experience the dynamic code cell content on this page.</noscript>

#### Define our function

<div id="qpyodide-insertion-location-2"></div>
<noscript>Please enable JavaScript to experience the dynamic code cell content on this page.</noscript>

#### Define the MH function

<div id="qpyodide-insertion-location-3"></div>
<noscript>Please enable JavaScript to experience the dynamic code cell content on this page.</noscript>

#### Run the algorithm

<div id="qpyodide-insertion-location-4"></div>
<noscript>Please enable JavaScript to experience the dynamic code cell content on this page.</noscript>

#### Plot the results

<div id="qpyodide-insertion-location-5"></div>
<noscript>Please enable JavaScript to experience the dynamic code cell content on this page.</noscript>

<script type="module">
/**
 * Factory function to create different types of cells based on options.
 * @param {Object} cellData - JSON object containing code, id, and options.
 * @returns {BaseCell} Instance of the appropriate cell class.
 */
globalThis.qpyodideCreateCell = function(cellData) {
    switch (cellData.options.context) {
        case 'interactive':
            return new InteractiveCell(cellData);
        case 'output':
            return new OutputCell(cellData);
        case 'setup':
            return new SetupCell(cellData);
        default:
            return new InteractiveCell(cellData);
            // throw new Error('Invalid cell type specified in options.');
    }
}  

/**
 * CellContainer class for managing a collection of cells.
 * @class
 */
class CellContainer {
    /**
     * Constructor for CellContainer.
     * Initializes an empty array to store cells.
     * @constructor
     */
    constructor() {
        this.cells = [];
    }

    /**
     * Add a cell to the container.
     * @param {BaseCell} cell - Instance of a cell (BaseCell or its subclasses).
     */
    addCell(cell) {
        this.cells.push(cell);
    }

    /**
     * Execute all cells in the container.
     */
    async executeAllCells() {
        for (const cell of this.cells) {
            await cell.executeCode();
        }
    }

    /**
     * Execute all cells in the container.
     */
    async autoRunExecuteAllCells() {
        for (const cell of this.cells) {
            await cell.autoRunExecuteCode();
        }
    }
}
  

/**
 * BaseCell class for handling code execution using Pyodide.
 * @class
 */
class BaseCell {
    /**
     * Constructor for BaseCell.
     * @constructor
     * @param {Object} cellData - JSON object containing code, id, and options.
     */
    constructor(cellData) {
        this.code = cellData.code;
        this.id = cellData.id;
        this.options = cellData.options;
        this.insertionLocation = document.getElementById(`qpyodide-insertion-location-${this.id}`);
        this.executionLock = false;
    }

    cellOptions() {
        // Subclass this? 
        console.log(this.options);
        return this.options;
    }

    /**
     * Execute the Python code using Pyodide.
     * @returns {*} Result of the code execution.
     */
    async executeCode() {
        // Execute code using Pyodide
        const result = getPyodide().runPython(this.code);
        return result;
    }
};

/**
 * InteractiveCell class for creating editable code editor with Monaco Editor.
 * @class
 * @extends BaseCell
 */
class InteractiveCell extends BaseCell {

    /**
     * Constructor for InteractiveCell.
     * @constructor
     * @param {Object} cellData - JSON object containing code, id, and options.
     */
    constructor(cellData) {
        super(cellData);
        this.editor = null;
        this.setupElement();
        this.setupMonacoEditor();
    }

    /**
     * Set up the interactive cell elements
     */
    setupElement() {

        // Create main div element
        var mainDiv = document.createElement('div');
        mainDiv.id = `qpyodide-interactive-area-${this.id}`;
        mainDiv.className = `qpyodide-interactive-area`;
        if (this.options.classes) {
            mainDiv.className += " " + this.options.classes
        }

        // Add a unique cell identifier that users can customize
        if (this.options.label) {
            mainDiv.setAttribute('data-id', this.options.label);
        }

        // Create toolbar div
        var toolbarDiv = document.createElement('div');
        toolbarDiv.className = 'qpyodide-editor-toolbar';
        toolbarDiv.id = `qpyodide-editor-toolbar-${this.id}`;

        // Create a div to hold the left buttons
        var leftButtonsDiv = document.createElement('div');
        leftButtonsDiv.className = 'qpyodide-editor-toolbar-left-buttons';

        // Create a div to hold the right buttons
        var rightButtonsDiv = document.createElement('div');
        rightButtonsDiv.className = 'qpyodide-editor-toolbar-right-buttons';

        // Create Run Code button
        var runCodeButton = document.createElement('button');
        runCodeButton.className = 'btn btn-default qpyodide-button qpyodide-button-run';
        runCodeButton.disabled = true;
        runCodeButton.type = 'button';
        runCodeButton.id = `qpyodide-button-run-${this.id}`;
        runCodeButton.textContent = '🟡 Loading Pyodide...';
        runCodeButton.title = `Run code (Shift + Enter)`;

        // Append buttons to the leftButtonsDiv
        leftButtonsDiv.appendChild(runCodeButton);

        // Create Reset button
        var resetButton = document.createElement('button');
        resetButton.className = 'btn btn-light btn-xs qpyodide-button qpyodide-button-reset';
        resetButton.type = 'button';
        resetButton.id = `qpyodide-button-reset-${this.id}`;
        resetButton.title = 'Start over';
        resetButton.innerHTML = '<i class="fa-solid fa-arrows-rotate"></i>';

        // Create Copy button
        var copyButton = document.createElement('button');
        copyButton.className = 'btn btn-light btn-xs qpyodide-button qpyodide-button-copy';
        copyButton.type = 'button';
        copyButton.id = `qpyodide-button-copy-${this.id}`;
        copyButton.title = 'Copy code';
        copyButton.innerHTML = '<i class="fa-regular fa-copy"></i>';

        // Append buttons to the rightButtonsDiv
        rightButtonsDiv.appendChild(resetButton);
        rightButtonsDiv.appendChild(copyButton);

        // Create console area div
        var consoleAreaDiv = document.createElement('div');
        consoleAreaDiv.id = `qpyodide-console-area-${this.id}`;
        consoleAreaDiv.className = 'qpyodide-console-area';

        // Create editor div
        var editorDiv = document.createElement('div');
        editorDiv.id = `qpyodide-editor-${this.id}`;
        editorDiv.className = 'qpyodide-editor';

        // Create output code area div
        var outputCodeAreaDiv = document.createElement('div');
        outputCodeAreaDiv.id = `qpyodide-output-code-area-${this.id}`;
        outputCodeAreaDiv.className = 'qpyodide-output-code-area';
        outputCodeAreaDiv.setAttribute('aria-live', 'assertive');

        // Create pre element inside output code area
        var preElement = document.createElement('pre');
        preElement.style.visibility = 'hidden';
        outputCodeAreaDiv.appendChild(preElement);

        // Create output graph area div
        var outputGraphAreaDiv = document.createElement('div');
        outputGraphAreaDiv.id = `qpyodide-output-graph-area-${this.id}`;
        outputGraphAreaDiv.className = 'qpyodide-output-graph-area';

        // Append buttons to the toolbar
        toolbarDiv.appendChild(leftButtonsDiv);
        toolbarDiv.appendChild(rightButtonsDiv);

        // Append all elements to the main div
        mainDiv.appendChild(toolbarDiv);
        consoleAreaDiv.appendChild(editorDiv);
        consoleAreaDiv.appendChild(outputCodeAreaDiv);
        mainDiv.appendChild(consoleAreaDiv);
        mainDiv.appendChild(outputGraphAreaDiv);

        // Insert the dynamically generated object at the document location.
        this.insertionLocation.appendChild(mainDiv);
    }

    /**
     * Set up Monaco Editor for code editing.
     */
    setupMonacoEditor() {

        // Retrieve the previously created document elements
        this.runButton = document.getElementById(`qpyodide-button-run-${this.id}`);
        this.resetButton = document.getElementById(`qpyodide-button-reset-${this.id}`);
        this.copyButton = document.getElementById(`qpyodide-button-copy-${this.id}`);
        this.editorDiv = document.getElementById(`qpyodide-editor-${this.id}`);
        this.outputCodeDiv = document.getElementById(`qpyodide-output-code-area-${this.id}`);
        this.outputGraphDiv = document.getElementById(`qpyodide-output-graph-area-${this.id}`);
        
        // Store reference to the object
        var thiz = this;

        // Load the Monaco Editor and create an instance
        require(['vs/editor/editor.main'], function () {
            thiz.editor = monaco.editor.create(
                thiz.editorDiv, {
                    value: thiz.code,
                    language: 'python',
                    theme: 'vs-light',
                    automaticLayout: true,           // Works wonderfully with RevealJS
                    scrollBeyondLastLine: false,
                    minimap: {
                        enabled: false
                    },
                    fontSize: '17.5pt',              // Bootstrap is 1 rem
                    renderLineHighlight: "none",     // Disable current line highlighting
                    hideCursorInOverviewRuler: true,  // Remove cursor indictor in right hand side scroll bar
                    readOnly: thiz.options['read-only'] ?? false
                }
            );
        
            // Store the official counter ID to be used in keyboard shortcuts
            thiz.editor.__qpyodideCounter = thiz.id;
        
            // Store the official div container ID
            thiz.editor.__qpyodideEditorId = `qpyodide-editor-${thiz.id}`;
        
            // Store the initial code value and options
            thiz.editor.__qpyodideinitialCode = thiz.code;
            thiz.editor.__qpyodideOptions = thiz.options;
        
            // Set at the model level the preferred end of line (EOL) character to LF.
            // This prevent `\r\n` from being given to the Pyodide engine if the user is on Windows.
            // See details in: https://github.com/coatless/quarto-Pyodide/issues/94
            // Associated error text: 
            // Error: <text>:1:7 unexpected input
        
            // Retrieve the underlying model
            const model = thiz.editor.getModel();
            // Set EOL for the model
            model.setEOL(monaco.editor.EndOfLineSequence.LF);
        
            // Dynamically modify the height of the editor window if new lines are added.
            let ignoreEvent = false;
            const updateHeight = () => {
            const contentHeight = thiz.editor.getContentHeight();
            // We're avoiding a width change
            //editorDiv.style.width = `${width}px`;
            thiz.editorDiv.style.height = `${contentHeight}px`;
                try {
                    ignoreEvent = true;
            
                    // The key to resizing is this call
                    thiz.editor.layout();
                } finally {
                    ignoreEvent = false;
                }
            };
        
            // Helper function to check if selected text is empty
            function isEmptyCodeText(selectedCodeText) {
                return (selectedCodeText === null || selectedCodeText === undefined || selectedCodeText === "");
            }
        
            // Registry of keyboard shortcuts that should be re-added to each editor window
            // when focus changes.
            const addPyodideKeyboardShortCutCommands = () => {
            // Add a keydown event listener for Shift+Enter to run all code in cell
            thiz.editor.addCommand(monaco.KeyMod.Shift | monaco.KeyCode.Enter, () => {
                // Retrieve all text inside the editor
                thiz.runCode(thiz.editor.getValue());
            });
        
            // Add a keydown event listener for CMD/Ctrl+Enter to run selected code
            thiz.editor.addCommand(monaco.KeyMod.CtrlCmd | monaco.KeyCode.Enter, () => {
                    // Get the selected text from the editor
                    const selectedText = thiz.editor.getModel().getValueInRange(thiz.editor.getSelection());
                    // Check if no code is selected
                    if (isEmptyCodeText(selectedText)) {
                        // Obtain the current cursor position
                        let currentPosition = thiz.editor.getPosition();
                        // Retrieve the current line content
                        let currentLine = thiz.editor.getModel().getLineContent(currentPosition.lineNumber);
                
                        // Propose a new position to move the cursor to
                        let newPosition = new monaco.Position(currentPosition.lineNumber + 1, 1);
                
                        // Check if the new position is beyond the last line of the editor
                        if (newPosition.lineNumber > thiz.editor.getModel().getLineCount()) {
                            // Add a new line at the end of the editor
                            thiz.editor.executeEdits("addNewLine", [{
                            range: new monaco.Range(newPosition.lineNumber, 1, newPosition.lineNumber, 1),
                            text: "\n", 
                            forceMoveMarkers: true,
                            }]);
                        }
                        
                        // Run the entire line of code.
                        thiz.runCode(currentLine);
                
                        // Move cursor to new position
                        thiz.editor.setPosition(newPosition);
                    } else {
                        // Code to run when Ctrl+Enter is pressed with selected code
                        thiz.runCode(selectedText);
                    }
                });
            }
        
            // Register an on focus event handler for when a code cell is selected to update
            // what keyboard shortcut commands should work.
            // This is a workaround to fix a regression that happened with multiple
            // editor windows since Monaco 0.32.0 
            // https://github.com/microsoft/monaco-editor/issues/2947
            thiz.editor.onDidFocusEditorText(addPyodideKeyboardShortCutCommands);
        
            // Register an on change event for when new code is added to the editor window
            thiz.editor.onDidContentSizeChange(updateHeight);
        
            // Manually re-update height to account for the content we inserted into the call
            updateHeight();
                
        });

        
        // Add a click event listener to the run button
        thiz.runButton.onclick = function () {
            thiz.runCode(
                thiz.editor.getValue()
            );
        };
        
        // Add a click event listener to the reset button
        thiz.copyButton.onclick = function () {
            // Retrieve current code data
            const data = thiz.editor.getValue();
            
            // Write code data onto the clipboard.
            navigator.clipboard.writeText(data || "");
        };
        
        // Add a click event listener to the copy button
        thiz.resetButton.onclick = function () {
            thiz.editor.setValue(thiz.editor.__qpyodideinitialCode);
        };
    }

    disableInteractiveCells() {
        // Enable locking of execution for the cell
        this.executionLock = true;

        // Disallowing execution of other code cells
        document.querySelectorAll(".qpyodide-button-run").forEach((btn) => {
            btn.disabled = true;
        });
    }

    enableInteractiveCells() {
        // Remove locking of execution for the cell
        this.executionLock = false;

        // All execution of other code cells
        document.querySelectorAll(".qpyodide-button-run").forEach((btn) => {
            btn.disabled = false;
        });
    }

    /**
     * Execute the Python code inside the editor.
     */
    async runCode(code) {
        
        // Check if we have an execution lock
        if (this.executeLock) return; 
        
        this.disableInteractiveCells();

        // Force wait procedure
        await mainPyodide;

        // Clear the output stock
        qpyodideResetOutputArray();

        // Generate a new canvas element, avoid attaching until the end
        let graphFigure = document.createElement("figure");
        document.pyodideMplTarget = graphFigure;

        console.log("Running code!");
        // Obtain results from the base class
        try {
            // Always check to see if the user adds new packages
            await mainPyodide.loadPackagesFromImports(code);

            // Process result
            const output = await mainPyodide.runPythonAsync(code);

            // Add output
            qpyodideAddToOutputArray(output, "stdout");
        } catch (err) {
            // Add error message
            qpyodideAddToOutputArray(err, "stderr");
            // TODO: There has to be a way to remove the Pyodide portion of the errors... 
        }

        const result = qpyodideRetrieveOutput();

        // Nullify the output area of content
        this.outputCodeDiv.innerHTML = "";
        this.outputGraphDiv.innerHTML = "";        

        // Design an output object for messages
        const pre = document.createElement("pre");
        if (/\S/.test(result)) {
            // Display results as HTML elements to retain output styling
            const div = document.createElement("div");
            div.innerHTML = result;
            pre.appendChild(div);
        } else {
            // If nothing is present, hide the element.
            pre.style.visibility = "hidden";
        }

        // Add output under interactive div
        this.outputCodeDiv.appendChild(pre);

        // Place the graphics onto the page
        if (graphFigure) {

            if (this.options['fig-cap']) {
                // Create figcaption element
                const figcaptionElement = document.createElement('figcaption');
                figcaptionElement.innerText = this.options['fig-cap'];
                // Append figcaption to figure
                graphFigure.appendChild(figcaptionElement);    
            }

            this.outputGraphDiv.appendChild(graphFigure);
        }

        // Re-enable execution
        this.enableInteractiveCells();
    }
};

/**
 * OutputCell class for customizing and displaying output.
 * @class
 * @extends BaseCell
 */
class OutputCell extends BaseCell {
    /**
     * Constructor for OutputCell.
     * @constructor
     * @param {Object} cellData - JSON object containing code, id, and options.
     */
    constructor(cellData) {
      super(cellData);
    }
  
    /**
     * Display customized output on the page.
     * @param {*} output - Result to be displayed.
     */
    displayOutput(output) {
        const results = this.executeCode();
        return results;
    }
  }

/**
 * SetupCell class for suppressed output.
 * @class
 * @extends BaseCell
 */
class SetupCell extends BaseCell {
    /**
     * Constructor for SetupCell.
     * @constructor
     * @param {Object} cellData - JSON object containing code, id, and options.
     */
    constructor(cellData) {
        super(cellData);
    }

    /**
     * Execute the Python code without displaying the results.
     */
    runSetupCode() {
        // Execute code without displaying output
        this.executeCode();
    }
};
</script>

<script type="module">
// Handle cell initialization initialization
qpyodideCellDetails.map(
    (entry) => {
      // Handle the creation of the element
      qpyodideCreateCell(entry);
    }
  );
</script>
