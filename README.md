# githubactions

steps to create Continuous Integration:
=======================================
1. create a workflow file called "github-actions-demo.yml" in the .github/workflows directory.
2. create a file .github and make a folder/directories workflows

github-actions-demo.yml:
========================
name: GitHub Actions Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
===
jobs:
======
Explore-GitHub-Actions:
=======================
runs-on: ubuntu-latest
steps:
======
- run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
- run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
- run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
- name: Check out repository code
uses: actions/checkout@v4
- run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
- run: echo "🖥️ The workflow is now ready to test your code on the runner."
- name: List files in the repository
run: |
ls ${{ github.workspace }}
- run: echo "🍏 This job's status is ${{ job.status }}."



steps:
======
1. name: name of the github actions
2. on: [push], developers merged the code into a  shared github repository, then this "on" event triggered the code then build,run and tests the code
3. jobs: define the tasks to run
4. uses: actions/checkout@v4 : checkout the github repo code.
5. on:
   workflow_call:, means used to create reusable workflows (means other workflows can call them, like expense-backend/expense-frontend)
6. A called workflow is a reusable workflow
7. caller workflow is the workflow that invokes (calls) the reusable workflow when certain events occur
   jobs:
   call-container-integration-reusable:
   uses: pdevops78/github-reusable-workflows/.github/workflows/ci.yml@main