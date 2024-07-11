# Deploy to Server Action

This GitHub Action deploys files to a remote server using rsync (deleting extraneous files from the destination directory on the remote server by default)and optionally executes remote commands after deployment.

## Usage

Example usage in a workflow:

```yaml
name: Deploy to Standalone Development Server

on:
  push:
    branches:
      - develop

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: Deploy to production server
      uses: alleyinteractive/action-deploy-to-server@develop
      with:
        server_user: 'your-server-username'
        server_ip: 'your-server-ip'
        ssh_private_key: ${{ secrets.SERVER_SSH_KEY }}
        source_directory: './dist'
        destination_directory: '/var/www/html'
        exclude_list: '.git,node_modules,.env'
        remote_commands: |
          composer install --no-dev
          php artisan migrate --force
          php artisan config:cache
```

## Inputs

> Specify using `with` keyword.

### `server_user`

- Specify the username for the remote server.
- Required.

### `server_ip`

- Specify the IP address of the remote server.
- Required.

### `source_directory`

- Specify the source directory to deploy.
- Defaults to `'.'`.

### `destination_directory`

- Specify the destination directory on the remote server (relative to server_user home directory, or you can use an absolute path if you begin with a `/`).
- Required.

### `exclude_list`

- Specify a comma-separated list of files and directories to exclude from sync. These will be written to a temporary file and passed to rsync via `--exclude-from`.
- Defaults to `'.git,node_modules,.env'`.

### `ssh_private_key`

- Specify the SSH private key for authentication.
- Required.

### `remote_commands`

- Specify commands to execute on the remote server after deployment.
- Defaults to `''`.

### `delete_extraneous_files_from_destination`

- Determine whether to delete extraneous files from the destination directory on the remote server (rsync --delete).
- Accepts a boolean string (`'true'` or `'false'`).
- Defaults to `'true'`.

### `dry_run`

- Perform a dry run of the rsync command (rsync --dry-run).
- Accepts a boolean string (`'true'` or `'false'`).
- Defaults to `'false'`.

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Related Projects ❤️

- [alleyinteractive/action-deploy-to-remote-repository](https://github.com/alleyinteractive/action-deploy-to-remote-repository)

## Credits

This project is actively maintained by [Alley Interactive](https://github.com/alleyinteractive).

- [Ben Bolton](https://github.com/benpbolton)
- [All Contributors](../../contributors)

## License

The GNU General Public License (GPL) license. Please see [License File](LICENSE) for more information.