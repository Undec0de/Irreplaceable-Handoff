# Session Handoff — Q1 newsletter editorial calendar and launch workflow

## Session scope & starting point
Planning the first six issues of a bi-weekly Italian/English tech newsletter focused on practical AI workflows and self-hosting, targeting a launch in early January. Constraints: maximum 4 hours of writing time per issue and a repeatable research-to-publish checklist stored in Notion.

## Completed work & decisions
- [verified] Locked bi-weekly Tuesday 08:30 CET publishing cadence — balances depth with the 4-hour production budget.
- [verified] Finalized the 6-issue editorial outline in Notion page `notion://editorial-q1-2027-calendar` — alternates one hands-on tutorial with one tool/workflow comparison.
- [verified] Drafted Issue #1 ("Building a Portable AI Handoff Workflow") in `https://docs.google.com/document/d/example-issue-01` — intro and core framework complete (~950 words).
- [unconfirmed] Estimated Ghost email template HTML payload at ~75 KB — needs a test send to Gmail before scheduling.

## Dead ends & constraints discovered
- Weekly cadence rejected after time-boxing test — leaves no buffer for hands-on testing before recommending tools.
- Rejected covering breaking AI news roundups — overlaps with daily aggregators and decays too quickly for evergreen archive value.
- Avoided embedding full raw code files inside emails — triggers Gmail clipping above 102 KB; link to GitHub gists/repos instead.

## Priority references
- Plan / spec file: `notion://editorial-q1-2027-calendar` — master schedule, issue angles, and target publish dates; read first.
- `https://docs.google.com/document/d/example-issue-01` — Issue #1 working draft (needs closing section and tool picks).
- Artifact: `Newsletter Pre-Publish Checklist v1` — verification steps for links, tone, and mobile readability.

## Active state
- Background processes / jobs / routines: none
- Servers / ports / branches / connectors: Notion MCP connected to workspace `Content Hub`
- Open drafts / pending approvals / external actions: Issue #1 draft open in Google Docs; awaiting user review on the two "Tested Tools" picks before finalizing

## Verification & acceptance checks
- Issue #1 total word count stays between 1,100 and 1,400 words.
- Every recommended tool or workflow includes one concrete real-world limitation tested firsthand.
- Final HTML email size stays under 90 KB in a live Gmail test send to prevent clipping.

## Deferred & open questions
- Deferred: Sponsor media kit and pricing tier — revisit after reaching 2,500 active subscribers.
- Open: Should Issue #1 be published in Italian only at launch or bilingual (IT + EN) from day one? — impacts whether we need a second Substack/Ghost section before scheduling.

## Next actions
- Start here: Confirm the launch language choice (Italian only vs. bilingual) with the user.
- Then (ordered): 1) Write the two "Tested Tools" blurbs and closing section in `https://docs.google.com/document/d/example-issue-01`; 2) Run `Newsletter Pre-Publish Checklist v1` and check total word count; 3) Send a test email to Gmail to verify HTML size stays under 90 KB.
