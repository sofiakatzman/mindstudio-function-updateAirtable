# Update Airtable Record Function Documentation

This function allows you to update a single record in an Airtable table with new field values.

## Configuration

### Authentication
- **Airtable API Key** (Required)
  - Your Airtable API key that starts with either 'pat' or 'key'
  - Keep this secure and never share it
  - Can be found in your Airtable account settings

### Record Configuration
- **Base ID** (Required)
  - The ID of your Airtable base, starts with 'app'
  - Found in your Airtable base URL
- **Table ID** (Required)
  - The name or ID of your table
  - Can be the table name or the ID starting with 'tbl'
- **Record ID** (Required)
  - The ID of the specific record you want to update
  - Starts with 'rec'
  - Can be found in the record URL or via API

### Update Data
- **Fields to Update** (Required)
  - A JSON array of objects containing the field names and values you want to update
  - Each object should have the column name as the key and the new value as the value
  - Example format:
    ```json
    [
      {"Column Name": "New Value"},
      {"Another Column": "Another Value"}
    ]
    ```

## Outputs

The function sets the following variables:

- `updateSuccess` (boolean)
  - `true` if the update was successful
  - `false` if the update failed
- `error` (string, only set if update fails)
  - Contains the error message if the update failed

## How It Works

1. The function first validates all inputs to ensure they're present and properly formatted.

2. It parses the update data JSON string and converts it into the format required by Airtable's API.

3. Makes a PATCH request to Airtable's API with your authentication and update data.

4. Handles the response:
   - On success: Sets `updateSuccess` to true
   - On failure: Sets `updateSuccess` to false and stores the error message

5. Logs the progress and results to the console for debugging.

## Example Usage

Configuration:
```json
{
  "apiKey": "patXXXXXXXXXXXXXX",
  "baseId": "appXXXXXXXXXXXXXX",
  "tableId": "tblXXXXXXXXXXXXXX",
  "recordId": "recXXXXXXXXXXXXXX",
  "updateData": "[{"Name": "John Doe"}, {"Status": "Active"}]"
}
```

This would update the specified record's "Name" field to "John Doe" and "Status" field to "Active".

## Error Handling

The function handles various error cases:
- Missing or invalid configuration
- Malformed JSON in update data
- Network errors
- API errors (rate limits, permissions, etc.)
- Invalid record/table/base IDs

All errors are logged to the console and stored in the `error` variable.

## Best Practices

1. Always validate your update data format before running the function
2. Use valid column names exactly as they appear in Airtable
3. Keep your API key secure
4. Monitor the console logs for detailed operation information
5. Check both `updateSuccess` and `error` variables to handle results in your workflow
