# Hermes Dashboard Cron Job Improvement Plan

## Current State Analysis

### Backend (Already Implemented)
- Full cron job model with rich features in `./cron/jobs.py`:
  - Skills/skill loading before job execution
  - Model/provider/base_url overrides per job
  - Script injection for data collection/change detection
  - Repeat settings (count-based execution limits)
  - Origin tracking for delivery
  - Detailed schedule parsing (once, interval, cron expressions)
  - Grace periods and catch-up logic
  - Output storage and error tracking

### Frontend (Limited)
- Current `CronPage.tsx` only exposes:
  - Prompt (required)
  - Schedule (required)
  - Name (optional)
  - Delivery destination (local/telegram/discord/slack/email)
  - Basic controls: create, pause/resume, trigger now, delete

## Improvement Goals

Enhance the dashboard to expose the full power of the cron system while maintaining usability.

### Phase 1: Basic Enhancements (Low Effort, High Impact)

1. **Repeat Count Field**
   - Add input for "Repeat" (how many times to run, empty = infinite)
   - Help text: "Leave blank to run forever, or specify a number (e.g., 5 to run 5 times)"

2. **Skills Field**
   - Add dropdown/multi-select for skills to load before job execution
   - Fetch available skills from `/api/skills` endpoint
   - Allow selecting multiple skills in order

3. **Model Override**
   - Add dropdown for model selection (fetch from `/api/model/info` or similar)
   - Allow overriding the default model for this specific job

### Phase 2: Advanced Enhancements

4. **Provider/Base URL Override**
   - Add fields for provider and base URL overrides
   - Provider dropdown (from available providers)
   - Base URL text input

5. **Script Injection**
   - Add textarea for script path whose output gets prepended to prompt
   - Help text: "Path to a Python script that runs before each execution. Its stdout is injected as context."

6. **Advanced Schedule Options**
   - Better schedule input with examples and validation
   - Split into: Simple (every Xh/Ym) and Advanced (cron expression/timestamp)
   - Show human-readable next run time

7. **Job Details View**
   - Click on a job to see full details:
     - Creation time
     - Last run time/status/error
     - Next run time
     - Repeat progress (X/Y times run)
     - Skills/model/provider used
     - Last output preview/link

### Phase 3: UX Improvements

8. **Job Templates**
   - Save common job configurations as templates
   - Quick-create from templates (e.g., "Daily summary", "Hourly data check")

9. **Bulk Operations**
   - Select multiple jobs for bulk pause/resume/delete

10. **Execution History**
    - View past runs of a job with output/logs
    - Link to output files in `~/.hermes/cron/output/{job_id}/`

## Technical Implementation Plan

### Frontend Changes (`./web/src/`)

1. **Update CronJob Type** (`lib/api.ts`)
   ```typescript
   export interface CronJob {
     id: string;
     name?: string;
     prompt: string;
     schedule: { kind: string; expr?: string; display: string };
     schedule_display: string;
     enabled: boolean;
     state: string;
     deliver?: string;
     last_run_at?: string | null;
     next_run_at?: string | null;
     last_error?: string | null;
     
     // New fields to expose:
     repeat?: { times: number | null; completed: number };
     skills?: string[];
     skill?: string;
     model?: string;
     provider?: string;
     base_url?: string;
     script?: string;
     origin?: any;
   }
   ```

2. **Update API Endpoints** (`lib/api.ts`)
   - `createCronJob`: Accept new fields
   - Return full job object with all fields

3. **Enhanced CronPage Component** (`pages/CronPage.tsx`)
   - Add repeat input field
   - Add skills multi-select (with async loading)
   - Add model/provider/base_url inputs
   - Add script input
   - Update form validation
   - Enhance job card to show more details
   - Add detail view modal/page

4. **Skills Loader Component**
   - Reusable component for skill selection
   - Fetch from `/api/skills`
   - Searchable multi-select

### Backend Verification

Ensure all endpoints already support the full job object:
- ✓ `create_job()` in `cron/jobs.py` accepts all parameters
- ✓ `update_job()` handles updates to all fields
- ✓ API endpoints in `web_server.py` pass through the full object
- ✓ No changes needed - backend already supports everything!

## Risks and Mitigations

1. **Complexity Overload**
   - Mitigation: Hide advanced fields behind "Show advanced options" toggle
   - Default view shows only essential fields

2. **Invalid Skill/Model Combinations**
   - Mitigation: Validate against available skills/models before saving
   - Show warnings but allow override (advanced users)

3. **Schedule Validation**
   - Mitigation: Reuse existing `parse_schedule()` validation logic
   - Show real-time validation feedback as user types

## Estimated Effort

- Phase 1 (Basic): 2-3 hours
- Phase 2 (Advanced): 4-6 hours  
- Phase 3 (UX): 3-5 hours
- Total: 9-14 hours

## Next Steps

1. Fork/copy the existing dashboard to work on improvements
2. Implement Phase 1 enhancements first
3. Test with real cron jobs
4. Gather feedback and iterate
5. Proceed to Phase 2 and 3

The key insight: **The backend already supports all the advanced features** - we just need to expose them in the frontend!