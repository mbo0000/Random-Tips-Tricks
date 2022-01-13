# Reference to a Dynamic Range 
A little bit of context: In a recent collaboration project, my colleagues and I needed to create a new tab each month, at the beginning of the new month, to be used for data ingestion. The newly created tab will have the same name format of "mm-yyyy". The data will be processed in another tab that references the current month tab by its name and specified range.

### Problem:
With each new tab created, the function referencing value range has to be updated manually as well. This created issue with the validity of our data when the function is not referencing the correct tab. This happened often as many of us either forget the Slack reminder messages or were not available to do so. The downstream effect causes more issues with our internal teams. 

For example, the function `INDEX('11-2020'!A1:D11)` will return the data in range A1:D11 of the `11-2020` tab. When December rolls around, this will become an issue if the tab name is not updated to such. The processed result from the data will be invalid. 

### Solution:
Instead of having a static data range, we have the function point to another cell value that contains the most current tab name. To do this, the cell containing the updated tab name has to be dynamic as well as formatted correctly. The formula is `=CONCATENATE("'",MONTH(TODAY()) &"-",YEAR(TODAY()),"'")`. Assuming the current month is December and year is 2020, the result string is `'12-2020'`.

Using the example from this [Google Sheet](https://docs.google.com/spreadsheets/d/1iIklqK1gsn7O5kH7MCvWXCExigsqSAA27-TVvmyYjNE), the cell B1, in the Main tab, contains the name for the latest tab with the data to be processed. Cell B2 contains the `INDIRECTION` function that returns the data values of the range specified. B1 now can be automatically updated using other formulas(MONTH and YEAR) to fit our needs. 

This method works for INDIRECT and other similar functions where the data range reference can be a string type. It can be expanded and applied to other use cases based on business or project needs. 

For IMPORTRANGE example: `=IMPORTRANGE("1iIklqK1gsn7O5kH7MCvWXCExigsqSAA27-TVvmyYjNE", ""&B1&"!A1")`
