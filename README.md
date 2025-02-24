# PostgreSQL Configuration and Setup Playbook

This Ansible playbook configures a PostgreSQL server to listen on all interfaces, allows password authentication, ensures proper file permissions, and creates a database user and a database if they do not already exist.

## Requirements

- Ansible 2.9 or higher
- PostgreSQL installed on the target hosts

## Variables

The following variables need to be defined for the playbook to work:

- `db_user`: The database user to be created/updated.
- `db_password`: The password for the database user.
- `db_name`: The name of the database to be created.
- `postgresql_version`: The version of PostgreSQL installed on the target hosts.

These variables can be defined in a separate variables file (e.g., `vars.yml`) and included in the playbook.

## Usage

1. **Define Variables**: Create a file named `vars.yml` with the following content:

    ```yaml
    db_user: ******
    db_password: ******
    db_name: mydatabase
    postgresql_version: ******
    ```

2. **Include Variables in Playbook**: Ensure that the playbook includes the variables file:

    ```yaml
    - name: Configure PostgreSQL to listen on all interfaces
      hosts: dbservers
      become: yes
      vars_files:
        - vars.yml
      ...
    ```

3. **Run the Playbook**: Execute the playbook with the following command:

    ```sh
    ansible-playbook -i your_inventory_file setup_db.yml
    ```

## Playbook Tasks

### Pre-Tasks

- **Install acl package**: Ensures that the `acl` package is installed on the target hosts.

### Tasks

1. **Install PostgreSQL**: Installs PostgreSQL on the target hosts.
2. **Ensure PostgreSQL is running**: Starts the PostgreSQL service if it is not already running.
3. **Configure PostgreSQL to listen on all interfaces**: Updates the `postgresql.conf` file to listen on all network interfaces.
4. **Allow password authentication in pg_hba.conf**: Updates the `pg_hba.conf` file to allow password authentication.
5. **Ensure configuration file have correct permissions**: Ensures that the `postgresql.conf` file has the correct ownership and permissions.
6. **Create database user**: Creates or updates a PostgreSQL user with the specified username and password.
7. **Create database**: Creates a PostgreSQL database with the specified name and owner if it does not already exist.

### Handlers

- **restart postgresql**: Restarts the PostgreSQL service to apply configuration changes.
- **reload postgresql**: Reloads the PostgreSQL service to apply authentication changes.

## Notes

- Ensure that the PostgreSQL service is properly installed and configured on the target hosts before running the playbook.
- The playbook assumes that the `postgres` user has administrative privileges and can perform the necessary tasks.
```` ▋
