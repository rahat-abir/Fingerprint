# How To Import A New CSV File

This guide explains the easiest and safest way to add a new employee CSV file into the database.

Use this guide whenever you get a new CSV file and want to add its data.

## Before You Start

Please make sure:

1. Your CSV file is inside the `imports` folder.
2. Docker Desktop is running.
3. The `jambura_app` container is running.
4. Your CSV file has these column names:

```csv
employee_id,name,join_date,department,section,designation,nid,unit
```

## Important Rule

For normal use, we will only **append** new data.

That means:

- we add new rows
- we do **not** delete old data
- we do **not** replace everything

## Step 1: Put The CSV File In The Right Place

Example:

```text
imports/NEW_FILE.csv
```

## Step 2: Check The File First

Run this command first.

Replace `NEW_FILE.csv` with your real file name.

```powershell
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/NEW_FILE.csv --expect-host=mysql-1cdb2fa3-3 --expect-db=ahmed
```

This step does **not** import anything yet.

It only checks:

- the file format
- the column names
- the target database
- possible duplicate problems

## Step 3: Read The Output Carefully

Make sure the script shows:

- the correct database host
- the correct database name
- no error message

If there is an error, stop and fix the CSV first.

## Step 4: Import The CSV

If the check was successful, run this command:

```powershell
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/NEW_FILE.csv --expect-host=mysql-1cdb2fa3-3 --expect-db=ahmed --execute
```

This is the normal command you should use in the future.

It will:

- add the new rows
- keep the old data
- stop if the new file conflicts with existing data

## Example Using Your Current File

Check first:

```powershell
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ICML_1.csv --expect-host=mysql-1cdb2fa3-3 --expect-db=ahmed
```

Then import:

```powershell
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ICML_1.csv --expect-host=mysql-1cdb2fa3-3 --expect-db=ahmed --execute
```

## If You Get Another CSV File Later

Use the same 2-step process:

1. check the file
2. import the file

Just change the file name in the command.

Example:

```powershell
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ANOTHER_FILE.csv --expect-host=mysql-1cdb2fa3-3 --expect-db=ahmed
```

```powershell
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/ANOTHER_FILE.csv --expect-host=mysql-1cdb2fa3-3 --expect-db=ahmed --execute
```

## Simple Reminder

Always follow this order:

1. Put the CSV into `imports`
2. Run the check command
3. Confirm there are no errors
4. Run the import command

<p style="color: red; font-size: 22px; font-weight: 800;">Danger Zone: The command below wipes current employee-related data and then imports a new file. Do not use this unless you truly want to remove the old data first.</p>

```powershell
docker exec -it jambura_app php /var/www/html/jambura/attendancedeviceintegration/web/scripts/import_employees_csv.php --file=/var/www/html/jambura/attendancedeviceintegration/web/imports/NEW_FILE.csv --expect-host=mysql-1cdb2fa3-3 --expect-db=ahmed --replace-existing --execute
```
