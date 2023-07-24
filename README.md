# Getting Started

This is a demonstration of how a documentation would look and function like.

**Look on the top left** for the menu button and click on that to open up the side menu for table of contents for quick navigation and search feature.

## Requirements

The only thing that's needed to run this documentation application is these three files you see on the lefthand side (README.md - Where the content lies, index.html - the basic structure that houses the README.md file, and the package.json - the package instructions.)

Extremely lightweight. We would host these on the on-premise server that we choose to use)

The other thing that we would need is just the command-line-interface tool by docsify (in addition to the node.js package that I assume would already be on there since it is the basis of all the utilities I have built so far.)

On the server, we would install by typing

```bash
npm i docsify-cli -g
```

## Benefits

**Why I think this is a great alternative, or to use in parallel with current methods)**

- **Modern** In-line with all modern technical documentations
- **Safe and auditable** Documentation would be version-controlled via github, meaning all participants of documentation can make changes, but there is an audit trail of every change and merged.

Plus, changes need to flow through a pipeline for selected approvers and mergers, further controlling the content's maintenance, as well as being able to revert back to previous states in case of a mistakes, etc.. This will be very useful for meeting software documentation audits and compliances.

_(Currently, if something is changed in the Application Support folder, i'm not fully aware of any auditable and returning back to previous versions of the folder's structure and/or its contents.)_

- **Searchable** Contents of documentation is searchable using a live-search that returns in less than a second of all possible matches.

# Projects

## Multipurpose Charge-Off Form

Some example of one wiki entry with another code entry and instructions

```bash
Some code instructions
```

### Overview

To build a tool outside of webfocus (being sunsetted) that allows charge-off submissions and includes logic for routing to appropriate department to handle and validate, then to further relay the request to proper management individual(s) for review and final approval of charge-offs.

### Technical Details

Add some technical Details here

## Financial Transaction Management Templates

### Overview

Creating new Financial Transaction Management templates to be utilized by Controllers department to help eliminate interoffice settlement tickets

### Technical Details

### Things to Know

### Getting Started

#### Resources

## Optimizing Asset Verification Process

### Overview

To help optimize the process of asset verification, We've devised this method, which are split into three parts, to help with batch processing of requests:

**Capturing Data from SSA/Medicaid**

**Batch Processing of Data**

- A script ran locally on your computer. It only needs the file generated from the previous step to be placed in the folder. Double click the script to execute it

- Generates two lists for accounts not found and for accounts found

