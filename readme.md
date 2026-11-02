## Workflow Architecture

1. **Scheduled Trigger:** Starts the automation on a configured schedule.
2. **Retrieve Patient Records:** Reads patient information from Google Sheets.
3. **Eligibility Check:** Evaluates patient status, follow-up count, follow-up flag, and time since last contact.
4. **Generate Follow-Up Attempt ID:** Creates an identifier using the patient's Telegram ID and next follow-up number.
5. **Duplicate Check:** Searches the follow-up log for an existing attempt.
6. **Create Pending Log:** Records the new follow-up attempt before sending.
7. **Generate AI Message:** Produces a personalized message based on the patient's context.
8. **Send Telegram Message:** Delivers the follow-up.
9. **Update Follow-Up Log:** Records successful delivery.
10. **Update Patient CRM:** Updates the follow-up count, contact timestamp, and follow-up flag.
11. **Mark CRM Synchronized:** Records completion of the CRM update.

## Follow-Up Eligibility Rules

A patient qualifies only when all conditions are satisfied:

- Patient status is `Interested` or `Follow Up`.
- Follow-up count is below `3`.
- Follow-up flag is `false`.
- At least two days have passed since the last contact.

Patients who do not meet these conditions are excluded from the sending process.

## Duplicate Prevention

Before generating a new message, the workflow creates a follow-up attempt ID and searches the follow-up log.

If an existing attempt is found, the workflow checks its recorded status instead of immediately sending another message.

This helps prevent duplicate follow-ups during repeated executions.

## Testing and Validation

Five planned test scenarios were completed successfully.

| Test | Scenario | Result |
|---|---|---|
| 1 | Successful personalized follow-up and CRM update | Passed |
| 2 | Previously contacted patient excluded by eligibility rules | Passed |
| 3 | Existing completed attempt prevents duplicate delivery | Passed |
| 4 | Ineligible patients correctly filtered | Passed |
| 5 | Simulated CRM update interruption after message delivery | Passed |

### Failure Simulation

A controlled test simulated a successful Telegram delivery followed by an incomplete CRM update.

The follow-up log retained the sent-message status while CRM synchronization remained pending.

This demonstrated that the workflow could preserve the recorded delivery state without automatically resending the same attempt.

It did not test every possible API failure or implement automatic CRM repair.

## Setup Requirements

1. Install or access an n8n instance.
2. Configure Google Sheets, OpenAI, and Telegram credentials.
3. Prepare a patient CRM sheet and a follow-up log sheet with the expected fields.
4. Import the workflow JSON.
5. Configure the spreadsheet references and Telegram recipient mapping.
6. Review the AI prompt and eligibility conditions.
7. Run controlled tests using authorized test accounts before activation.

The portfolio export may contain placeholders and requires configuration before execution.

## Screenshots

Add sanitized screenshots of:

- Complete n8n workflow
- Successful follow-up execution
- Duplicate prevention branch
- Eligibility filtering
- CRM failure simulation and follow-up log

## Known Limitations

- **CRM Recovery:** Failed or pending CRM synchronization requires manual investigation. Automatic recovery is not implemented.
- **Pending Messages:** Pending follow-up attempts are held for review rather than automatically retried.
- **Follow-Up Flag Reset:** The current workflow does not automatically reset the `follow_up` flag to enable future follow-up attempts.
- **Duplicate Prevention:** Attempt-ID checks reduce duplicate sends but do not guarantee exactly-once delivery under concurrent executions or uncertain delivery outcomes.
- **Deployment:** Additional reliability, security, privacy, and operational validation would be required before using the system with real patient information.

## Potential Future Improvements

- Automatic CRM recovery with safe retry logic
- Monitoring and alerts for unresolved follow-up attempts
- Controlled follow-up scheduling and flag resets
- Support for additional messaging channels
- More robust handling of API failures and concurrent executions

## Project Status

**Portfolio Prototype — Core Workflow Tested**

All five planned functional tests passed. Further validation and operational safeguards are required for production deployment.
