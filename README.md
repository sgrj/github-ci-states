# Testing GitHub CI states

This is a repository that you can use to experiment with different values for
GitHub [statuses](https://docs.github.com/en/rest/reference/repos#statuses)
and [check-runs](https://docs.github.com/en/rest/reference/checks)
on a pull request.
They are controlled with two files.
1. `statuses.json` contains an array of json objects that are turned into
   statuses on a pull request. Each object contains a `context`, a `state`,
   and possibly a `min_runtime_in_seconds`. When the pull request is created
   or updated, a GitHub Action creates a *status* for this object with the
   given `name` and a `pending` state. If `min_runtime_in_seconds` is specified,
   the action sleeps for this amount and then sets the state to the `state`
   taken from the json file.

1. `check-runs.json` contains an array of json objects that are turned into
   check runs on a pull request. Each object contains a `name`, a `conclusion`,
   and a possible `min_runtime_in_seconds`. When the pull request is created or updated, a
   GitHub Action creates a *check-run* for this object with the given `name`
   and a status `in_progress`. If `min_runtime_in_seconds` is specified, the
   action sleeps for this amount and then sets the status to `completed`, with
   the `conclusion` taken from the json file.

## Example

If a pull request is created with `statuses.json` set to
```
[
  {
    "context": "Some status",
    "min_runtime_in_seconds": 15,
    "state": "success"
  }
]
```
and `check-runs.json` set to
```
[
  {
    "name": "Some check run",
    "min_runtime_in_seconds": 30,
    "conclusion": "failure"
  }
]
```
the pipeline will initially contain one pending state and one check-run that is
in progress. After 15 seconds, the status is marked as a `success`, and after 30
seconds the check-run is marked as `failure`.

## Try it out!

You can create a fork of this repository and create a pull request against this
repository to try it out yourself!