- Use list of "Accounts not Found" to quickly process (checkmarking 'accounts not found' on SSA site

**Generating Detailed Report for Accounts Found**

- Using the accounts found list generated from the previous step, place in Web Intelligence

- Use "Accounts Found" list as input for Web Intelligence Report

- Generates report of details/balances/etc for accounts

_The final step would require users to just manually paste the report's results for each individual's inquiry, and for 'accounts not found', users can go down the list and check off all the requests that appear on the 'accounts-not-found' list._

### Getting Started

**Capturing the Data**

1. Visit the AVS / SSI website and head over to your request inbox.
2. Change the view to 100 per page.

**Installing and Using the Extension**

1. Unzip the extension package somewhere on your computer.

2. Open up Google Chrome, go to Extensions, and on the top-right, there should be a "Developer Mode" toggler. Toggle it on.

3. Now on the top-left, there is a button that says 'load unpacked'. Click on that, it will ask you to pick a folder.

4. Navigate to the folder where you saved the package. Remember to select the folder and not the individual file while you're selecting. Confirm selection.

5. Now you should see the name of the package on your extension list. Make sure it is turned on.

6. Now close out of extensions page and click on the extension button on the top-right so that a list of your extensions appear.

7. Look for the Extension we just installed and PIN it.

_BE SURE TO BE ON THE AVS OR SSI PAGE's TAB, ON PAGE 1_

8. Click the extension to open the options for it. Pick either AVS or SSI (depending on the site you're on)

_This captures the multi-page data from the AVS or SSI site (each has their own button due to small differences)_

Let it do its thing. And it'll down the file for you when it is completed.

**Comparing the Data**

1. Extract the 'Compare CSV.zip' to a location on your computer.

2. Now that you have an export from AVS or SSI, copy that file into the directory of the CSV Compare folder.

3. Ensure the file is named search_list.csv (depending if you have extensions set up to be shown or hidden.)

4. Now, make sure you have an updated master_list.csv. This is a report generated from Web Intelligence.

5. Now, open up Powershell on your computer.

6. CD (change directory) to the directory of Compare CSV folder.

_Suppose the path is: C:\CodeProjects\Compare CSV_

you would type in Powershell:

```powershell
cd "C:\CodeProjects\Compare CSV"
```

_The quotes are needed above, especially if the path has any spaces in it. You will run into an error if you do not put your path in quotes._

If that was successful, you will now see in your powershell, the path that you inputted.

7. Run the application, you will type:

```node
node index
```

#### Anatomy of the Extension

```folder structure
./background.js - background process to monitor browser events like click events.
./content.css - styling for the popup.
./content.js - the main script executed when clicked on AVS to instruct to copy column data and export into csv.
./icon.png - icon, self explanatory.
./manifest.json - package info/details/basic instructions
./popup.html - the bare html pop-up when extension is clicked
./popup.js - instructions for task execution in the popup (button 1 or button 2, export 1 or export 2)
./script2.js - executes the same script as content.js except for SSI site as there are some differences between the two site's naming conventions.
```

### Things to Know

1. Must be used with Google Chrome
2. Files must be renamed to proper name before comparison application can be executed.

### Technical Details

_Full Script AVS_

```javascript
const fifthColumn = [];
const filename = "search_list.csv";

let index = document.querySelectorAll("[data-dt-idx]").length - 2;
let length =
  document.getElementsByClassName("paginate_button")[index].textContent * 1;

function grabFifthColumn() {
  var table = document.getElementById("resultsTable");

  for (var i = 1; i < table.rows.length; i++) {
    var cell = table.rows[i].cells[4]; // 4 represents the index of the 5th column (0-based index)
    fifthColumn.push(cell.textContent.trim().replace(/-/g, ""));
  }
}

function writeCsvFile(csvContent, fileName) {
  return new Promise((resolve) => {
    setTimeout(() => {
      const blob = new Blob([new Uint8Array([0xef, 0xbb, 0xbf]), csvContent], {
        type: "text/csv;charset=utf-8;"
      });

      if (typeof navigator.msSaveBlob !== "undefined") {
        // For Internet Explorer and Microsoft Edge
        navigator.msSaveBlob(blob, fileName);
        resolve();
      } else {
        const link = document.createElement("a");

        if (link.download !== undefined) {
          // For other browsers
          const url = URL.createObjectURL(blob);
          link.setAttribute("href", url);
          link.setAttribute("download", fileName);
          link.style.visibility = "hidden";
          document.body.appendChild(link);
          link.click();
          document.body.removeChild(link);
          resolve();
        }
      }
    }, 4000); // Delay of 4 seconds before generating the CSV file
  });
}

function copyWithDelays() {
  let promiseChain = Promise.resolve();

  for (let i = 0; i < length; i++) {
    promiseChain = promiseChain.then(() => {
      return new Promise((resolve) => {
        setTimeout(function () {
          grabFifthColumn();
          $("a#resultsTable_next").click();
          resolve();
        }, 2000);
      });
    });
  }

  return promiseChain;
}

if (length === 1) {
  fifthColumn.unshift("id");
  writeCsvFile(fifthColumn.join("\n"), filename);
} else {
  copyWithDelays().then(() => {
    fifthColumn.unshift("id");
    writeCsvFile(fifthColumn.join("\n"), filename);
  });
}
```

SSI

```javascript
const fifthColumn = [];
const filename = "search_list.csv";

let index = document.querySelectorAll("[data-dt-idx]").length - 2;
let length =
  document.getElementsByClassName("paginate_button")[index].textContent * 1;

function grabFifthColumn() {
  var table = document.getElementById("requestTable");

  for (var i = 1; i < table.rows.length; i++) {
    var cell = table.rows[i].cells[4]; // 4 represents the index of the 5th column (0-based index)
    fifthColumn.push(cell.textContent.trim().replace(/-/g, ""));
  }
}

function writeCsvFile(csvContent, fileName) {
  return new Promise((resolve) => {
    setTimeout(() => {
      const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });

      if (typeof navigator.msSaveBlob !== "undefined") {
        // For Internet Explorer and Microsoft Edge
        navigator.msSaveBlob(blob, fileName);
        resolve();
      } else {
        const link = document.createElement("a");

        if (link.download !== undefined) {
          // For other browsers
          const url = URL.createObjectURL(blob);
          link.setAttribute("href", url);
          link.setAttribute("download", fileName);
          link.style.visibility = "hidden";
          document.body.appendChild(link);
          link.click();
          document.body.removeChild(link);
          resolve();
        }
      }
    }, 4000); // Delay of 4 seconds before generating the CSV file
  });
}

function copyWithDelays() {
  let promiseChain = Promise.resolve();

  for (let i = 0; i < length; i++) {
    promiseChain = promiseChain.then(() => {
      return new Promise((resolve) => {
        setTimeout(function () {
          grabFifthColumn();
          $("a#requestTable_next").click();
          resolve();
        }, 2000);
      });
    });
  }

  return promiseChain;
}

if (length === 1) {
  grabFifthColumn();
  fifthColumn.unshift("id");
  writeCsvFile(fifthColumn.join("\n"), filename);
} else {
  copyWithDelays().then(() => {
    fifthColumn.unshift("id");
    writeCsvFile(fifthColumn.join("\n"), filename);
  });
}
```

### Validation Methods

Validating automated processes are important to ensure it functions and performs as expected and that various methods of validation are to be utilized to add layers of confidence in its performance to run with little supervision. This is especially important when it comes to working/processing large big data sets, but it can also very challenging to measure the reliability and accuracy of the results.

Here are some validation methods that are currently being considered:

#### Unit Testing:

I've separated the workflow into three separate processes.

**Capturing Data from SSA/Medicaid**

- Using script, ran in Google Chrome's
  Snippets that you'll save, like a browser bookmark.
  Only needs to be clicked on while on website to run this

- Run script to capture multi-page data from request inbox, formats it,
  and generates a csv

**Batch Processing of Data**
-A script ran locally on your computer. It only needs the file generated from the previous step to be placed in the folder. Double click the script to execute it

- Generates two lists for accounts not found and for accounts found

- Use list of "Accounts not Found" to quickly process (checkmarking 'accounts not found' on SSA site

**Generating Detailed Report for Accounts Found**

- Using the accounts found list generated from the previous step, place in Web Intelligence

- Use "Accounts Found" list as input for Web Intelligence Report

- Generates report of details/balances/etc for accounts

_The final step would require users to just manually paste the report's results for each individual's inquiry, and for 'accounts not found', users can go down the list and check off all the requests that appear on the 'accounts-not-found' list._

#### Integration Testing:

Test the integration of various components or modules within the automation script. This ensures that the different parts work together as expected and produce the desired outcome.

#### Sample Data Testing:

Use a representative sample of the large data set to validate the automation script. By selecting a subset of the data, you can perform tests more quickly and efficiently while still covering a significant portion of the dataset.

**Boundary Testing:** Test the automation script with data at the boundaries or limits of what it is expected to handle. This includes testing with minimum and maximum values, edge cases, and outliers to check if the script handles them correctly.

Cross-Validation: Divide the large dataset into multiple subsets and validate the script's performance on each subset separately. This helps to assess the script's ability to generalize and produce consistent results across different subsets of data.

**Parallel Validation:** Run the automation script in parallel with an existing manual process or another trusted system to compare the results. This allows you to verify the accuracy and consistency of the automation script against a known benchmark.

**Error Handling Testing:** Test the automation script's ability to handle errors, exceptions, and unexpected scenarios. Introduce deliberate errors or invalid data to assess how well the script detects and handles such situations.

**Performance Testing:** Evaluate the performance and efficiency of the automation script when dealing with large data sets. Measure factors like execution time, resource utilization, and scalability to ensure the script can handle the volume of data within acceptable timeframes.

**User Acceptance Testing:** Involve end-users or stakeholders to validate the results generated by the automation script. Gather their feedback, review the output, and ensure that it meets the desired requirements and expectations.

**Regression Testing:** After any updates or modifications to the automation script, perform regression testing to validate that the existing functionalities and outputs are still correct. This helps to ensure that the changes haven't introduced any unintended issues.

```html
<!-- index.html -->

<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="//unpkg.com/docsify/themes/vue.css" />
  </head>
  <body>
    <div id="app"></div>
    <script>
      window.$docsify = {
        //...
      };
    </script>
    <script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
  </body>
</html>
```

If you installed python on your system, you can easily use it to run a static server to preview your site.

```bash
cd docs && python -m SimpleHTTPServer 3000
```

## Another Section

If you want, you can show a loading dialog before docsify starts to render your documentation:

```html
<!-- index.html -->

<div id="app">Please wait...</div>
```

You should set the `data-app` attribute if you changed `el`:

```html
<!-- index.html -->

<div data-app id="main">Please wait...</div>

<script>
  window.$docsify = {
    el: "#main"
  };
</script>
```
