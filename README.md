# RetroPGF Round 2 Automation
In order to fill the RetroPGF Round 2 Form as a Badgeholder; there is currently a very manual process of moving allocations from a Google Sheet into a Deform. 

Ludens -- from [Lattice](https://twitter.com/latticexyz) -- is a badgeholder for RetroPGF, and we worked on a process to make this super easy for them and others to import their allocations into the final form.

<img width="900" alt="CleanShot 2023-02-16 at 17 44 19@2x" src="https://user-images.githubusercontent.com/21203499/224312542-2df00efd-0b43-410d-a513-9e23aa06575a.gif">

_Using the automation_

# How to use
__Summary:__ Instead of duplicating the OP provided Sheet, you duplicate a [modified Google Sheet](https://docs.google.com/spreadsheets/d/1fSRLfEw47hc7l33gZVs3sLa7rvXQom03wfjGz6FpUpo/edit#gid=0) containing an AppSript that allows for JSON export of the allocations.
Then, when visiting [the form](https://app.deform.cc/form/85e65189-6679-4a13-908e-c42ea3cf1498/), you can import the JSON into the form into a Bookmarklet.

## Step 1: Make a Copy of the modified Google Sheet
__Regardless of how many exports / imports you do, this step is to be done only once__

Open the [modified Google Sheet](https://docs.google.com/spreadsheets/d/1fSRLfEw47hc7l33gZVs3sLa7rvXQom03wfjGz6FpUpo/edit#gid=0); and make a copy of it.

__Important: Use File -> Make a Copy instead of duplicating the the sheet; otherwise the AppScript won't be carried over.__

<img width="383" alt="CleanShot 2023-02-16 at 17 44 19@2x" src="https://user-images.githubusercontent.com/21203499/224313286-880b7868-7558-4b8e-a5f2-0938c7ff37ae.png">

You will need to accept a disclaimer about AppScript

## Step 2: Go on the sheet you copied, and export your Ballot

Fill in your ballot the same way you would with the original OP Foundation provided sheet, then click on the "OP Vote Allocation" menu and choose "Export Vote Json". If the menu doesn't appear, reload the page. If it still doesn't appear, you probably duplicated the sheet instead of making a copy.

<img width="383" alt="CleanShot 2023-02-16 at 17 44 19@2x" src="https://user-images.githubusercontent.com/21203499/224313895-f5f53855-da36-4c06-9b83-ff477783d2fa.png">

You can now copy your ballot as a JSON file, either keeping it into your clipboard and saving it into a text file.

There is also a feature to import a Ballot! We hope Badge Holders will share their ballot in a json format on Discord and Telegram, and this function will make it easy to import someone else's ballot into your sheet.

## Step 3: Create the bookmarklet to fill the form from a JSON export
__Regardless of how many exports / imports you do, this step is to be done only once__ 

You will need to create a bookmarklet with the code below:

```javascript
javascript: (function () {
  var inputJsonString = prompt("Enter Project Allocation JSON");
  var projectAllocations = JSON.parse(inputJsonString);

  var inputs = document.evaluate(
    "//input",
    document,
    null,
    XPathResult.ORDERED_NODE_SNAPSHOT_TYPE,
    null
  );
  for (var i = 1; i < inputs.snapshotLength; i++) {
    var input = inputs.snapshotItem(i);
    var projectIndex = i - 1;
    var project = projectAllocations.find((projectAllocation) => {
      return projectAllocation.projectIndex === projectIndex;
    });

    input.value = project ? Math.round(project.allocation * 100) : 0;
  }
})();
```

This bookmarklet tutorial assumes you are using Chrome, but this will work in any browser.

1. Right click your bookmark bar and hit `Add Page...`
<img width="383" alt="CleanShot 2023-02-16 at 17 44 19@2x" src="https://user-images.githubusercontent.com/4297920/219503796-a70d0844-5e07-4c59-ba88-ea90ae5597e2.png">

2. Paste in the code snippet found here for the URL.
<img width="510" alt="CleanShot 2023-02-16 at 17 44 52@2x" src="https://user-images.githubusercontent.com/4297920/219503965-a9c82b43-b44c-4822-8898-8784cc36195b.png">

You now have the Bookmarklet that imports a Ballot in the JSON format into the [RetroPGF Round 2 form](https://app.deform.cc/form/85e65189-6679-4a13-908e-c42ea3cf1498/)

## Step 4: Import your Ballot
Go to the [RetroPGF Round 2 form](https://app.deform.cc/form/85e65189-6679-4a13-908e-c42ea3cf1498/); then click on your bookmarklet

<img width="383" alt="CleanShot 2023-02-16 at 17 44 19@2x" src="https://user-images.githubusercontent.com/21203499/224315362-0c88f0f4-a2c3-47a0-8768-a4e965a52363.png">

<img width="383" alt="CleanShot 2023-02-16 at 17 44 19@2x" src="https://user-images.githubusercontent.com/21203499/224315380-32a1f868-828c-4c63-8535-cdf205945fa4.png">

Paste your JSON Ballot created in Step 2 from your clipboard (or first copy it from the text file you saved it to) into the text box and click "Ok".

The form should now reflect your ballot!
