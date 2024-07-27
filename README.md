# MySQL to PostgreSQL Migration Tool

This Python script migrates data from a MySQL table to a PostgreSQL table. It uses the `pymysql` and `psycopg2` libraries to connect to the respective databases.

## Configuration

To use this script, you will need to configure the following:

* `mydb_opts`: a dictionary containing the connection options for the MySQL database (e.g. `user`, `password`, `host`, `port`, `database`).
* `pgdb_opts`: a dictionary containing the connection options for the PostgreSQL database (e.g. `user`, `password`, `host`, `port`, `database`).
* `table_name`: the name of the table to migrate.

## How it Works

1. The script connects to the MySQL database using the `pymysql` library and retrieves the total number of rows in the specified table.
2. It then connects to the PostgreSQL database using the `psycopg2` library.
3. The script fetches the last inserted row in the PostgreSQL table from a JSON file (`sync_det.json`). If the file does not exist, it assumes that no rows have been migrated yet.
4. It then migrates the data from the MySQL table to the PostgreSQL table in batches of 10,000 rows at a time. For each batch, it:
	* Fetches the next batch of rows from the MySQL table using `LIMIT` and `OFFSET`.
	* Inserts the rows into the PostgreSQL table using a single `INSERT` statement with multiple values.
	* Commits the changes to the PostgreSQL table.
	* Updates the JSON file with the last inserted row.
5. The script repeats step 4 until all rows have been migrated.
6. Finally, it closes the connections to both databases and prints the execution time.

## Usage

1. Clone this repository to your local machine.
```
git clone https://github.com/Arvindh-1211/mysql2postgres
```
2. Pip install the required packages.
```
pip install pymysql psycopg2
```
3. Configure the `mydb_opts`, `pgdb_opts`, `table_name`, and `json_file_path` variables in the script.
4. Run the script.
```
python main.py
```

## Note

This script assumes that the table structure is identical in both MySQL and PostgreSQL. If the table structures differ, you may need to modify the script accordingly.

# Replication
To replicate the tables run a cronjob
1. Edit the crontab
```
crontab -e
```
2. Insert the following command to run the sync once in every hour
```
0 * * * * python3 /path/to/the/dir/main.py
```
Replace `/path/to/the/dir/main.py` with the actual path to your script.