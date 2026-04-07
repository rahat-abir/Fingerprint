# Employee CSV Import

This folder contains the employee CSV import utility:

- [import_employees_csv.php](/mnt/c/fingerprint/attendancedeviceintegration/web/scripts/import_employees_csv.php)

The script is designed to safely import employee CSV files into the configured MySQL database using the same `FP_DB_*` environment variables as the app.

You do not need PHP installed on your host machine if you run the script inside the `jambura_app` Docker container.

<p style="color: red; font-weight: 700;">Danger: treat every import as production-impacting. A wrong command can overwrite live employee data.</p>

<p style="color: red; font-weight: 700;">Team rule from now on: append only. Do not use <code>--replace-existing</code> unless you intentionally decide to do a full replacement and have a fresh backup.</p>

## What The Script Does

- Validates the CSV header and row structure
- Requires these columns:

```csv
employee_id,name,join_date,department,section,designation,nid,unit
```

- Converts `join_date` from `M/D/YYYY` to MySQL `YYYY-MM-DD`
- Allows blank `nid` values and imports them as `NULL`
- Rejects duplicate `employee_id` values in the CSV
- Rejects duplicate nonblank `nid` values in the CSV
- Verifies the target database host and database name before execution
- Can either:
  - dry-run and validate only
  - append rows
  - replace existing employee-related data and then import

## Safe Run Order

Always do a dry run first.

## Dry Run

```bash
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php \
  --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ICML_1.csv \
  --expect-host=mysql-1cdb2fa3-3 \
  --expect-db=ahmed
```

This will:

- read and validate the CSV
- connect to the target DB
- print the connected host, port, DB, and user
- show the import plan
- stop without inserting anything

Important:

- `--expect-host` must match the script output line `host: ...`
- for this environment, the working value was `mysql-1cdb2fa3-3`
- this may be different from the public Aiven DNS hostname

## Recommended Ongoing Workflow

From now on, use the script in append mode only.

1. Dry run the CSV.
2. Confirm the printed `host` and `db` are correct.
3. Run the append import with `--execute`.
4. Never add `--replace-existing` unless you intentionally want a full replacement.

## Append Import

This is the default and recommended mode going forward.

```bash
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php \
  --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ANOTHER_FILE.csv \
  --expect-host=mysql-1cdb2fa3-3 \
  --expect-db=ahmed \
  --execute
```

In append mode:

- existing rows are not deleted
- the script stops if `employee_id` conflicts with an existing employee row
- the script stops if nonblank `nid` conflicts with an existing employee row
- this is the safest mode and should be your normal workflow

## Replace Existing Employee Data And Import

Use this when you want to wipe employee-related data and replace it with the CSV contents.

<p style="color: red; font-weight: 700;">Warning: this mode deletes current employee-related rows before importing. Do not use this for normal imports.</p>

```bash
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php \
  --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ICML_1.csv \
  --expect-host=mysql-1cdb2fa3-3 \
  --expect-db=ahmed \
  --replace-existing \
  --execute
```

This will delete rows from:

- `employee_extra_ids`
- `employee_fingers`
- `scan_logs`
- `employees`

It does **not** delete:

- tables
- columns
- indexes
- foreign keys
- `users`
- `migrations`

## Use A Different Delimiter

If a future file uses semicolons instead of commas:

```bash
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php \
  --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ANOTHER_FILE.csv \
  --delimiter=';' \
  --expect-host=mysql-1cdb2fa3-3 \
  --expect-db=ahmed
```

## CSV Rules For Future Files

Future CSV files should:

- use this exact header:

```csv
employee_id,name,join_date,department,section,designation,nid,unit
```

- keep `join_date` in `M/D/YYYY` format
- avoid duplicate `employee_id`
- avoid duplicate nonblank `nid`
- keep blank `nid` empty, not `0`

## Important Notes

- the `jambura_app` container must be running
- the script runs with the same `FP_DB_*` environment variables as the app container
- check your `.env` carefully before running, because it may point to Aiven
- the safest ongoing mode is append mode without `--replace-existing`
- `--execute` is required for real inserts
- `--expect-host` and `--expect-db` are required when using `--execute`
- without `--replace-existing`, the script performs conflict checks against existing DB rows
- with `--replace-existing`, the script deletes employee-related rows first, then imports the CSV in the same transaction

## Example With Your Current File

```bash
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php \
  --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ICML_1.csv \
  --expect-host=mysql-1cdb2fa3-3 \
  --expect-db=ahmed
```

Then:

```bash
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php \
  --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ICML_1.csv \
  --expect-host=mysql-1cdb2fa3-3 \
  --expect-db=ahmed \
  --execute
```

That second command is now the preferred pattern for future imports because it appends and does not delete existing employee data.
