
# Publish and View Schedule with detailed code

## Description
This use case covers the workflow for publishing a schedule and then viewing it. It involves selecting a provider and facility from dropdowns, publishing the schedule, and then verifying the published schedule.

## Preconditions
- Playwright is installed and configured.
- The CSV file (`PublishSchedule.csv`) with provider and facility data is available in the specified directory.

## Steps
1. **Navigate to Publish Schedule Page**
   ```javascript
   const schPublish = new PublishSchedulePage(page);
   await schPublish.nvgtToPublishSchedule();

2.  **Read Data from CSV and Execute Workflow**
    
    ```javascript
    const records = parse(fs.readFileSync(path.join(__dirname, '../utils/PublishSchedule.csv')), {
        columns: true,
        skip_empty_lines: true
    });
    for (const record of records) {
        await test.step(`Publish Schedule: ${record.ID}`, async () => {
            const timeSlots = new PublishSchedulePage(page);
            await timeSlots.selectProvider(record.Provider);
            await timeSlots.selectFacility(record.Facility);
    
            const viewPubSch = new ViewSchedulePage(page);
            await viewPubSch.selectProvider();
            await viewPubSch.selectFacility();
            await viewPubSch.closeViewScheduleModal();
        });
    }
    
    ```
    
3.  **Publish Schedule**
    
    ```javascript
    await schPublish.selectProvider(record.Provider);
    await schPublish.selectFacility(record.Facility);
    
    ```
    
4.  **View Published Schedule**
    
    ```javascript
    const viewPubSch = new ViewSchedulePage(page);
    await viewPubSch.viewSchedule();
    
    ```
    

## Expected Results

-   The schedule should be published successfully for the selected provider and facility.
-   The published schedule should be viewable and match the data from the CSV file.

## Diagrams and Screenshots

Might add the flowchart or screenshot of UI here in future

## Best Practices

-   **Code Organization**: Ensure that the page object models (`PublishSchedulePage`  and  `ViewSchedulePage`) are well-structured and reusable.
-   **Error Handling**: Implement error handling for scenarios where the provider or facility selection fails.
-   **Performance Optimization**: Optimize the CSV reading and data parsing to handle large datasets efficiently.
