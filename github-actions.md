GitHub Actions Deep Dive
===
Two separate sessions that are _NOT_ covered in this workshop
  Securing GitHub Actions
  Comparing to Jenkins

GitHub Actions book available on O'Reilly early access  (****)
In-depth separate session on securing github actions

Actions (capital 'A' => framework)
actions (lowercase 'a' => plugins, modules)

Actions is the entire pipeline
workflows are like Jenkinsfiles
actions are individual "operations" like checkouts

Can update GitHub issues as part of actions

Migration tool to try to convert other types of pipeline frameworks

GitHub Actions provide starter workflows (can be used as templates for building your own)

GitHub Actions live in a .github/workflows directory in your source code (****)

Actions menu item now on just about everything in GitHub

repository_dispatch is a way to call into github actions run externally via something like a webhook

action events can trigger an action workflow

on: pull_request_review_comment =>  (*** interesting)
standard YAML syntax


on: workflow-dispatch (I want to be able to run the workflow from the web interface, takes params, etc.)
  great way to test without having to invoke via normal triggers of push

on: repository-dispatch (Webhooks that can cause all of the workflows to be run -- try to use externally)

workflow reuse events
on: workflow-call (invoked by another workflow)

activity types => opened, edited, just about every github event


jobs similar to stages in Jenkins (jobs have steps)

Separate out by job based upon level of "success/failure" view you wish to see (all steps-within you only know that the whole job/stage failed or succeeded)

runs-on: ubuntu-latest (github labels for github hosted runners -- kind of like jenkins slave nodes)

uses: actions/checkout@v2  (relative path to github.com)

with: clause is how to pass in parameters to uses: clauses (maybe other operations)

runner is just like a jenkins agent node

By DEFAULT every Job spins up a brand new VM runner (this can be good/bad)

Quick edit of files hit "." when in a file

Self-hosted runners
  Recommended to only utilize for private repositories (forked repos could execute code in workflow via PR)


GitHub Actions Storage - Artifacts
  Defaul retention is 90 days (can make it less)
  Artifacts are uploaded during workflow run (can be shared between jobs in the same workflow)
  visible in the UI

Blue checkmark means that GitHub has "verified" a public action (limited, but you still want to look at the code that is being utilized)

action.yml is what allows a code repository to represent an action

Can re-run a workflow within a 30-day time period

Adding a manual way to start workflow:
on:
  workflow_dispatch:
    inputs:
      myValues:
        description: "Input Values"

workflow_dispatch only works on the default branch (generally main or master)

Reference by github.event.inputs.myVersion

Can "pass" environment variables by echo-ing things into a special $GITHUB_ENV variable
-name: Set timestamp
 run: echo TDS=${date} >> $GITHUB_ENV

-name: Tag artifact
 run: echo something-${{ github.event.inputs.myVersion }}${{ env.TDS }}.jar

workflow commands use "::error"  double colon syntax to invoke action, usually with echo

GITHUB_STATE
GITHUB_OUTPUT   (like GITHUB_ENV)

Step requires an id field to allow output to be used from another step

inputs can be specified as required or not for actions.yaml

Can have required workflows that run all repositories within an organization

