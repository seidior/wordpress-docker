# WordPress Docker Compose project template

This project template will allow you to work on a new or existing WordPress
installation on your local machine without having to install a web server.

## Step 1: Set up Docker Desktop

Install Docker Desktop for your machine, which will install Docker Compose. If
you're running macOS, this is best done with [Homebrew](https://brew.sh). With
Homebrew installed, run the following command to install Docker Desktop:

```bash
brew install --cask docker
```

## Step 2: Clone this repository

Clone this repository by navigating to your project directory (ideally, I'd
recommend `~/Developer`) in the terminal app of your choice:

```bash
mkdir -p ~/Developer
cd ~/Developer
```

then cloning this repository into that folder and changing directory inside it:

```bash
git clone https://github.com/seidior/wordpress-docker.git
cd wordpress-docker
```

## Step 3: Pre-configure WordPress

### New install

To get started with a new WordPress installation, download WordPress to your
machine, then copy the extracted files into the `wordpress/` folder of the
project template. (The `wp-config.php` file that is already there should stay
put and not be overwritten.)

Modify these files with your ideal values:

- Change the password values in the `.env` file to something more distinct.
- Change the "unique keys and salts" section of `wp-config.php` to appropriate
  values by following the instructions in the file, starting at line 67.

### Existing install

These details will need to be filled in at a later date, but the idea here is
to merge your existing `wp-config.php` with the one provided in this template,
then import a backup of the database from your live site into the MySQL/MariaDB
container.

## Step 4: Set up WordPress

Back in your terminal application, with Docker Desktop running in the
background, run this command to start up the containers for running WordPress
locally:

```bash
docker compose build
docker compose up -d
```

Once you see "Container nginx Started" in your terminal, open your WordPress
install in your web browser: http://localhost

Follow the instructions to set up WordPress.

## Step 5: Concluding development

When you want to stop the containers running WordPress, go back to your
terminal and run:

```bash
docker compose down
```

# Caveats

This is intended as a starting point for local development of WordPress. It is
neither a production-optimized setup, nor a full development-optimized
environment. Please do not use this to serve a publicly-accessible website.

If you want direct access to your database, such as from an application like
Sequel Ace or MySQL Workbench, you can change the `docker-compose.yml`
configuration values for the database container to be the following:

```yaml
  mysql:
    image: mariadb:latest
    container_name: db
    env_file: .env
    volumes:
      - mysqldata:/var/lib/mysql
    ports:
      - "3306:3306"
    networks:
      - backend
      - frontend
```

Note: this is generally *highly* inadvisable, as it leaves your database server
accessible on the network. Make this change only on networks you trust.

(An alternate solution would be to run phpMyAdmin, and a sample configuration
for this is provided in the `docker-compose.yml` file, but commented out.
Again, this should only be enabled on networks you trust. If you would like to
run phpMyAdmin, you can comment out these lines, save the file, then run
`docker compose up -d` again. Once it's up, visit it in your web browser at
http://localhost:8080. )
