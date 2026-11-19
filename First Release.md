# First Release: Mobile Printing


## Objective

Let patrons submit a PDF from their own phone or laptop and collect it through Fernwick's existing Print Release workflow, while staff can check job status. The release tests usability and whether the Print platform supports mobile jobs without new hardware.

## Core Workflow

1. Patron opens a library-specific page by link or QR code and selects a pickup branch if one is not already selected.
2. Patron uploads a PDF and selects color and copies.
3. The system validates the file and submits it to the Print Queue. A loading indicator appears while the job is Pending.
4. Once the Print Queue accepts the job, the patron immediately sees a success confirmation, Release Code, and pickup instructions. The job is Ready.
5. At the correct branch's Print Release Station, the patron enters the code and completes the existing payment or authorization process. The job changes to Printing, then Completed when the Print Service confirms success.
6. At another branch, release is denied. The correct branch is shown only if secure lookup is supported.
7. If submission or printing fails, the system shows an error and appropriate instructions.
8. **Staff:** Authorized staff can search by Release Code within their branch to view job status and basic details. They cannot open documents, cancel jobs, or replace codes.

## In Scope

- Responsive web page without account registration.
- Library branch selected through a dropdown or preselected by a library-specific link or QR code.
- One PDF per job, maximum 10 MB; color and copy settings; clear file validation errors.
- Print Queue submission, unique Release Code generation after queue acceptance, and kiosk validation.
- **Lost codes are not recoverable.** The confirmation screen displays the code prominently with a copy button and a reminder to save it. Patrons who lose their codes must submit a new job.
- Staff read-only lookup: job status, submission time, page count, color, copies, and failure reason.
- Documents expire after 24 hours by default. Fernwick support may configure the retention period; no library administrator interface is included.
- Basic tracking of queue acceptance, printing results, rejected file types, repeat uploads, and staff-assisted jobs.

## Out of Scope

**Future enhancements, based on pilot findings:** Email-to-print, multiple documents, cross-branch pickup, staff cancellation, secure Release Code recovery, and library administrator settings.

**Not planned:** Replacement of the existing Print system, a new payment processor, AI features, or staff access to patron documents.

## Job Status

| Status | Meaning |
|---|---|
| Pending | File validated and submitted; waiting for Print Queue acceptance. No Release Code is available. Submission fails if the queue does not respond within 2 minutes. |
| Ready | Print Queue accepted the job. A valid Release Code is available for pickup. |
| Printing | Code verified, payment or authorization approved, and the job released for printing. |
| Completed | Print Service confirmed successful printing. |
| Failed | Queue rejected or timed out, printing failed, or completion was not confirmed within 10 minutes. The failure reason is recorded. |
| Expired | An uncompleted job exceeded its retention period. Its document is deleted and its code becomes invalid. |

**User-facing behavior:** Pending appears as a submission loading state, not a separate page. The success confirmation and Release Code appear only when the job becomes Ready.

Only Ready jobs can be released. A code becomes unusable after successful release, but a kiosk connection failure before release confirmation must not consume it. Duplicate printing must be prevented.

Reuploading creates a new job with a separate Release Code and expiration time. It does not cancel the previous job.

Documents are deleted after the retention period, including documents from completed jobs. Refunds and reprints follow each library's existing process.

## Success Metrics

| Metric | Pilot Target |
|---|---|
| Queue acceptance rate | At least 95% of valid submissions |
| Print success rate (Completed ÷ released jobs) | At least 95% |
| Pickup rate (released jobs ÷ accepted jobs) | Track baseline |
| Repeat uploads (same file within 2 hours) | Track baseline |
| Staff-assisted jobs per 100 jobs | Track baseline |
| Rejected file types | Track frequency by format |
| Integration compatibility | Limited integration cost and no required new hardware |
| Willingness to pay | At least 2 of 3 pilot customers |
| Cost feasibility | Updated model supports 18-month payback under the approved development budget |

Targets, conversion assumptions, and timeouts are planning assumptions, not confirmed Fernwick performance benchmarks.

Proceed to wider development only if integration is feasible, at least two pilot customers confirm willingness to pay, and the updated model supports 18-month payback.

## Technical Assumptions

The provided data does not confirm Fernwick's Print Queue API, hardware compatibility, authentication, or payment integration.

The assessment prototype simulates queue acceptance, queue failure, kiosk authorization, and printing results. It does not connect to actual printers. Real integration must be validated before a production pilot.

Release Codes must be difficult to guess, time-limited, and protected against repeated failed attempts. The prototype uses mock authentication for Staff Lookup; production requires actual authentication and branch-level authorization.

Repeat-upload tracking uses a file hash rather than storing document contents for comparison.

Princh, ePRINTit, and EnvisionWare workflows are design references only and do not confirm Fernwick's technical capabilities.

## Acceptance Criteria

### AC1. Submit a Valid PDF

**Given** a patron opens a library-specific Mobile Printing page and selects a PDF up to 10 MB,

**When** they select print settings and submit the file,

**Then** the system validates the file, submits it to the Print Queue, and shows a loading state while the job is Pending. No account registration is required.

### AC2. Reject an Invalid File

**Given** a patron selects a non-PDF file or a PDF larger than 10 MB,

**When** they attempt to submit it,

**Then** the system rejects the file with a clear explanation and does not create a print job.

### AC3. Receive a Release Code

**Given** a valid print job has been submitted,

**When** the Print Queue accepts the job,

**Then** its status changes to Ready and the patron immediately sees a success confirmation, unique Release Code, selected branch, pickup instructions, and a reminder to save the code.

The Release Code must not appear before queue acceptance.

### AC4. Handle Submission and Printing Failures

**Given** a valid print job has been submitted,

**When** the Print Queue rejects it or does not respond within 2 minutes,

**Then** the job becomes Failed, no usable Release Code is issued, and the patron can submit the document again.

**Given** a job has been released for printing,

**When** printing fails or remains unconfirmed for 10 minutes,

**Then** the job becomes Failed with a recorded reason, the code cannot be reused, and the patron is instructed to contact library staff.

### AC5. Release and Complete a Print Job

**Given** a Ready job has a valid Release Code,

**When** the code is entered at the correct branch and payment or authorization succeeds,

**Then** the job changes to Printing and becomes Completed only after the Print Service confirms successful printing.

If the kiosk disconnects before release is confirmed, the code remains available for a safe retry without creating a duplicate print job.

### AC6. Handle Unusable Codes and Wrong Branches

**Given** a Release Code is invalid, expired, or already used,

**When** a patron enters it at the kiosk,

**Then** release is denied without revealing job information or allowing duplicate printing.

**Given** a valid Release Code belongs to another branch,

**When** it is entered at the wrong kiosk,

**Then** release is denied. The correct branch is displayed only if secure cross-branch lookup is supported. Otherwise, the patron is asked to check the pickup location on their confirmation.

### AC7. Expire and Delete a Job

**Given** an uncompleted job has exceeded its configured retention period,

**When** the expiration process runs,

**Then** the job becomes Expired, its document is deleted, and its Release Code can no longer be used.

Documents belonging to completed jobs are also deleted when their retention period ends.

### AC8. Staff View Job Status

**Given** an authorized staff member is signed in for a library branch,

**When** they search using a valid Release Code,

**Then** they can view the job's status, submission time, page count, color, copies, and failure reason, if available.

Staff cannot access document contents, view jobs from other branches, cancel jobs, or generate replacement codes.