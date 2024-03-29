# Import CSV/XLS(x).

This function can be used to import the contents of an import file into a Betty Blocks application.<br />
You can map column names from the import file to properties in your data model.<br />
You can specify different import file columns to database property mappings for new or updated records.<br />
With the deduplication option, you can prevent duplicate records based on a column name of your import file with the unique value of the records.

## Configure the function

### URL TO THE IMPORT FILE:

![image](https://github.com/Betty-Services/import-csv/assets/96063344/d5bec7e4-b6d0-40dd-b71f-741aecfa9d2c)

Here you need to specify a URL to your import file. This can be a static public reachable URL or a text variable from a file property of a record (you can use the expression step to retrieve the URL of a file property).<br />

### IMPORT FILE TYPE:

![image](https://github.com/Betty-Services/import-csv/assets/96063344/6c460217-c271-409f-bfc3-d33bf1099057)

Enter the type of the import file. In general, the size of a CSV file can be larger than an XLS(X) file because an Excel spreadsheet also stores additional meta-data.

### IMPORT MODEL:

![image](https://user-images.githubusercontent.com/96063344/227200400-48686778-9c22-4d61-970d-0961973887ef.png)

Specify the data model in which the records will be imported.

### IMPORT MAPPING:

![image](https://user-images.githubusercontent.com/96063344/227200534-0323cf3f-ecf7-4f1d-83ab-feaa30cd099b.png)

A key/value combination that allows you to map your import file column names to the properties of the selected model.<br />
The key column contains the (exact) column name of your import file.<br />
The value column contains the database name of your property (in snake_case).<br />
For belongs-to relations, you can use the notation: "modelname.property_name" (in snake_case) combination.<br />

### FORMAT MAPPING FOR IMPORT COLUMNS:

![image](https://github.com/Betty-Services/import-csv/assets/96063344/6fb2273a-5518-4ee2-9cab-cd6e1e421b79)

A key/value combination to allow you to specify if a column is a checkbox, decimal, number, or a specific date format.<br />
Text fields will all work out of the box and should not be included here.<br />

The key column specifies the CSV column name.<br />
The value column specifies either the word "checkbox", "decimal", "number", "Date", "Time", or "Datetime".<br />
For the date, time, and datetime options, you will need to specify how the item is formatted in your file. The notation is as follows:</br>
Date, date-fns format or Time, date-fns format, or Datetime, date-fns format.</br></br>For example: "Date, dd-MM-yyyy" or "Date, MM/dd/yyyy".</br></br>
We use the formats defined by the date-fns package and the specifications can be found here: https://date-fns.org/v2.16.1/docs/format<br />
Make sure a mapping exists for each of your date columns in the import file, otherwise the column will not be imported correctly.<br />
See the limitations below for the supported patterns.

### DEDUPLICATE RECORDS (UPDATE RECORDS IF MATCHED):

![image](https://user-images.githubusercontent.com/96063344/227200721-31d1812b-87aa-4529-9768-ac4441ff872d.png)

This toggle will determine if the import records will be matched to records inside your Betty Blocks application.<br />
If selected, make sure you enter the UNIQUE RECORD IDENTIFIER option.<br />

### UNIQUE RECORD IDENTIFIER (IMPORT COLUMN NAME):

![image](https://user-images.githubusercontent.com/96063344/227200790-0ccb9d8a-6854-479f-a045-f7bb2169cfe9.png)

Specify the column name (from your import file) that uniquely identifies your records.

### UNIQUE RECORD TYPE

![image](https://user-images.githubusercontent.com/96063344/232803057-0051661c-d364-4ce0-bf73-fd93879ba95a.png)

Specify the property type of the unique record property type. Current supported types are Id, Text, Number, or Decimal property types.
Please make sure you enter this when selecting deduplication.

### UPDATE IMPORT MAPPING FOR EXISTING RECORDS (LEAVE EMPTY TO USE DEFAULT):

![image](https://user-images.githubusercontent.com/96063344/227200906-b10a21ac-4ced-48b4-ae49-a3add274cf7f.png)

Same principle as the IMPORT MAPPING, but these mappings will only be used for existing records. For new records, the IMPORT MAPPING options will be used.<br />
This should be left empty if you do not want to distinguish the columns to import between creates and updates.

### TURN ON BATCHED PROCESSING FOR THE IMPORT

![image](https://github.com/Betty-Services/import-csv/assets/96063344/df4c5306-6b29-45c5-b0bd-9f816ea131e6)

When on, the system will use multiple calls (batches) for processing the import and store the progress in the model/properties selected below. This can be useful if you have huge amounts of data and the import can't finish in a single run.

### THE MODEL IN WHICH WE STORE THE BATCH SIZE AND CURRENT OFFSET.

![image](https://github.com/Betty-Services/import-csv/assets/96063344/899683ff-ac4d-4f1b-a95a-e5c00b908f13)

### THE (NUMBER) PROPERTY TO STORE THE SIZE OF EACH BATCH.

![image](https://github.com/Betty-Services/import-csv/assets/96063344/8644c183-b231-4c93-8844-4655f7901cff)

### BATCH SIZE (NUMBER OF RECORDS TO PROCESS IN A BATCH).

![image](https://github.com/Betty-Services/import-csv/assets/96063344/5306af99-5470-4e30-9680-1c9b06d42686)

### THE (NUMBER) PROPERTY TO STORE THE OFFEST WHILE RUNNING THE IMPORT.

![image](https://github.com/Betty-Services/import-csv/assets/96063344/8981da78-50cc-4a70-bc48-1ce8660e501d)

### THE (TEXT) PROPERTY TO STORE THE FILE NAME TO UNIQUELY IDENTIFY THIS IMPORT.

![image](https://github.com/Betty-Services/import-csv/assets/96063344/96096206-de5e-4176-aa70-f47c8ab142c5)

### TURN ON LOGGING FOR THIS ACTION:

![image](https://user-images.githubusercontent.com/96063344/227200968-a8898a64-1ae9-4c19-b84e-9d82456b02eb.png)

When selected, debug information will be logged into the logs.

### VALIDATE REQUIRED COLUMNS

When set to true, add a \* after the name in the KEY fields of the Import Mapping to the columns that you would like to validate as required (for example firstName\*).

### RESULT:

![image](https://user-images.githubusercontent.com/96063344/227201040-034a15b3-c7af-4745-a19b-55a1b5583da9.png)

This is the (Text) output of the function and will contain the number of records that have been created and the number of records updated in the following format:<br />
records created: 1, records updated: 1

### Using the result

The output of this action function could be used by other action functions or as the final result of your action.

## Packages

This action function uses the following packages:

~~- https://www.npmjs.com/package/papaparse~~ in v2.2 papaparse has been replaced by our internal helper function parseData.

- https://www.npmjs.com/package/date-fns
- https://www.npmjs.com/package/lodash

The versions of the used packages can be found in the package.json file.

## Limitations

- Limited support for relational data (belongs to relations only).
- At the time of this writing Betty Blocks NextGen actions have a maximum runtime of 60 seconds.<br />This function (and any other functions in your action) needs to complete within 60 seconds.<br />
  If the import does not exceed this 60-second time limit, turn on batched processed, so that you run the same action multiple times and the step will continue processing from the moment the action timed out previously.
- Time properties have not been tested and are currently not supported.
- The date-fns format patterns that have been tested include yyyy (year), dd (day in 2 digits), MM (month in 2 digits), HH (hours), mm (minutes), ss (seconds).
- AM/PM times in date time properties are currently NOT supported.
