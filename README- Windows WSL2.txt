# README

## Setup Instructions - Windows

### Prerequisites
- Make sure to enable Virtual Machine & Windows Subsystem for Linux (WSL) features on your Windows machine.
- Ensure that your system is up to date by installing the latest updates and rebooting if necessary.

### Installing WSL2 & Ubuntu 22.04 LTS
1. Install WSL2 & Ubuntu 22.04 LTS from the Microsoft Store.
2. Reboot your system.
3. Open Ubuntu terminal and update the system:
    ```bash
    sudo apt update && sudo apt -y full-upgrade
    [ -f /var/run/reboot-required ] && sudo reboot -f
    sudo reboot
    ```

### Adding PostgreSQL-13
1. Update package list and install necessary packages:
    ```bash
    sudo apt update
    sudo apt install curl gpg gnupg2 software-properties-common apt-transport-https lsb-release ca-certificates
    ```
2. Add PostgreSQL repository and install PostgreSQL-13:
    ```bash
    curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
    echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
    sudo apt update
    sudo apt install postgresql-13 postgresql-client-13 libpq-dev
    ```
3. Create a PostgreSQL user and set a password:
    ```bash
    sudo -u postgres createuser name -s
    sudo -u postgres psql -c "ALTER USER postgres PASSWORD '<new-password>';"
    ```

### Installing ASDF (Version Manager)
1. Clone ASDF repository and configure it:
    ```bash
    cd ~
    git clone https://github.com/asdf-vm/asdf.git ~/.asdf
    echo '. "$HOME/.asdf/asdf.sh"' >> ~/.bashrc
    echo '. "$HOME/.asdf/completions/asdf.bash"' >> ~/.bashrc
    echo 'legacy_version_file = yes' >> ~/.asdfrc
    echo 'export EDITOR="code --wait"' >> ~/.bashrc
    exec $SHELL
    ```
2. Add ASDF plugins for Ruby and Node.js:
    ```bash
    asdf plugin add ruby
    asdf plugin add nodejs
    ```

### Installing Ruby, Node.js, Rails, and Redis
1. Install Ruby 3.1.1 and set it as the global version:
    ```bash
    asdf install ruby 3.1.1
    asdf global ruby 3.1.1
    gem update --system
    ```
2. Install Node.js 20.9.0 and set it as the global version:
    ```bash
    asdf install nodejs 20.9.0
    asdf global nodejs 20.9.0
    npm install -g yarn
    ```
3. Install Rails 7.1.3:
    ```bash
    gem install rails -v 7.1.3
    ```
4. Install Redis:
    ```bash
    curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
    echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
    sudo apt-get update
    sudo apt-get install redis
    ```

### Setup Project
1. Update and upgrade system packages:
    ```bash
    sudo apt update
    sudo apt upgrade
    ```
2. Install dependencies in root folder:
    ```bash
    cp -a .env.example .env
    bundle install
    yarn install
    rails db:create
    rails db:create
    
    ```

### Troubleshooting


#### If encountering errors related to database configuration:
- Clean bundle and install again:
    ```bash
    bundle clean --force
    bundle install
    ```
- If prompted for a password in `database.yml`, set pasword for user postgres and use it in your conection string:
    ```bash
    sudo -u postgres psql
    postgres=# \password
    ```
In .env file DATABASE_URL="postgres://postgres:postgres_password@127.0.0.1:5432"

#### If facing permission denied errors:
- Exit the folder and adjust permissions:
    ```bash
    chmod -R 775 folder_name
    ```

#### If encountering "cannot load client/packs/app-bundle.js:Zone.Identifier" error:
- Locate Zone.Identifier files and delete their contents.

