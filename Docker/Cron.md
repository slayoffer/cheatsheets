To restart a Docker container on a schedule, you can use a combination of Docker and a task scheduler, such as cron. Here's a step-by-step guide to achieving this:

1. Create a bash script:
   Create a new bash script that contains the command to restart your Docker container. For example, let's assume your container name is `my-container`. Create a file named `restart_container.sh` and add the following content:

   ```bash
   #!/bin/bash
   docker restart my-container
   ```

   Save the file and make it executable by running the following command:
   ```bash
   chmod +x restart_container.sh
   ```

2. Set up a cron job:
   Cron is a time-based job scheduler in Unix-like operating systems. It allows you to schedule tasks to run periodically. You can add a cron job to restart your Docker container based on your desired schedule.

   Open the crontab for editing by running the following command:
   ```bash
   crontab -e
   ```

   This will open the crontab file in your default text editor.

3. Add the cron job entry:
   In the crontab file, add the following line to schedule the restart of your Docker container at 6:10 AM from Monday to Friday:

   ```
   10 6 * * 1-5 /path/to/restart_container.sh
   ```

   Replace `/path/to/restart_container.sh` with the actual path to your `restart_container.sh` script.

4. Save and exit the crontab file:
   After adding the cron job entry, save and exit the crontab file.

   The cron job is now configured to restart your Docker container at the specified schedule. The container will be restarted at 6:10 AM on weekdays (Monday to Friday).