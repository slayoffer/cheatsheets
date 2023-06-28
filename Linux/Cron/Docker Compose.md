To create a bash script to run `docker-compose-backup-init.yml` and set up a cron job to run it at 22:00 (10 PM) every day, you would do the following:

1. Create a bash script as described in the previous response. Let's call it `run_docker_compose.sh`:

```bash
#!/bin/bash

# Check if docker-compose is installed
if ! command -v docker-compose &> /dev/null
then
    echo "docker-compose could not be found"
    exit
fi

# Define the file name
FILE="docker-compose-backup-init.yml"

# Check if file exists
if [ ! -f $FILE ]
then
    echo "File $FILE does not exist."
    exit
fi

# Run docker-compose up -d with the file
docker-compose -f $FILE up -d
```

Make the script executable:

```bash
chmod +x run_docker_compose.sh
```

2. Set up the cron job. Cron is a time-based job scheduler in Unix-like operating systems. To edit the crontab file, you'll use the `crontab -e` command. This will open the crontab file in your default text editor. If you're asked to choose an editor and you're not sure which one to pick, `nano` is a good choice for beginners:

```bash
crontab -e
```

3. Once you're in the crontab file, you'll add your cron job. In your case, the cron job will be set to run the `run_docker_compose.sh` script at 22:00 (10 PM) every day. You'll add the following line to the crontab file:

```bash
0 22 * * * /path/to/your/script/run_docker_compose.sh
```

Remember to replace `/path/to/your/script/` with the actual path to your `run_docker_compose.sh` script.

4. Save the crontab file and exit the text editor. If you chose `nano`, you would do this by pressing `Ctrl+X`, then `Y` to confirm that you want to save the changes, and then `Enter` to confirm the file name.

After you've done this, the cron job is set up and will run your Docker Compose file at 22:00 (10 PM) every day.