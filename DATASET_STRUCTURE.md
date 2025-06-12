# TravisTorrent Dataset Structure

## Directory Layout

```
travistorrent-java-ci-build-dataset/
|
|-- <ProjectName@Repo>/
|    |-- repo-data-travis.csv       # Build event summary per project
|    |-- buildlog-data-travis.csv   # Detailed job/build logs per project
```

## repo-data-travis.csv

| build_id | commit | pull_req | branch | status | duration | started_at | jobs | event_type |
|----------|--------|----------|--------|--------|----------|------------|------|------------|
| ...      | ...    | ...      | ...    | ...    | ...      | ...        | ...  | ...        |

## buildlog-data-travis.csv

| tr_build_id | tr_job_id | tr_build_number | tr_original_commit | tr_log_lan | tr_log_status | ... | tr_log_buildduration |
|-------------|-----------|-----------------|--------------------|------------|---------------|-----|---------------------|
| ...         | ...       | ...             | ...                | ...        | ...           | ... | ...                 |

- Each row = one build or job.
- Each project = one directory with two main CSVs.

## Notes

- The structure allows for easy project-level or cross-project analysis.
- Column names are consistent so you can join/merge files.
- Not all columns may be filled for all languages/projects.
