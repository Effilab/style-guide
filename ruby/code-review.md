# Code review workflow

## Merge request lifecycle
### Prerequisites:
- Developer works on a Jira ticket. Ticket is marked as `In progress`
- The code reviewer field must be defined on ticket

### Once ready for code review, open the MR:
1. Open a new MR on gitlab (cf. _Q&A -> How to open a Merge Request ?_ for more details)
2. Add a comment to the Jira ticket with the MR's URL
3. Move the ticket to `Ready code review`

### Review worflow:
1. Reviewer adds comment on the MR
2. Reviewer sets MR assignee to the developer
3. Reviewer sets Jira ticket to `Todo`
4. Developer handles remarks / makes changes / responds to discussion
5. Developer sets MR assignee to the reviewer
5. Developer sets Jira ticket to `Ready code review`
5.a If needed go back to 1.
5.b If review is satisfying continue
6. Reviewer resolves discussions
6. Reviewer sets MR assignee to the developer
7. Reviewer adds a comment on Jira ticket, eg. `Code review OK`
7. Reviewer sets Jira ticket to `To Do`
8. Review is over, the developer prepares QA and ticket workflow continues

## Do
- Apply Rubocop recommendation before code review unless you have a good reason not to do so[1]
- When developer and reviewer can't aggree on a discussion within a reasonable time/exchanges, ask the Lead Developer to settle
- The reviewer must resolve discussion once changes are done and validated
- Be gentle with developer, not with code
- Qualify the level of need of your remarks using corresponding emoji (emojis TBD)
- If some exchanges are done orally, please write a summary of the decisions on the MR

## Don't
- Assign a MR to the reviewer if your CI build is totally broken
- Assign a MR to the reviewer if your changes are conflictuals with target branch
- Mark MR for code review if the diff shows irrelevant changes
- The developer must not resolve discussion unless agreed with reviewer
- Set automatic merge/Merge if discussion are still open and/or reviewer has not validated by a comment on Jira
- Do not focus your comments on code style unless it really misses some readability / introduces performance issues
- Add comment directly on commit. All discussion must be created on MR
- Review too much code in a row, split your review to remain effecient
- If you don’t feel qualified to give approval, get the right person to look at the code by adding a mention on the diff
- Manually merge code if MR has been rejected / closed

## Q&A

### How to open a Merge Request ?
- Name must starts with ticket id, eg. _IT-XXXX Super fancy feature on product_
- Target branch must be set to `master` or to the epic `branch` in case of multiple tickets
- Assignee must be the reviewer
- Add at least one label qualifing the feature. You can add new label if needed.
- Check `Remove source branch when merge request is accepted` option

### What if I open the MR before I'm ready for code review ?
- Assignee must be the developer
- The title must starts with `WIP:`


[1]: If you find yourself often not following the same rubocop rules, please discuss it with the Lead Developer. Know that rules can be changed if justified.
